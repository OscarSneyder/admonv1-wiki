# Requisitos funcionales implementados

> **Nota:** esta tabla registra los requisitos implementados en v1. En PAREX las capacidades se expresan como casos de uso (C.U); la correspondencia RF ↔ C.U está en el [mapa de casos de uso](01-mapa-de-casos-de-uso.md).

| ID | Requisito | Prioridad | Endpoint / Pantalla | Escenario | Estado |
|----|-----------|-----------|---------------------|-----------|--------|
| RF-01 | Autenticar al residente con correo y contraseña y emitir un token | Alta | `POST /api/auth/login` · `/login` | [EUV-01](../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md) | 🟢 |
| RF-02 | Listar las convocatorias | Alta | `GET /api/calls` · `/convocations` | [EUV-02](../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) | 🟢 |
| RF-03 | Mostrar el detalle de una convocatoria | Media | `GET /api/calls/{callId}` · `/convocations/:id` | EUV-02 | 🟢 |
| RF-04 | Verificar si el residente puede presentar una solicitud a una convocatoria | Alta | `GET /api/applications/check` (el frontend lo invoca desde el detalle de la convocatoria; la validación se repite al registrar) | [EUV-03](../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) | 🟢 |
| RF-05 | Exponer el catálogo de tipos de documento (obligatoriedad, tamaño, formato) | Media | `GET /api/documents/types` | [EUV-04](../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) | 🟢 |
| RF-06 | Cargar documentos PDF (≤ 2 MB) a S3 | Alta | `POST /api/documents/presigned-url` · `POST /api/documents/{documentId}/complete` | EUV-04 | 🟢 |
| RF-07 | Registrar una solicitud validando documentos obligatorios y unicidad por unidad privada | Alta | `POST /api/applications` · `/convocations/:id/apply` | [EUV-05](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | 🟢 |
| RF-08 | Listar las solicitudes del residente, paginadas | Media | `GET /api/applications` · `/mis-postulaciones` | [EUV-06](../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | 🟡 Lista las solicitudes del usuario autenticado (el backend filtra por el token). La interfaz muestra las 10 más recientes y no tiene controles de paginación |
| RF-09 | Mostrar el detalle de una solicitud con sus documentos y, si fue rechazada, el motivo (RN-38) | Media | `GET /api/applications/{applicationId}` · `/applications/:id` | EUV-06 | 🟢 |
| RF-10 | Publicar convocatorias con cupos y periodo de vigencia ([C.U-01](casos-de-uso/C.U-01-publicar-convocatoria.md)) | Alta (RBS, TJ-02) | `POST/PUT/DELETE /api/calls`, `PATCH /api/calls/{callId}/close` y `/reopen` · `/admin/convocations` | ✏️ EUV por crear | 🟡 Falta el periodo de vigencia (RN-15) |
| RF-11 | Validar que el residente esté a paz y salvo antes de registrar la solicitud ([C.U-03](casos-de-uso/C.U-03-registrar-solicitud.md), RN-16) | ✏️ | — | — | 🔴 |
| RF-12 | Ejecutar el sorteo de cupos de una convocatoria cerrada entre las solicitudes aprobadas, una sola vez, o asignarlos directamente si hay cupos para todas ([C.U-08](casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md), RN-17 a RN-20) | Alta (RBS, TJ-03) | `GET /api/calls/{callId}/draw/preview`, `POST /api/calls/{callId}/draw` · `/admin/convocations/:id/draw` | ✏️ EUV por crear | 🟢 |
| RF-13 | Consultar el resultado del sorteo (seleccionados y lista de espera en orden) y su registro de auditoría ([C.U-08](casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md), RN-21) | Alta (RBS, TJ-03) | `GET /api/calls/{callId}/draw`, `/draw/results`, `/draw/hash`, `GET /api/admin/calls/{callId}/draw/export` · `/admin/calls/:id/draw/results`, `/calls/:id/draw/results` | ✏️ EUV por crear | 🟢 |
| RF-14 | Cancelar una solicitud `PENDING_VALIDATION` dentro del periodo de la convocatoria ([C.U-06](casos-de-uso/C.U-06-cancelar-solicitud.md), RN-26 a RN-28) | Media | `PATCH /api/applications/{applicationId}/cancel` · `/applications/:id` | ✏️ EUV por crear | 🟢 |
| RF-15 | Listar y filtrar las solicitudes para su revisión ([C.U-07](casos-de-uso/C.U-07-aceptar-solicitud.md), RN-30) | Alta (RBS, TJ-02) | `GET /api/admin/applications` · `/admin/applications` | ✏️ EUV por crear | 🟢 Paginado de 20 en 20, con filtros por convocatoria y estado (D-11 resuelto el 2026-11-09) |
| RF-16 | Consultar el detalle de una solicitud y previsualizar sus documentos ([C.U-07](casos-de-uso/C.U-07-aceptar-solicitud.md), RN-30, RN-35) | Alta (RBS, TJ-02) | `GET /api/admin/applications/{applicationId}`, `GET /api/admin/applications/{applicationId}/documents/{documentId}/url` · `/admin/applications/:id` | ✏️ EUV por crear | 🟢 |
| RF-17 | Aprobar o rechazar una solicitud y revocar una aprobación, con registro de auditoría ([C.U-07](casos-de-uso/C.U-07-aceptar-solicitud.md), RN-31 a RN-34) | Alta (RBS, TJ-02) | `PATCH /api/applications/{applicationId}/accept`, `PATCH /api/applications/{applicationId}/reject` · `/admin/applications/:id` | ✏️ EUV por crear | 🟢 |

## Historias de usuario
En PAREX las historias de usuario se sustituyen por los casos de uso (C.U) y sus EUV; ver el [mapa de casos de uso](01-mapa-de-casos-de-uso.md) y el [catálogo de EUV](../04-modelo-de-validacion/README.md).
