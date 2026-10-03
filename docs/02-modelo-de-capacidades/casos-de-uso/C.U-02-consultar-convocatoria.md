# C.U-02 · Consultar convocatoria

| Campo | Valor |
|-------|-------|
| Paquete | Solicitudes |
| Actor primario | Residente |
| Actores secundarios | — |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 3 |
| Objetivo del actor | Conocer las convocatorias de parqueadero comunal y sus condiciones para decidir si presenta una solicitud |
| Resultado observable | Listado de convocatorias y detalle de la convocatoria seleccionada (vigencia, cupos disponibles y estado) |
| Requisitos funcionales relacionados | RF-02, RF-03 |
| Estado | 🟢 Implementado |

> **Fuente de los flujos:** implementación v1 (`ConvocationsPage`, `ConvocationDetailPage`, `GET /api/calls`, `GET /api/calls/{callId}`).

## Precondiciones
1. El residente inició sesión ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).

## Postcondiciones
1. El residente conoce las condiciones de la convocatoria; si tiene cupos disponibles, puede iniciar [C.U-03](C.U-03-registrar-solicitud.md).

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Ingresa a la opción de convocatorias | 🟢 `/convocations` |
| 2 | Sistema | Muestra las convocatorias con título, vigencia, cupos y estado | 🟢 `GET /api/calls` |
| 3 | Residente | Selecciona una convocatoria | 🟢 |
| 4 | Sistema | Muestra el detalle de la convocatoria con ayuda contextual sobre cupos y estado | 🟢 `GET /api/calls/{callId}`, componente `Tooltip` |
| 5 | Sistema | Habilita la opción de presentar una solicitud. Fin | 🟢 Habilitada si `availableSlots > 0` |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 4 | El residente desea ver otra convocatoria | Regresa al listado de convocatorias | Paso 2 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Estado v1 |
|--------|:----:|-----------|-----------------------|-----------|
| FE-01 | 2 | No hay convocatorias registradas | Muestra "No hay convocatorias disponibles." Fin | 🟢 |
| FE-02 | 4 | La convocatoria no existe (`CALL_NOT_FOUND`) | Muestra un mensaje de error. Fin | 🟢 |
| FE-03 | 5 | La convocatoria no tiene cupos disponibles | No habilita la opción de presentar una solicitud. Fin | 🟡 El estado `OPEN/CLOSED` no se evalúa |
| FE-04 | 2, 4 | El token no es válido o expiró (401) | Muestra un mensaje de error; no redirige al inicio de sesión | 🟡 Ver [RNF-02](../03-atributos-de-calidad-y-rnf.md#diagramas-de-secuencia-de-rnf) |

## Reglas de negocio aplicables
[RN-01](../../01-modelo-de-negocio/04-reglas-de-negocio.md) · [RN-14](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar) (cupos por convocatoria).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Consulta solo con JWT válido |
| RNF-07 Usabilidad | Ayuda contextual sobre cupos y estado |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| [EUV-02](../../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) | Flujo básico, FE-02 | Implementado |
