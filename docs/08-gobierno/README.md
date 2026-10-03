# Gobierno de la arquitectura

El gobierno define **cómo se toman, comunican y verifican** las decisiones de arquitectura, para que el sistema evolucione de forma controlada.

## Modelo de gobierno
> ✏️ **Diagrama por agregar:** `![Modelo de gobierno](../assets/imagenes/gobierno-modelo.png)`

## Elementos del gobierno
| Elemento | Propósito | Dónde |
|----------|-----------|-------|
| Principios | Guían las decisiones | [01-principios](01-principios.md) |
| ADRs | Registran decisiones y su justificación | [04-decisiones-adr](../03-modelo-de-arquitectura/04-decisiones-adr/README.md) |
| Estándares | Convenciones de código, Git y API | [02-estandares-y-convenciones](02-estandares-y-convenciones.md) |
| Trazabilidad | Conecta requisitos ↔ escenarios ↔ componentes ↔ pruebas | [03-matriz-de-trazabilidad](03-matriz-de-trazabilidad.md) |
| Riesgos y deuda | Visibiliza y prioriza problemas | [05-riesgos-y-deuda-tecnica](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md) |
| Roles (RACI) | Quién decide qué | [Equipo y roles](../00-proyecto/03-equipo-y-hebras.md) |

## Comunicación
| Audiencia | Artefacto | Frecuencia | Canal |
|-----------|-----------|------------|-------|
| Equipo | ADRs nuevos, cambios en vistas | Por decisión | PR a la wiki + ✏️ canal |
| Docente / evaluador | Wiki completa, entregables | Por entrega | ✏️ |
| Stakeholders | Visión, escenarios, demo | Por sprint | Revisión de sprint |

## Checklist de conformidad (en cada PR con impacto arquitectónico)
- [ ] ¿Respeta las reglas de dependencia por capas (back) y por features (front)?
- [ ] ¿Usa `ApiResponse<T>` y los códigos de error del catálogo?
- [ ] ¿Los endpoints nuevos requieren JWT (salvo justificación)?
- [ ] ¿Se actualizaron los [contratos API](../03-modelo-de-arquitectura/06-contratos-api.md) y la trazabilidad?
- [ ] ¿Requiere un ADR nuevo o modificar uno existente?
- [ ] ¿Se agregaron o actualizaron las pruebas?
