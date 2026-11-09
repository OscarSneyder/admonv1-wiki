# C.U-06 · Cancelar solicitud

> **Alcance:** la [RBS](../../06-direccion-y-planeacion/02-rbs.md) lo difiere, pero ya está implementado en v1 (commit *feat: cancel application implementation*, 2026-10-02).

| Campo | Valor |
|-------|-------|
| Paquete | Solicitudes |
| Actor primario | Residente |
| Actores secundarios | — |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | — |
| Objetivo del actor | Retirar su solicitud de una convocatoria mientras la convocatoria sigue abierta |
| Resultado observable | Solicitud en estado `CANCELLED`, visible en el detalle de la solicitud |
| Requisitos funcionales relacionados | RF-14 |
| Estado | 🟢 Implementado |

> **Fuente de los flujos:** implementación v1. Backend: `PATCH /api/applications/{applicationId}/cancel` (`ApplicationServiceImpl.cancelApplication`). Frontend: botón *Cancelar postulación* y modal de confirmación en `/applications/:id`. Revisión del código de `develop`: 2026-11-07.

## Precondiciones
1. El residente inició sesión ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).
2. El residente tiene una solicitud en estado `PENDING_VALIDATION`.
3. La fecha actual está dentro del periodo de la convocatoria.

## Postcondiciones
1. La solicitud queda en estado `CANCELLED`.
2. La unidad privada puede volver a presentar una solicitud en la misma convocatoria, porque [RN-04](../../01-modelo-de-negocio/04-reglas-de-negocio.md) no cuenta las solicitudes `CANCELLED`. Para permitirlo, este mismo commit retiró la restricción única `uk_application_apartment_call` (ver [R-09](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)).

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Consulta el detalle de su solicitud ([C.U-04](C.U-04-consultar-solicitud.md)) | 🟢 `/applications/:id` |
| 2 | Sistema | Muestra la opción *Cancelar postulación* si la solicitud está `PENDING_VALIDATION` y la convocatoria está dentro de su periodo | 🟢 |
| 3 | Residente | Elige cancelar | 🟢 |
| 4 | Sistema | Pide confirmación | 🟢 Modal de confirmación |
| 5 | Residente | Confirma | 🟢 |
| 6 | Sistema | Valida la solicitud ([RN-26 a RN-28](../../01-modelo-de-negocio/04-reglas-de-negocio.md)) y la marca `CANCELLED` | 🟢 `PATCH /api/applications/{applicationId}/cancel` |
| 7 | Sistema | Muestra la solicitud con el estado actualizado. Fin | 🟢 |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 5 | El residente no confirma | Cierra el modal sin cambios | Paso 1 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Regla | Estado v1 |
|--------|:----:|-----------|-----------------------|-------|-----------|
| FE-01 | 6 | La solicitud no existe | Rechaza (`APPLICATION_NOT_FOUND`). Fin | — | 🟢 |
| FE-02 | 6 | La solicitud pertenece a otro residente | Rechaza (`APPLICATION_FORBIDDEN`). Fin | RN-26 | 🟢 El residente se toma del JWT |
| FE-03 | 6 | La solicitud no está `PENDING_VALIDATION` | Rechaza (`APPLICATION_CANNOT_BE_CANCELLED`). Fin | RN-27 | 🟢 |
| FE-04 | 6 | La fecha actual está fuera del periodo de la convocatoria | Rechaza (`APPLICATION_PERIOD_CLOSED`). Fin | RN-28 | 🟢 |

## Reglas de negocio aplicables
RN-04 y RN-26 a RN-28 ([Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | El residente se toma del JWT; solo el dueño cancela su solicitud (FE-02) |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| ✏️ EUV por crear · Cancelar solicitud | Flujo básico, FE-02 a FE-04 | Pendiente; `test_api.py` no cubre la cancelación |
