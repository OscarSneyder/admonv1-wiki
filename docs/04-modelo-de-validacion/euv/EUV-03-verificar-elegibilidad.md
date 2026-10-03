# EUV-03 · Verificar elegibilidad para presentar una solicitud

| Campo | Valor |
|-------|-------|
| Requisito | RF-04 |
| Actor | Residente autenticado |
| Disparador | Invocación directa al endpoint. El frontend actual no lo invoca (la función `checkApplicationEligibility` fue retirada); la única participación se valida al registrar la solicitud (C.U-03, paso 8) |
| Endpoint | `GET /api/applications/check?callId={id}` |
| Identidad | El correo se toma del JWT (`authentication.getName()`), no de un parámetro |
| Componentes | Front: — (no se invoca) · Back: `ApplicationController.checkApplicationEligibility`, `ApplicationServiceImpl` |

## Flujo
> ✏️ **Diagrama por agregar:** `![Flujo de verificación de elegibilidad](../../assets/imagenes/esc-f-03-verificar-elegibilidad.png)`

## Respuestas
| Caso | HTTP | Código |
|------|------|--------|
| Parámetros faltantes | 400 | `INVALID_REQUEST` |
| Convocatoria inexistente | 404 | `CALL_NOT_FOUND` |
| Usuario inexistente | 404 | `USER_NOT_FOUND` |
| Sin unidad privada | 400 | `APARTMENT_NOT_FOUND` |
| Elegible | 200 | `{canApply: true, existingApplication: null}` |
| Con solicitud previa | 200 | `{canApply: false, existingApplication: {id, applicationNumber, status, createdAt}}` |

## Criterios de aceptación
- **Dado** una unidad privada que ya tiene solicitud en la convocatoria, **cuando** se consulta `GET /api/applications/check?callId=`, **entonces** la respuesta es `canApply: false` con la solicitud existente.
