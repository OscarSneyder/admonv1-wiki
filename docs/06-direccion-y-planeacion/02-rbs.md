# Realización Básica del Servicio (RBS)

> **Propósito:** definir el alcance mínimo, de extremo a extremo, que demuestra el servicio de negocio dentro del timebox y el budgetbox.

| Campo | Valor |
|-------|-------|
| Servicio de negocio | [SN-06](../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | [ESN-01](../01-modelo-de-negocio/06-escenarios-de-servicio.md) Reserva de parqueadero comunal |
| Criterio de completitud | Una convocatoria recorre todo el ciclo: publicación, solicitudes, validación, sorteo y asignación, con evidencia |

## C.U y flujos incluidos
| C.U | Flujos incluidos | Flujos diferidos | Tajada |
|-----|------------------|------------------|--------|
| [C.U-01 Publicar convocatoria](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | Básico (cupos y periodo parametrizables) | ✏️ | TJ-02 |
| [C.U-02 Consultar convocatoria](../02-modelo-de-capacidades/casos-de-uso/C.U-02-consultar-convocatoria.md) | Básico | — | Implementado |
| [C.U-03 Registrar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | Básico, FA-01 a FA-03 y excepciones FE-02 a FE-10 | ✏️ FE-01 paz y salvo (por decidir) | Implementado (residente desde el JWT) |
| [C.U-04 Consultar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-04-consultar-solicitud.md) | Básico (Residente) | Consulta del Administrador | Implementado; TJ-01 restringe la consulta al residente (R-01) |
| [C.U-07 Aceptar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md) | Básico (aceptar / rechazar) | ✏️ | TJ-02 |
| [C.U-08 Ejecutar sorteo de cupos](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | Básico | ✏️ | TJ-03 |

Diferidos fuera de la RBS: C.U-05 Ajustar solicitud, C.U-06 Cancelar solicitud, C.U-09 Configurar zona común.

## EUV seleccionados
| EUV | Motivo de inclusión |
|-----|---------------------|
| [EUV-02](../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) a [EUV-06](../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | Demuestran los C.U implementados |
| [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) | Pico de concurrencia en cada apertura de convocatoria |
| [EUV-Q-02](../04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md), [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) | Seguridad y unicidad de la solicitud |
| ✏️ EUV por crear para C.U-01, C.U-07 y C.U-08 | Completar la demostración de extremo a extremo |

## RNF que debe cumplir la RBS
| RNF | Criterio ajustado a la RBS |
|-----|----------------------------|
| RNF-01 Rendimiento | p95 < 2 s y errores < 1 % con 300 usuarios concurrentes en la apertura de convocatoria |
| RNF-02 Seguridad | 100 % de rechazo sin token; el residente se toma del JWT en creación y consulta (R-01) |
| RNF-03 Integridad | 0 solicitudes duplicadas por unidad privada y convocatoria |
| RNF-04 Desplegabilidad | Despliegue automatizado con verificación de salud |
| RNF-05 Escalabilidad | Los archivos no pasan por el backend |
