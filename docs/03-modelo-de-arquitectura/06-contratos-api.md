# Contratos de la API REST

Base: `/api` · Formato: JSON `ApiResponse<T>` · Especificación viva: `/swagger-ui.html`.

> ✏️ Recomendado: exportar `/v3/api-docs` a `assets/documentos/openapi.json` en cada entrega.

## Resumen de endpoints
| Método | Ruta | Auth | Descripción | Request | Response `data` | RF |
|--------|------|:----:|-------------|---------|-----------------|----|
| POST | `/api/auth/login` | ❌ | Iniciar sesión | `LoginRequestDTO {email, password}` | `LoginResponseDTO` | RF-01 |
| GET | `/api/health` | ❌ | Salud del servicio | — | texto `UP` | — |
| GET | `/api/db` | ✅ | Prueba de conexión a BD | — | `Map` | — |
| GET | `/api/hola` | ✅ | Endpoint de prueba | — | texto | — |
| GET | `/api/calls` | ✅ | Listar convocatorias | — | `List<CallDTO>` | RF-02 |
| GET | `/api/calls/{callId}` | ✅ | Detalle de convocatoria | — | `CallDTO` | RF-03 |
| GET | `/api/applications/check?callId=` | ✅ | Elegibilidad (usuario tomado del JWT) | — | `{canApply, existingApplication}` | RF-04 |
| POST | `/api/applications` | ✅ | Registrar solicitud | `ApplicationRequestDTO {callId, residentId, documentIds[]}` (`residentId` se ignora; el residente se toma del JWT) | `ApplicationResponseDTO` | RF-07 |
| GET | `/api/applications` | ✅ | Listar (paginado) | `residentId?, page, size, sortBy, direction` (⚠️ sin `residentId` lista todas las solicitudes, R-01) | `PageResponseDTO<ApplicationResponseDTO>` | RF-08 |
| GET | `/api/applications/{applicationId}` | ✅ | Detalle (⚠️ no valida que la solicitud sea del residente, R-01) | — | `ApplicationDetailResponseDTO` | RF-09 |
| GET | `/api/documents/types` | ✅ | Catálogo de tipos de documento | — | `List<DocumentTypeResponseDTO>` | RF-05 |
| POST | `/api/documents/presigned-url` | ✅ | Solicitar URL de carga | `{fileName, contentType, size, documentType}` | `{documentId, fileName, uploadUrl}` | RF-06 |
| POST | `/api/documents/{documentId}/complete` | ✅ | Confirmar carga | — | `DocumentResponseDTO` | RF-06 |

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

{ "callId": 1, "residentId": 1, "documentIds": ["3f6c…", "a91b…", "c02d…"] }
```
Respuesta exitosa: `200 OK` con `ApplicationResponseDTO`.

## Versionado y compatibilidad
✏️ Definir la política (p. ej. prefijo `/api/v1`, cambios compatibles hacia atrás, deprecación con aviso).
