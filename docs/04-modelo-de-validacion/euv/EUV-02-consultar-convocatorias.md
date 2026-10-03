# EUV-02 · Consultar convocatorias y su detalle

| Campo | Valor |
|-------|-------|
| Requisitos | RF-02, RF-03 |
| Actor | Residente autenticado |
| Pantallas | `/convocations` → `ConvocationsPage` · `/convocations/:id` → `ConvocationDetailPage` |
| Endpoints | `GET /api/calls` · `GET /api/calls/{callId}` |
| Componentes | Front: `useConvocations`, `useConvocation`, `GetConvocations`, `GetConvocationById`, `HttpConvocationRepository` · Back: `CallController`, `CallServiceImpl`, `CallRepository` |

## Flujo principal
> ✏️ **Diagrama por agregar:** `![Flujo de consulta de convocatorias](../../assets/imagenes/esc-f-02-consultar-convocatorias.png)`

## Datos de ejemplo (semilla)
| id | Título | Vigencia | Cupos | Estado |
|----|--------|----------|-------|--------|
| 1 | Convocatoria Parqueadero Comunal 2026 | 2026-08-01 → 2026-09-30 | 200 | OPEN |
| 2 | Convocatoria Parqueadero Comunal 2026 - 2 | 2026-10-01 → 2026-11-30 | 180 | OPEN |

## Criterios de aceptación
- **Dado** un residente autenticado, **cuando** abre `/convocations`, **entonces** ve todas las convocatorias con título, fechas y cupos.
- ✏️ Criterio para convocatoria inexistente (404).

## Evidencias
✏️ Capturas de pantalla / pruebas.
