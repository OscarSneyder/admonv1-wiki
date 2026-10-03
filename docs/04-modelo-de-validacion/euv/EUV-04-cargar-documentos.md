# EUV-04 · Cargar documentos a S3

| Campo | Valor |
|-------|-------|
| Requisitos | RF-05, RF-06 |
| Actor | Residente autenticado |
| Pantalla | `/convocations/:id/apply` → `CreateApplicationPage` (`DocumentUpload`) |
| Endpoints | `GET /api/documents/types` · `POST /api/documents/presigned-url` · `PUT <URL prefirmada S3>` · `POST /api/documents/{documentId}/complete` |
| Decisión | [ADR-0004 · Carga directa a S3 con URL prefirmada](../../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0004-carga-directa-a-s3.md) |
| Componentes | Front: `useDocumentTypes`, `useDocumentUpload`, `UploadDocument`, `HttpDocumentRepository`, `documents.api` · Back: `DocumentController`, `DocumentServiceImpl`, `S3Config` (`S3Presigner`, `S3Client`), `DocumentRepository` |

## Flujo principal (patrón *upload en tres pasos*)
> ✏️ **Diagrama por agregar:** `![Flujo de carga de documentos](../../assets/imagenes/esc-f-04-cargar-documentos.png)`

## Validaciones
| Validación | Código |
|-----------|--------|
| Tipo fuera del catálogo | `INVALID_DOCUMENT_TYPE` |
| Formato distinto de PDF | `INVALID_CONTENT_TYPE` |
| Tamaño > 2 MB | `FILE_TOO_LARGE` |
| Documento inexistente al completar | `DOCUMENT_NOT_FOUND` |
| Objeto no encontrado en S3 | `FILE_NOT_FOUND_IN_S3` |

## Criterios de aceptación
- **Dado** un PDF de 1 MB, **cuando** el residente lo carga, **entonces** el documento queda `UPLOADED`.
- **Dado** un archivo `.jpg`, **cuando** se solicita la URL, **entonces** se rechaza con `INVALID_CONTENT_TYPE`.
- ✏️ Qué ocurre si la URL expira antes de subir (> 5 min).
