# EUV-05 · Registrar solicitud

| Campo | Valor |
|-------|-------|
| Requisito | RF-07 |
| Actor | Residente autenticado |
| Precondiciones | Elegible (EUV-03) · documentos obligatorios en `UPLOADED` (EUV-04) |
| Postcondición | `Application` en estado `REGISTERED`, número `POST-<UUID>`, documentos vinculados |
| Pantalla | `/convocations/:id/apply` → `CreateApplicationPage` (`ApplicationStepper`) |
| Endpoint | `POST /api/applications` — cuerpo `{callId, residentId, documentIds[]}` |
| Componentes | Front: `useCreateApplication`, `CreateApplication`, `HttpApplicationRepository` · Back: `ApplicationController`, `ApplicationServiceImpl`, `ApplicationRepository`, `DocumentRepository`, `UserRepository`, `CallRepository` |

> **Seguridad:** el backend toma el residente del JWT; el `residentId` que el frontend aún envía desde `RESIDENT.id` (`src/constants.js`) se ignora. Ver [R-01](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md).

## Flujo principal
> ✏️ **Diagrama por agregar:** `![Flujo de registro de postulación](../../assets/imagenes/esc-f-05-registrar-postulacion.png)`

## Flujos de excepción que demuestra ([C.U-03](../../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md#flujos-de-excepción))
| Excepción C.U-03 | Caso | Código |
|------------------|------|--------|
| — | Solicitud inválida (sin convocatoria) | `INVALID_REQUEST` |
| — | Residente inexistente | `RESIDENT_NOT_FOUND` |
| FE-02 | Solicitud duplicada de la unidad privada | `APPLICATION_ALREADY_EXISTS` (409) |
| FE-04 | Convocatoria inexistente | `CALL_NOT_FOUND` (404) |
| FE-05 | Residente sin unidad privada | `APARTMENT_NOT_FOUND` |
| FE-09 | Documento inexistente o sin terminar de cargar | `DOCUMENT_NOT_FOUND` / `DOCUMENT_NOT_UPLOADED` |
| FE-10 | Faltan documentos obligatorios | `REQUIRED_DOCUMENTS_MISSING` |

## Criterios de aceptación
- **Dado** un residente elegible con los 3 documentos obligatorios cargados, **cuando** confirma, **entonces** se crea la solicitud `REGISTERED`.
- **Dado** una unidad privada con solicitud previa, **cuando** intenta presentar una solicitud de nuevo, **entonces** recibe 409.

## Evidencias
- Prueba de carga k6 sobre este endpoint: [EUV-Q-01](EUV-Q-01-concurrencia-postulaciones.md).
