# C.U-02 · Consultar convocatoria

| Campo | Valor |
|-------|-------|
| Paquete | Solicitudes |
| Actor primario | Residente |
| Actores secundarios | — |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 3 |
| Objetivo del actor | Conocer las convocatorias de parqueadero comunal y sus condiciones para decidir si presenta una solicitud |
| Resultado observable | Listado de convocatorias y detalle de la convocatoria seleccionada (vigencia, cupos y estado) |
| Requisitos funcionales relacionados | RF-02, RF-03 |
| Estado | 🟢 Implementado |

> **Fuente de los flujos:** implementación v1 (`ConvocationsPage`, `ConvocationDetailPage`, `ResidentDrawResultsPage`, `GET /api/calls`, `GET /api/calls/{callId}`). Revisión del código de la rama `fix/integracion-aprobacion-sorteo`: 2026-11-09.

## Precondiciones
1. El residente inició sesión con `ROLE_RESIDENT` ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)). El administrador no tiene acceso a las opciones del residente ([RN-40](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## Postcondiciones
1. El residente conoce las condiciones de la convocatoria; si está abierta, puede iniciar [C.U-03](C.U-03-registrar-solicitud.md).

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Ingresa a la opción de convocatorias | 🟢 `/convocations` |
| 2 | Sistema | Muestra las convocatorias con título, vigencia, cupos y estado | 🟢 `GET /api/calls` |
| 3 | Residente | Selecciona una convocatoria | 🟢 |
| 4 | Sistema | Muestra el detalle de la convocatoria con ayuda contextual sobre cupos y estado | 🟢 `GET /api/calls/{callId}`, componente `Tooltip` |
| 5 | Sistema | Habilita la opción de presentar una solicitud. Fin | 🟢 Habilitada si el estado derivado es `OPEN`; antes de continuar consulta la elegibilidad (`GET /api/applications/check`) |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 4 | El residente desea ver otra convocatoria | Regresa al listado de convocatorias | Paso 2 |
| FA-02 | 4 | La convocatoria ya fue sorteada | El sistema muestra *Ver resultados del sorteo*; el residente consulta el resultado y la posición **de sus propias solicitudes** (no ve a los demás participantes) y la huella de integridad del resultado completo ([C.U-08](C.U-08-ejecutar-sorteo-de-cupos.md)) | Fin |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Estado v1 |
|--------|:----:|-----------|-----------------------|-----------|
| FE-01 | 2 | No hay convocatorias registradas | Muestra "No hay convocatorias disponibles." Fin | 🟢 |
| FE-02 | 4 | La convocatoria no existe (`CALL_NOT_FOUND`) | Muestra un mensaje de error. Fin | 🟢 |
| FE-03 | 5 | La convocatoria no está abierta (`SCHEDULED` o `CLOSED`) | No habilita la opción de presentar una solicitud. Fin | 🟢 Tanto la lista como el detalle usan el estado derivado ([RN-25](../../01-modelo-de-negocio/04-reglas-de-negocio.md)). En la lista, *Ver Detalles* se deshabilita en las convocatorias cerradas, salvo las ya sorteadas, para que el residente pueda consultar los resultados |
| FE-04 | 2, 4 | El token no es válido o expiró (401) | Muestra un mensaje de error; no redirige al inicio de sesión | 🟡 `authenticatedFetch` no trata el 401. Ver [RNF-02](../03-atributos-de-calidad-y-rnf.md#diagramas-de-secuencia-de-rnf) |
| FE-05 | 5 | La unidad privada ya tiene una solicitud vigente en la convocatoria (`canApply = false`) | No abre el formulario y muestra el mensaje del backend. Fin | 🟢 `GET /api/applications/check` ([C.U-03](C.U-03-registrar-solicitud.md), FE-02) |
| FE-06 | 1 | El usuario es administrador | El menú no muestra *Convocatorias* y, si entra por URL a `/convocations`, al detalle o a los resultados del sorteo del residente, lo redirige al inicio. El administrador consulta las convocatorias en *Administrar Convocatorias* ([C.U-01](C.U-01-publicar-convocatoria.md)). Fin | 🟢 `Navbar`, `ProtectedRoute requiredRole="ROLE_RESIDENT"` (RN-40) |

## Reglas de negocio aplicables
[RN-01](../../01-modelo-de-negocio/04-reglas-de-negocio.md) · [RN-14](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar) (cupos por convocatoria) · [RN-25](../../01-modelo-de-negocio/04-reglas-de-negocio.md) (estado derivado).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Consulta solo con JWT válido |
| RNF-07 Usabilidad | Ayuda contextual sobre cupos y estado |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| [EUV-02](../../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) | Flujo básico, FE-02 | Implementado |
| [EUV-03](../../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) | FE-05 | Implementado |
| ✏️ EUV por crear · Resultados del sorteo (Residente) | FA-02 | Pendiente |
