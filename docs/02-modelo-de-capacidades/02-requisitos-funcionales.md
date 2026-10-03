# Requisitos funcionales implementados

> **Nota:** esta tabla registra los requisitos implementados en v1. En PAREX las capacidades se expresan como casos de uso (C.U); la correspondencia RF ↔ C.U está en el [mapa de casos de uso](01-mapa-de-casos-de-uso.md).

| ID | Requisito | Prioridad | Endpoint / Pantalla | Escenario | Estado |
|----|-----------|-----------|---------------------|-----------|--------|
| RF-01 | Autenticar al residente con correo y contraseña y emitir un token | Alta | `POST /api/auth/login` · `/login` | [EUV-01](../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md) | 🟢 |
| RF-02 | Listar las convocatorias | Alta | `GET /api/calls` · `/convocations` | [EUV-02](../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) | 🟢 |
| RF-03 | Mostrar el detalle de una convocatoria | Media | `GET /api/calls/{callId}` · `/convocations/:id` | EUV-02 | 🟢 |
| RF-04 | Verificar si el residente puede presentar una solicitud a una convocatoria | Alta | `GET /api/applications/check` (el frontend no lo invoca; la validación se aplica al registrar) | [EUV-03](../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) | 🟢 |
| RF-05 | Exponer el catálogo de tipos de documento (obligatoriedad, tamaño, formato) | Media | `GET /api/documents/types` | [EUV-04](../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) | 🟢 |
| RF-06 | Cargar documentos PDF (≤ 2 MB) a S3 | Alta | `POST /api/documents/presigned-url` · `POST /api/documents/{documentId}/complete` | EUV-04 | 🟢 |
| RF-07 | Registrar una solicitud validando documentos obligatorios y unicidad por unidad privada | Alta | `POST /api/applications` · `/convocations/:id/apply` | [EUV-05](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | 🟢 |
| RF-08 | Listar las solicitudes del residente, paginadas | Media | `GET /api/applications` · `/mis-postulaciones` | [EUV-06](../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | 🟢 |
| RF-09 | Mostrar el detalle de una solicitud con sus documentos | Media | `GET /api/applications/{applicationId}` · `/applications/:id` | EUV-06 | 🟢 |
| RF-10 | Publicar convocatorias con cupos y periodo de vigencia ([C.U-01](casos-de-uso/C.U-01-publicar-convocatoria.md)) | Alta (RBS, TJ-02) | — | — | 🔴 |
| RF-11 | Validar que el residente esté a paz y salvo antes de registrar la solicitud ([C.U-03](casos-de-uso/C.U-03-registrar-solicitud.md), RN-16) | ✏️ | — | — | 🔴 |

## Historias de usuario
En PAREX las historias de usuario se sustituyen por los casos de uso (C.U) y sus EUV; ver el [mapa de casos de uso](01-mapa-de-casos-de-uso.md) y el [catálogo de EUV](../04-modelo-de-validacion/README.md).
