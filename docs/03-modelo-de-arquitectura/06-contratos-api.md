# Contratos de la API REST

Base: `/api` · Formato: JSON `ApiResponse<T>` · Especificación viva: `/swagger-ui.html`.

> ✏️ Recomendado: exportar `/v3/api-docs` a `assets/documentos/openapi.json` en cada entrega.

## Resumen de endpoints
| Método | Ruta | Auth | Descripción | Request | Response `data` | RF |
|--------|------|:----:|-------------|---------|-----------------|----|
| POST | `/api/auth/login` | ❌ | Iniciar sesión | `LoginRequestDTO {email, password}` | `LoginResponseDTO {token, type, expiresIn, email, name, roles, userId, apartmentNumber, towerName}` (los tres últimos son `null` si el usuario no tiene unidad privada) | RF-01 |
| GET | `/api/health` | ❌ | Salud del servicio | — | texto `UP` | — |
| GET | `/api/db` | ✅ | Prueba de conexión a BD | — | `Map` | — |
| GET | `/api/hola` | ✅ | Endpoint de prueba | — | texto | — |
| GET | `/api/calls` | ✅ | Listar convocatorias | — | `List<CallDTO>` | RF-02 |
| GET | `/api/calls/{callId}` | ✅ | Detalle de convocatoria | — | `CallDTO` | RF-03 |
| GET | `/api/applications/check?callId=` | ✅ | Elegibilidad (usuario tomado del JWT) | — | `{canApply, existingApplication}` | RF-04 |
| POST | `/api/applications` | ✅ | Registrar solicitud | `ApplicationRequestDTO {callId, documentIds[]}` (el residente se toma del JWT) | `ApplicationResponseDTO` (número `POS-{convocatoria}-{consecutivo}`, RN-39) | RF-07 |
| GET | `/api/applications` | ✅ | Listar las solicitudes del usuario del JWT (paginado) | `page, size, sortBy, direction` | `PageResponseDTO<ApplicationResponseDTO>` | RF-08 |
| GET | `/api/applications/{applicationId}` | ✅ | Detalle (⚠️ no valida que la solicitud sea del residente, R-01) | — | `ApplicationDetailResponseDTO` | RF-09 |
| GET | `/api/documents/types` | ✅ | Catálogo de tipos de documento | — | `List<DocumentTypeResponseDTO>` | RF-05 |
| POST | `/api/documents/presigned-url` | ✅ | Solicitar URL de carga | `{fileName, contentType, size, documentType}` | `{documentId, fileName, uploadUrl}` | RF-06 |
| POST | `/api/documents/{documentId}/complete` | ✅ | Confirmar carga | — | `DocumentResponseDTO` | RF-06 |
| PATCH | `/api/applications/{applicationId}/cancel` | ✅ | Cancelar la solicitud propia mientras la convocatoria está abierta (RN-26 a RN-28) | — | `ApplicationResponseDTO` | RF-14 |
| POST · PUT · PATCH · DELETE | `/api/calls`, `/api/calls/{callId}`, `/close`, `/reopen` | 🔒 Admin | Crear, modificar, cerrar, reabrir y eliminar convocatorias ([C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md)) | `CallRequestDTO {title, description, startDate, endDate, totalSlots}` | `CallDTO` | RF-10 |
| GET | `/api/admin/applications` | 🔒 Admin | Listar solicitudes para revisión ([C.U-07](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md)) | `callId?, status?, page, size` (máx. 100) | `PageResponseDTO<AdminApplicationResponseDTO>` | RF-15 |
| GET | `/api/admin/applications/{applicationId}` · `/documents/{documentId}/url` | 🔒 Admin | Detalle de la solicitud y URL de lectura de un documento | — | `AdminApplicationDetailResponseDTO` · `{url}` | RF-16 |
| PATCH | `/api/applications/{applicationId}/accept` · `/reject` | 🔒 Admin | Aprobar o rechazar (motivo obligatorio) | `{reason}` al rechazar | `AdminApplicationResponseDTO` | RF-16, RF-17 |
| GET · POST | `/api/calls/{callId}/draw/preview`, `/api/calls/{callId}/draw` | 🔒 Admin | Resumen previo, ejecución y auditoría del sorteo ([C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md)) | — | `DrawPreviewDTO` · `DrawResultDTO` | RF-12, RF-13 |
| GET | `/api/calls/{callId}/draw/results` · `/hash` | ✅ | Resultado del sorteo y huella de integridad. El administrador recibe todas las filas; el residente, solo las de sus solicitudes (R-11) | — | `DrawResultsDTO` · `IntegrityHashDTO` | RF-13 |
| GET | `/api/admin/calls/{callId}/draw/export` | 🔒 Admin | Exportar el resultado en CSV | — | CSV | RF-13 |

## Ejemplos
### Solicitar URL prefirmada
```http
POST /api/documents/presigned-url
Authorization: Bearer eyJ...
Content-Type: application/json

{
  "fileName": "soat.pdf",
  "contentType": "application/pdf",
  "size": 734003,
  "documentType": "SOAT_VIGENTE"
}
```
```json
{
  "message": "URL generada correctamente",
  "data": {
    "documentId": "3f6c…",
    "fileName": "soat.pdf",
    "uploadUrl": "https://aes-puj-documents.s3.amazonaws.com/documents/3f6c…/soat.pdf?X-Amz-…"
  },
  "error": null
}
```

### Registrar solicitud
```http
POST /api/applications
Authorization: Bearer eyJ...
Content-Type: application/json

{ "callId": 1, "documentIds": ["3f6c…", "a91b…", "c02d…"] }
```
Respuesta exitosa: `200 OK` con `ApplicationResponseDTO`.

## Versionado y compatibilidad
✏️ Definir la política (p. ej. prefijo `/api/v1`, cambios compatibles hacia atrás, deprecación con aviso).
