# C.U-04 · Consultar solicitud

> **Nota:** el diagrama asocia este C.U a ambos actores: el Residente consulta sus solicitudes y el Administrador consulta las solicitudes de una convocatoria. Los flujos siguientes corresponden al Residente; la consulta del Administrador no está implementada en v1.

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

> **Fuente de los flujos:** implementación v1 (`ApplicationsPage`, `ApplicationDetailPage`, `GET /api/applications`, `GET /api/applications/{applicationId}`).

## Precondiciones
1. El residente inició sesión ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).

## Postcondiciones
1. El residente conoce el estado de sus solicitudes. No se modifica información.

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Ingresa a la opción de sus solicitudes | 🟢 `/mis-postulaciones` |
| 2 | Sistema | Muestra las solicitudes del residente, de la más reciente a la más antigua | 🟡 `GET /api/applications` (10 por página, sin controles de paginación en la interfaz). ⚠️ Hoy lista todas las solicitudes, no solo las del residente ([R-01](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) |
| 3 | Residente | Selecciona una solicitud | 🟢 |
| 4 | Sistema | Muestra el detalle de la solicitud con su estado y documentos. Fin | 🟡 `GET /api/applications/{applicationId}`; no valida que la solicitud pertenezca al residente (R-01) |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 4 | El residente desea ver otra solicitud | Regresa al listado de solicitudes | Paso 2 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Estado v1 |
|--------|:----:|-----------|-----------------------|-----------|
| FE-01 | 2 | El residente no tiene solicitudes | Informa que no hay solicitudes y ofrece ir a las convocatorias. Fin | 🟢 |
| FE-02 | 4 | La solicitud no existe | Muestra un mensaje de error. Fin | 🟢 |
| FE-03 | 2, 4 | El token no es válido o expiró (401) | Muestra un mensaje de error; no redirige al inicio de sesión | 🟡 |

## Reglas de negocio aplicables
[RN-13](../../01-modelo-de-negocio/04-reglas-de-negocio.md) (estado inicial `REGISTERED`).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Consulta solo con JWT válido; falta restringir a las solicitudes del residente (R-01) |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| [EUV-06](../../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | Flujo básico (paso 4 en la prueba de extremo a extremo) | Implementado parcialmente |
