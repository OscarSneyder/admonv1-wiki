# EUV-06 · Consultar mis solicitudes y su detalle

| Campo | Valor |
|-------|-------|
| Requisitos | RF-08, RF-09 |
| Actor | Residente autenticado |
| Pantallas | `/mis-postulaciones` → `ApplicationsPage` · `/applications/:id` → `ApplicationDetailPage` |
| Endpoints | `GET /api/applications?residentId=&page=0&size=10&sortBy=createdAt&direction=DESC` · `GET /api/applications/{applicationId}` |
| Componentes | Front: `useApplications`, `useApplication`, `GetApplications`, `GetApplicationById`, `ApplicationCard`, `ApplicationDetails`, `ApplicationDocuments`, `ApplicationStatus` · Back: `ApplicationController`, `ApplicationServiceImpl`, `ApplicationSpecification` (filtros dinámicos), `PageResponseDTO` |

## Flujo
> ✏️ **Diagrama por agregar:** `![Flujo de consulta de postulaciones](../../assets/imagenes/esc-f-06-consultar-postulaciones.png)`

## Parámetros de consulta
| Parámetro | Por defecto | Descripción |
|-----------|-------------|-------------|
| `residentId` | — | Filtro opcional; el frontend no lo envía, por lo que se listan todas las solicitudes ⚠️ (ver riesgo R-01) |
| `page` | 0 | Página |
| `size` | 10 | Tamaño de página |
| `sortBy` | `createdAt` | Campo de ordenamiento |
| `direction` | `DESC` | `ASC` / `DESC` |

## Criterios de aceptación
- ✏️
