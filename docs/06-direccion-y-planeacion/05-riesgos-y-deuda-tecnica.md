# Riesgos y deuda técnica

Hallazgos identificados al revisar el código. Cada uno debe tener responsable y fecha objetivo.

## Riesgos
| ID | Riesgo | Evidencia | Probabilidad | Impacto | Mitigación propuesta | Responsable | Estado |
|----|--------|-----------|:---:|:---:|----------------------|-------------|--------|
| R-01 | **Suplantación y exposición de solicitudes.** Creación: resuelto, el backend toma el residente del JWT e ignora `residentId`. Consulta: `GET /api/applications` no filtra por el residente autenticado (el frontend no envía `residentId`, por lo que se listan todas las solicitudes) y `GET /api/applications/{id}` no valida que la solicitud pertenezca al residente | `ApplicationController`, `ApplicationServiceImpl.getApplications/getApplication`, `useApplications`, `constants.js` | Alta | Alto | Filtrar listado y detalle por el residente del JWT; retirar `residentId` del DTO y la constante `RESIDENT` del frontend | ✏️ | 🟡 Parcial |
| R-02 | Consola H2 pública y `frameOptions` deshabilitado | `SecurityConfig` ("temporalmente para desarrollo") | Media | Alto | Deshabilitar por perfil (`spring.profiles`) en producción | ✏️ | 🔴 |
| R-03 | Pérdida de datos: H2 en memoria en el despliegue | `application.properties`, ADR-0005 | Alta | Alto | Migrar a PostgreSQL (RDS) + Flyway | ✏️ | 🔴 |
| R-04 | Sin autorización por rol | `SecurityConfig` | Media | Medio | `@PreAuthorize` / `requestMatchers(...).hasRole(...)` | ✏️ | 🔴 |
| R-05 | Tasa de error del 16,7 % en la prueba de carga | EUV-Q-01 | ✏️ | Alto | Analizar la causa (¿datos de prueba?) y repetir con usuarios semilla suficientes | ✏️ | 🟡 |
| R-06 | Excepción por restricción única traducida a 500 en lugar de 409 | EUV-Q-03 | Media | Medio | `@ControllerAdvice` para `DataIntegrityViolationException` | ✏️ | 🔴 Confirmado por revisión de código: no hay manejador de `DataIntegrityViolationException` ([RNF-03](../assets/imagenes/rnf-03-unicidad-solicitud.png)) |
| R-07 | Token en `localStorage` (XSS) | `TokenStorage` | Baja | Alto | CSP estricta o cookie `HttpOnly` | ✏️ | 🟡 |
| R-08 | Indisponibilidad breve durante el despliegue *in-place* (reemplazo del contenedor) | [ADR-0007](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md) | Alta | Bajo | Riesgo aceptado para el alcance actual; evolución a Load Balancer + Blue/Green si aumentan los requisitos de disponibilidad | ✏️ | 🟢 Aceptado |

## Matriz de riesgos
> ✏️ **Diagrama por agregar:** `![Matriz de riesgos](../assets/imagenes/riesgos-matriz.png)`

## Deuda técnica
| ID | Deuda | Ubicación | Esfuerzo | Prioridad |
|----|-------|-----------|----------|-----------|
| D-01 | Una sola prueba API de extremo a extremo ([flujo de 8 pasos](../04-modelo-de-validacion/04-automatizacion-de-euv.md#flujo-de-integración-del-pipeline)), solo camino exitoso; sin pruebas unitarias significativas | `api-tests/`, `src/test` | M | Alta |
| D-02 | `ddl-auto=create` en todos los entornos | `application.properties` | S | Alta |
| D-03 | Scripts y resultados de k6 dentro de `src/main/java` | `loadtest/` | S | Baja |
| D-04 | Los servicios devuelven `ResponseEntity` (capa de negocio acoplada a HTTP) | `service/*Impl` | M | Media |
| D-05 | ✅ Resuelta: `ConvocationDetailPage` importaba infraestructura de la feature `applications` | frontend | S | Media |
| D-06 | Datos de residente de ejemplo fijos en `constants.js` | frontend | S | Alta |
| D-07 | Endpoint de prueba `/api/hola` y `/api/db` expuestos | `HealthController` | S | Baja |
| D-08 | Metadatos vacíos en `pom.xml` (`description`, `scm`, `developers`) | backend | S | Baja |
| D-09 | Carpetas generadas versionadas o presentes (`dist/`, `target/`, `graphify-out/`) | ambos | S | Baja |
| D-10 | `GET /api/db` responde `"baseDatos": "PostgreSQL"` fijo aunque la base de datos es H2 | `HealthController` | S | Baja |
