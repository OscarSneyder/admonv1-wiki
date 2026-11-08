# Riesgos y deuda técnica

Hallazgos identificados al revisar el código. Cada uno debe tener responsable y fecha objetivo.

## Riesgos
| ID | Riesgo | Evidencia | Probabilidad | Impacto | Mitigación propuesta | Responsable | Estado |
|----|--------|-----------|:---:|:---:|----------------------|-------------|--------|
| R-01 | **Suplantación y exposición de solicitudes.** Creación: resuelto, el backend toma el residente del JWT e ignora `residentId`. Consulta: `GET /api/applications` no filtra por el residente autenticado (el frontend envía `residentId = 1` fijo desde `constants.js`, así que cualquier usuario ve las solicitudes del residente 1; sin ese parámetro el API devuelve todas) y `GET /api/applications/{id}` no valida que la solicitud pertenezca al residente | `ApplicationController`, `ApplicationServiceImpl.getApplications/getApplication`, `useApplications`, `constants.js` | Alta | Alto | Filtrar listado y detalle por el residente del JWT; retirar `residentId` del DTO y la constante `RESIDENT` del frontend | ✏️ | 🟡 Parcial |
| R-02 | Consola H2 pública y `frameOptions` deshabilitado | `SecurityConfig` ("temporalmente para desarrollo") | Media | Alto | Deshabilitar por perfil (`spring.profiles`) en producción | ✏️ | 🔴 |
| R-03 | Pérdida de datos: H2 en memoria en el despliegue | `application.properties`, ADR-0005 | Alta | Alto | Migrar a PostgreSQL (RDS) + Flyway | ✏️ | 🔴 |
| R-04 | Sin autorización por rol | `SecurityConfig` | Media | Medio | `@PreAuthorize` / `requestMatchers(...).hasRole(...)` | ✏️ | 🟡 Parcial: solo `ROLE_ADMIN` escribe convocatorias (`SecurityConfig`) y el frontend protege las rutas `/admin`; la consulta de solicitudes no se restringe por rol ni por dueño |
| R-05 | Tasa de error del 16,7 % en la prueba de carga | EUV-Q-01 | ✏️ | Alto | Analizar la causa (¿datos de prueba?) y repetir con usuarios semilla suficientes | ✏️ | 🟡 |
| R-06 | Excepción por restricción única traducida a 500 en lugar de 409 | EUV-Q-03 | Media | Medio | `@ControllerAdvice` para `DataIntegrityViolationException` | ✏️ | ⚪ Ya no aplica: la restricción única se retiró el 2026-10-02, así que ya no se produce el 500, pero ahora pueden quedar duplicados (ver R-09) |
| R-07 | Token en `localStorage` (XSS) | `TokenStorage` | Baja | Alto | CSP estricta o cookie `HttpOnly` | ✏️ | 🟡 |
| R-08 | Indisponibilidad breve durante el despliegue *in-place* (reemplazo del contenedor) | [ADR-0007](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md) | Alta | Bajo | Riesgo aceptado para el alcance actual; evolución a Load Balancer + Blue/Green si aumentan los requisitos de disponibilidad | ✏️ | 🟢 Aceptado |
| R-09 | **Solicitudes duplicadas.** El commit de cancelación (`0c9c2fb`, 2026-10-02) retiró la restricción única `uk_application_apartment_call`, y la validación de RN-04 solo cuenta las solicitudes `REGISTERED`. Dos solicitudes simultáneas de la misma unidad pueden quedar registradas, y una solicitud `PENDING_VALIDATION`, `APPROVED` o `REJECTED` no impide crear otra | `Application.java`, `ApplicationServiceImpl.createApplication` | Media | Alto | Índice único parcial sobre las solicitudes no canceladas, o bloqueo por unidad y convocatoria; definir qué estados bloquean una solicitud nueva | ✏️ | 🔴 Confirmado por revisión de código (2026-11-07) |
| R-10 | **`develop` del backend no compila.** El commit `b9a98a1` (2026-11-04) usa `getReviewedAt()` y `getRejectionReason()`, que no existen en la entidad `Application`; además agrega un límite por cupos al registrar solicitudes, contrario al modelo de sorteo ([C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md)) | `ApplicationServiceImpl` | Alta | Alto | Completar el commit con los campos de la entidad y revisar la validación de cupos antes de integrarlo en `main` | ✏️ | 🔴 Confirmado al compilar (2026-11-07) |

## Matriz de riesgos
> ✏️ **Diagrama por agregar:** `![Matriz de riesgos](../assets/imagenes/riesgos-matriz.png)`

## Deuda técnica
| ID | Deuda | Ubicación | Esfuerzo | Prioridad |
|----|-------|-----------|----------|-----------|
| D-01 | Una sola prueba API de extremo a extremo ([flujo de 8 pasos](../04-modelo-de-validacion/04-automatizacion-de-euv.md#flujo-de-integración-del-pipeline)), solo camino exitoso; sin pruebas unitarias significativas | `api-tests/`, `src/test` | M | Alta |
| D-02 | `ddl-auto=create` en todos los entornos | `application.properties` | S | Alta |
| D-03 | Scripts y resultados de k6 dentro de `src/main/java` | `loadtest/` | S | Baja |
| D-04 | Los servicios devuelven `ResponseEntity` (capa de negocio acoplada a HTTP) | `service/*Impl` | M | Media |
| D-05 | `ConvocationDetailPage` importa infraestructura de la feature `applications` (`checkApplicationEligibility`). Estaba marcada como resuelta, pero sigue presente en `develop` (revisión 2026-11-07) | frontend | S | Media |
| D-06 | Datos de residente de ejemplo fijos en `constants.js`; además, `RESIDENT.id` se usa para filtrar *Mis postulaciones* (R-01) | frontend | S | Alta |
| D-07 | Endpoint de prueba `/api/hola` y `/api/db` expuestos | `HealthController` | S | Baja |
| D-08 | Metadatos vacíos en `pom.xml` (`description`, `scm`, `developers`) | backend | S | Baja |
| D-09 | Carpetas generadas versionadas o presentes (`dist/`, `target/`, `graphify-out/`) | ambos | S | Baja |
| D-10 | `GET /api/db` responde `"baseDatos": "PostgreSQL"` fijo aunque la base de datos es H2 | `HealthController` | S | Baja |
