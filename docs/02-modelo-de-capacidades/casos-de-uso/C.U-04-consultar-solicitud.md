# C.U-04 · Consultar solicitud

> **Nota:** el diagrama asocia este C.U a ambos actores: el Residente consulta sus solicitudes y el Administrador consulta las solicitudes de una convocatoria. Los flujos siguientes corresponden al Residente; la consulta del Administrador se implementó como parte de la revisión de solicitudes ([C.U-07](C.U-07-aceptar-solicitud.md), pasos 1 a 5). El administrador no tiene la opción *Mis Postulaciones* (RN-40).

| Campo | Valor |
|-------|-------|
| Paquete | Solicitudes · Convocatorias |
| Actor primario | Residente, Administrador |
| Actores secundarios | — |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | — |
| Objetivo del actor | Conocer el estado de sus solicitudes y los documentos asociados |
| Resultado observable | Listado de solicitudes y detalle de la solicitud seleccionada (número, convocatoria, estado, fecha y documentos) |
| Requisitos funcionales relacionados | RF-08, RF-09 (Residente) |
| Estado | 🟡 Implementado para Residente |

> **Fuente de los flujos:** implementación v1 (`ApplicationsPage`, `ApplicationDetailPage`, `GET /api/applications`, `GET /api/applications/{applicationId}`). Revisión del código de la rama `fix/integracion-aprobacion-sorteo`: 2026-11-09.

## Precondiciones
1. El residente inició sesión con `ROLE_RESIDENT` ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)). El administrador no tiene acceso a las opciones del residente ([RN-40](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## Postcondiciones
1. El residente conoce el estado de sus solicitudes. No se modifica información.

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Ingresa a la opción de sus solicitudes | 🟢 `/mis-postulaciones` |
| 2 | Sistema | Muestra las solicitudes del residente, de la más reciente a la más antigua | 🟡 `GET /api/applications`. El backend limita siempre el listado al usuario del token e ignora `residentId`. Muestra las 10 más recientes: la interfaz no tiene controles de paginación |
| 3 | Residente | Selecciona una solicitud | 🟢 |
| 4 | Sistema | Muestra el detalle de la solicitud con su número, convocatoria, estado, fecha y documentos; si fue rechazada, el motivo ([RN-38](../../01-modelo-de-negocio/04-reglas-de-negocio.md)); si quedó en lista de espera tras el sorteo, su posición ([RN-20](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)). Fin | 🟡 `GET /api/applications/{applicationId}`; ⚠️ no valida que la solicitud pertenezca al residente: conociendo el identificador, un residente puede ver la solicitud de otro ([R-01](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 4 | El residente desea ver otra solicitud | Regresa al listado de solicitudes | Paso 2 |
| FA-02 | 4 | La solicitud está `PENDING_VALIDATION` y la convocatoria está abierta (dentro de su periodo y sin cierre manual) | Ofrece *Cancelar postulación* | [C.U-06](C.U-06-cancelar-solicitud.md) |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Estado v1 |
|--------|:----:|-----------|-----------------------|-----------|
| FE-01 | 2 | El residente no tiene solicitudes | Informa que no hay solicitudes y ofrece ir a las convocatorias. Fin | 🟢 |
| FE-02 | 4 | La solicitud no existe (`404`) | Muestra un mensaje de error. Fin | 🟢 |
| FE-03 | 2, 4 | El token no es válido o expiró (401) | Muestra un mensaje de error; no redirige al inicio de sesión | 🟡 |
| FE-04 | 2 | El usuario del token no existe (`404 USER_NOT_FOUND`) | Muestra un mensaje de error. Fin | 🟢 |
| FE-05 | 1 | El usuario es administrador | El menú no muestra *Mis Postulaciones* y, si entra por URL, lo redirige al inicio. Fin | 🟢 `Navbar`, `ProtectedRoute requiredRole="ROLE_RESIDENT"` (RN-40) |

## Reglas de negocio aplicables
[RN-13](../../01-modelo-de-negocio/04-reglas-de-negocio.md) (estado inicial `PENDING_VALIDATION`).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Consulta solo con JWT válido. El listado se restringe al residente del token; falta restringir el detalle (R-01) |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| [EUV-06](../../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | Flujo básico (paso 4 en la prueba de extremo a extremo) | Implementado parcialmente |
