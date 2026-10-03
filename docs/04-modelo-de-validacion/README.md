# Modelo de validación — Catálogo de EUV

Los **Escenarios de Uso y Validación (EUV)** representan situaciones significativas que demuestran el comportamiento del producto y se validan con evidencia. Hay dos tipos:

- **Funcionales (EUV-NN):** demuestran el comportamiento de un caso de uso (C.U), incluidos sus flujos alternativos y de excepción.
- **De calidad (EUV-Q-NN):** demuestran un RNF en formato QAS (fuente, estímulo, artefacto, entorno, respuesta, medida) y verifican los mecanismos de diseño.

Los resultados de ejecución se consolidan en [Ejecución y evidencia](../05-ejecucion-y-evidencia/02-lectura-integrada.md).

## EUV funcionales
| ID | Escenario | C.U | RF | Actor | Estado | Evidencia |
|----|-----------|-----|----|-------|--------|-----------|
| [EUV-01](euv/EUV-01-iniciar-sesion.md) | Iniciar sesión | Transversal | RF-01 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (paso 1) |
| [EUV-02](euv/EUV-02-consultar-convocatorias.md) | Consultar convocatorias y su detalle | [C.U-02](../02-modelo-de-capacidades/casos-de-uso/C.U-02-consultar-convocatoria.md) | RF-02, RF-03 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (paso 2) |
| [EUV-03](euv/EUV-03-verificar-elegibilidad.md) | Verificar elegibilidad | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | RF-04 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (paso 3) |
| [EUV-04](euv/EUV-04-cargar-documentos.md) | Cargar documentos a S3 | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | RF-05, RF-06 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (pasos 4–6) |
| [EUV-05](euv/EUV-05-registrar-postulacion.md) | Registrar solicitud | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | RF-07 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (paso 7), prueba k6 |
| [EUV-06](euv/EUV-06-consultar-postulaciones.md) | Consultar mis solicitudes y su detalle | [C.U-04](../02-modelo-de-capacidades/casos-de-uso/C.U-04-consultar-solicitud.md) | RF-08, RF-09 | Residente | 🟢 Desarrollado | `test_create_application_end_to_end` (paso 8, parcial) |

## EUV de calidad
| ID | RNF | Atributo | Escenario | Prioridad | Estado | Decisión |
|----|-----|----------|-----------|-----------|--------|----------|
| [EUV-Q-01](euv/EUV-Q-01-concurrencia-postulaciones.md) | RNF-01 | Rendimiento | 300 solicitudes concurrentes | (H, H) | 🟡 Parcial | ADR-0001, ADR-0005 |
| [EUV-Q-02](euv/EUV-Q-02-acceso-no-autenticado.md) | RNF-02 | Seguridad | Acceso sin token a la API | (H, M) | 🟢 | ADR-0003 |
| [EUV-Q-03](euv/EUV-Q-03-postulacion-duplicada.md) | RNF-03 | Integridad | Solicitud duplicada de la misma unidad privada | (H, M) | 🟡 Ante solicitudes simultáneas responde 500 ([R-06](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) | ADR-0005 |
| [EUV-Q-04](euv/EUV-Q-04-despliegue-automatizado.md) | RNF-04 | Desplegabilidad | Merge de `develop` a `main` despliega automáticamente | (M, M) | 🟢 | ADR-0006 |
| [EUV-Q-05](euv/EUV-Q-05-carga-documentos-s3.md) | RNF-05 | Escalabilidad | Carga de archivos sin pasar por la API | (M, M) | 🟢 | ADR-0004 |

## Flujo completo del residente
> ✏️ **Diagrama por agregar:** `![Flujo completo del residente](../assets/imagenes/escenarios-flujo-residente.png)`

Para agregar un EUV usar las plantillas [euv](../plantillas/euv.md) o [euv-calidad](../plantillas/euv-calidad.md).
