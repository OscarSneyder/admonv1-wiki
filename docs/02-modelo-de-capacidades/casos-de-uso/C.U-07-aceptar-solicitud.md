# C.U-07 · Aceptar solicitud

> **Alcance:** incluido en la RBS (tajada TJ-02). Usa los estados `PENDING_VALIDATION`, que es el estado inicial de toda solicitud ([RN-13](../../01-modelo-de-negocio/04-reglas-de-negocio.md)), `APPROVED` y `REJECTED`.

| Campo | Valor |
|-------|-------|
| Paquete | Convocatorias |
| Actor primario | Administrador |
| Actores secundarios | Residente (consulta el estado de su solicitud y el motivo del rechazo en [C.U-04](C.U-04-consultar-solicitud.md)) |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 6 |
| Objetivo del actor | Validar manualmente los documentos que cada residente cargó en su solicitud y decidir cuáles solicitudes participan en el sorteo de cupos |
| Resultado observable | Solicitud en estado `APPROVED` (participa en el sorteo) o `REJECTED` (con su motivo), con el registro de quién la revisó y cuándo |
| Requisitos funcionales relacionados | RF-15, RF-16, RF-17 |
| Estado | 🟢 Implementado (falta la notificación al residente, fuera del alcance) |

> **Fuente de los flujos:** implementación v1 (CU 1.2.4 en el código). Backend: `AdminApplicationServiceImpl`, `AdminApplicationController`, `ApplicationController` y `DocumentServiceImpl.generateReadUrl`. Frontend: `AdminApplicationsPage`, `AdminApplicationDetailPage` y `AdminRejectModal`. Revisión del código de la rama `fix/integracion-aprobacion-sorteo`: 2026-11-09. Los comentarios del backend citan estas reglas con otra numeración; la de referencia es la de esta wiki.

## Precondiciones
1. El administrador inició sesión con `ROLE_ADMIN` ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).
2. Existe una solicitud en estado `PENDING_VALIDATION` (para aprobarla o rechazarla) o `APPROVED` (para revocar la aprobación).
3. La convocatoria de la solicitud no tiene un sorteo ejecutado ([RN-34](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

La revisión se puede hacer con la convocatoria abierta o cerrada, pero debe terminar antes del sorteo: mientras haya solicitudes `PENDING_VALIDATION`, el sorteo no se puede ejecutar ([RN-37](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## Postcondiciones
1. Una solicitud aprobada queda en `APPROVED` y participará en el sorteo si conserva ese estado al ejecutarlo ([C.U-08](C.U-08-ejecutar-sorteo-de-cupos.md), [RN-17](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
2. Una solicitud rechazada o revocada queda en `REJECTED`, no participa en el sorteo y el residente puede consultar el motivo ([RN-38](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).
3. La revisión registra el administrador, la fecha y hora (zona America/Bogota) y, al rechazar, el motivo ([RN-33](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).
4. Los cupos de la convocatoria no cambian ([modelo de cupos](../../01-modelo-de-negocio/04-reglas-de-negocio.md#modelo-de-cupos)).
5. Si la solicitud se rechaza mientras la convocatoria está abierta, la unidad privada puede registrar una solicitud nueva ([RN-04](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## Criterio de aceptación
> **Decisión del equipo (2026-11-09):** una solicitud se acepta, principalmente, cuando el administrador valida de forma manual los documentos que el residente cargó en ella ([RN-36](../../01-modelo-de-negocio/04-reglas-de-negocio.md)). El sistema verifica al registrar la solicitud que los documentos obligatorios estén cargados, pero no evalúa su contenido.

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Administrador | Ingresa a *Revisar Postulaciones* | 🟢 `/admin/applications`, visible solo para `ROLE_ADMIN` |
| 2 | Sistema | Muestra las solicitudes, de la más reciente a la más antigua, con número, estado, residente, unidad privada, convocatoria, cantidad de documentos, motivo del rechazo y resultado del sorteo, paginadas de 20 en 20 (*Mostrando 21–40 de N*, botones *Anterior* y *Siguiente*) | 🟢 `GET /api/admin/applications?page=&size=`. Filtros y página se guardan en la URL (`?callId=&status=&page=`), así que al volver del detalle se conserva la posición. D-11 resuelto (2026-11-09) |
| 3 | Administrador | Elige *Ver detalle* en una solicitud | 🟢 `/admin/applications/:id`. Las acciones de revisión solo están en el detalle |
| 4 | Sistema | Muestra los datos de la solicitud, la convocatoria, el residente, la unidad privada, los documentos y la auditoría de la revisión | 🟢 `GET /api/admin/applications/{applicationId}` |
| 5 | Administrador | Previsualiza y valida los documentos del vehículo y del residente ([RN-36](../../01-modelo-de-negocio/04-reglas-de-negocio.md)) | 🟢 `GET /api/admin/applications/{applicationId}/documents/{documentId}/url`; se abre en una pestaña nueva ([RN-35](../../01-modelo-de-negocio/04-reglas-de-negocio.md)) |
| 6 | Administrador | Elige *Aceptar* | 🟢 Botón habilitado solo si la solicitud está `PENDING_VALIDATION` y la convocatoria no se ha sorteado; no pide confirmación |
| 7 | Sistema | Valida la operación ([RN-31](../../01-modelo-de-negocio/04-reglas-de-negocio.md), [RN-34](../../01-modelo-de-negocio/04-reglas-de-negocio.md)), marca la solicitud `APPROVED` y registra la auditoría | 🟢 `PATCH /api/applications/{applicationId}/accept` |
| 8 | Sistema | Regresa al listado de solicitudes, con los filtros y la página desde los que se abrió el detalle. Fin | 🟢 |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno | Estado v1 |
|--------|:--------------:|-----------|--------|---------|-----------|
| FA-01 | 6 | El administrador decide rechazar una solicitud `PENDING_VALIDATION` | Elige *Rechazar* e ingresa el motivo, que es obligatorio y el residente podrá consultar. Al confirmar, el sistema marca la solicitud `REJECTED` y registra la auditoría | Paso 8 | 🟢 `PATCH /api/applications/{applicationId}/reject` |
| FA-02 | 6 | El administrador decide revocar una solicitud `APPROVED` | Sigue el mismo flujo de FA-01: la solicitud pasa de `APPROVED` a `REJECTED` y deja de participar en el sorteo | Paso 8 | 🟢 Mismo endpoint |
| FA-03 | FA-01, FA-02 | El administrador no confirma el rechazo | Cierra el modal sin cambios | Paso 4 | 🟢 |
| FA-04 | 2 | El administrador filtra el listado por estado o por convocatoria | El sistema muestra las solicitudes que cumplen los filtros, desde la primera página, o, si no hay ninguna, informa que no hay postulaciones | Paso 2 | 🟢 La lista de convocatorias se carga de `GET /api/calls`. Si la página guardada ya no existe (por ejemplo, tras aprobar con un filtro activo), se muestra la última disponible |
| FA-05 | 4 | La solicitud ya no se puede revisar, por su estado o porque la convocatoria se sorteó | El sistema muestra el detalle con las acciones deshabilitadas | Paso 2 | 🟢 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Regla | Estado v1 |
|--------|:----:|-----------|-----------------------|-------|-----------|
| FE-01 | 4, 7, FA-01, FA-02 | La solicitud no existe | Rechaza (`404 APPLICATION_NOT_FOUND`). Fin | — | 🟢 |
| FE-02 | 7 | La solicitud no está `PENDING_VALIDATION` al aprobarla | Rechaza (`409 APPLICATION_INVALID_STATE`). Fin | RN-31 | 🟢 |
| FE-03 | FA-01, FA-02 | La solicitud no está `PENDING_VALIDATION` ni `APPROVED` al rechazarla | Rechaza (`409 APPLICATION_INVALID_STATE`). Fin | RN-31 | 🟢 |
| FE-04 | 7, FA-01, FA-02 | La convocatoria ya fue sorteada | Rechaza (`409 CALL_ALREADY_DRAWN`). Fin | RN-34 | 🟢 |
| FE-05 | FA-01, FA-02 | El motivo del rechazo supera los 500 caracteres | Rechaza (`400 REJECTION_REASON_TOO_LONG`). Retorna al modal | RN-32 | 🟢 El formulario limita la longitud a 500 caracteres |
| FE-06 | 7, FA-01, FA-02 | Otra operación modificó la solicitud o no se pudo obtener el bloqueo | Rechaza (`409 APPLICATION_CONCURRENT_UPDATE`) y pide intentar de nuevo. Fin | RNF-03 | 🟢 `GlobalExceptionHandler` |
| FE-07 | 1 a 7 | El usuario no tiene `ROLE_ADMIN` | Rechaza la petición (`403`; `401` sin token válido). Fin | RN-30 | 🟢 `SecurityConfig` y ruta protegida en el frontend |
| FE-08 | 2 | El filtro de convocatoria o de estado no es válido | Rechaza (`400 INVALID_CALL_ID` o `400 INVALID_STATUS`). Fin | — | 🟢 |
| FE-09 | 4, 7, FA-01, FA-02 | El identificador de la solicitud no es válido | Rechaza (`400 INVALID_APPLICATION_ID` en la consulta; `400 INVALID_REQUEST` en la revisión). Fin | — | 🟢 |
| FE-10 | 5 | El documento no existe o no pertenece a la solicitud | Rechaza (`404 DOCUMENT_NOT_FOUND` o `404 DOCUMENT_NOT_IN_APPLICATION`) y muestra un error junto al documento | RN-35 | 🟢 |
| FE-11 | 5 | El archivo no existe en S3 | Rechaza (`404 FILE_NOT_FOUND_IN_S3`) e informa que el archivo no está disponible | RN-35 | 🟢 |
| FE-12 | 5 | El almacenamiento no está disponible | Rechaza (`503 STORAGE_UNAVAILABLE`) y pide intentar de nuevo | — | 🟢 |
| FE-13 | FA-01, FA-02 | Falta el motivo del rechazo | Rechaza (`400 REJECTION_REASON_REQUIRED`). Retorna al modal | RN-32 | 🟢 El botón de confirmar se habilita solo con un motivo |

## Ciclo de vida de la solicitud en la revisión
```text
(registro, C.U-03) ─────► PENDING_VALIDATION
PENDING_VALIDATION ──(aceptar)──► APPROVED ──(sorteo, C.U-08)──► SELECTED o NOT_SELECTED
PENDING_VALIDATION ──(rechazar)─► REJECTED
APPROVED ────────────(revocar)──► REJECTED
```
`REJECTED` es un estado final: una solicitud rechazada no se puede volver a aprobar. Mientras la convocatoria esté abierta, la unidad privada puede registrar una solicitud nueva.

## Reglas de negocio aplicables
RN-04, RN-13, RN-17, RN-30 a RN-38 y [modelo de cupos](../../01-modelo-de-negocio/04-reglas-de-negocio.md#modelo-de-cupos) ([Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Solo `ROLE_ADMIN` lista y revisa solicitudes (FE-07); el administrador se toma del JWT |
| RNF-03 Integridad | Cada revisión es una transacción. Bloquea primero la convocatoria y luego la solicitud, en el mismo orden que el sorteo, y la solicitud tiene control de versión (FE-06) |
| ✏️ RNF por definir · Auditabilidad | Cada revisión conserva quién, cuándo y el motivo (RN-33) |

## Fuera del alcance de este C.U
- **Notificar la decisión al residente:** depende de [MEC-05](../../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md), que no está implementado.
- **Validación automática de los documentos:** la decisión es del administrador; el sistema no comprueba el contenido ni la vigencia de los documentos.
- **Paz y salvo del residente:** [RN-16](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar), por implementar.

## Puntos por confirmar
| Punto | Comportamiento actual |
|-------|-----------------------|
| Confirmación al aprobar | Rechazar pide confirmación; aprobar se ejecuta de inmediato |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| ✏️ EUV por crear · Revisar solicitudes | Flujo básico, FA-01 a FA-05, FE-01 a FE-13 | Pendiente. `DrawIntegrationTest` cubre aceptar, rechazar, revocar sin cambiar los cupos, FE-04, FE-13 y la consulta del motivo por el residente; no cubre los demás flujos de excepción, el listado ni la previsualización |
| ✏️ EUV-Q por crear · Revisión concurrente con el sorteo | RNF-03, FE-06 | Pendiente. `DrawConcurrencyTest` comprueba que el sorteo espera el bloqueo de la convocatoria |
