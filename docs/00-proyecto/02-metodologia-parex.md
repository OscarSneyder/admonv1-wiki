# Metodología PAREX en ADMON v1

> **Propósito:** explicar cómo el equipo aplica la metodología [PAREX](https://www.sistemaparex.co/es) en el proyecto y cómo se organiza esta wiki alrededor de ella.

## Principio rector
PAREX considera **avance real** solo aquel que está integrado, validado y respaldado por evidencia verificable. El producto se entiende como un sistema de modelos conectados y crece por **tajadas**: intervenciones pequeñas para construir, pero suficientemente completas para validar comportamiento, calidad, riesgo y evidencia.

## Modelos PAREX y secciones de la wiki
| Modelo / vista PAREX | Pregunta que responde | Sección de la wiki |
|----------------------|-----------------------|--------------------|
| Modelo de Negocio | ¿Qué realidad debe soportar el software? | [01 · Modelo de negocio](../01-modelo-de-negocio/05-servicios-de-negocio.md) |
| Modelo de Capacidades del Software | ¿Qué debe ser capaz de hacer el sistema y bajo qué condiciones de calidad? | [02 · Modelo de capacidades](../02-modelo-de-capacidades/01-mapa-de-casos-de-uso.md) |
| Modelo de Arquitectura del Software | ¿Qué decisiones y mecanismos técnicos sostienen esas capacidades? | [03 · Modelo de arquitectura](../03-modelo-de-arquitectura/README.md) |
| Modelo de Validación | ¿Cómo se demuestra que el producto responde a lo esperado? | [04 · Modelo de validación](../04-modelo-de-validacion/README.md) |
| Vista de Ejecución y Evidencia | ¿Qué ocurrió al validar y qué evidencia quedó? | [05 · Ejecución y evidencia](../05-ejecucion-y-evidencia/02-lectura-integrada.md) |
| Dirección estratégica y planeación | ¿Qué se construye primero y con qué inversión? | [06 · Dirección y planeación](../06-direccion-y-planeacion/02-rbs.md) |

Las secciones [07 · Código y despliegue](../07-codigo-y-despliegue/01-gitflow.md) y [08 · Gobierno](../08-gobierno/README.md) complementan la metodología con la gestión de código (GitFlow), el pipeline hacia AWS y las convenciones del equipo.

## Cadena de trazabilidad
`Servicio de negocio (SN)` → `Escenario de servicio (ESN)` → `Caso de uso (C.U)` → `RNF` → `Mecanismo de diseño (MEC)` → `EUV` → `Evidencia`

La cadena completa se mantiene en la [matriz de trazabilidad](../08-gobierno/03-matriz-de-trazabilidad.md).

## Identificadores
| Elemento | Prefijo | Ejemplo | Plantilla |
|----------|---------|---------|-----------|
| Servicio de negocio | `SN-NN` | SN-06 Reservación de zonas comunes | [servicio-de-negocio](../plantillas/servicio-de-negocio.md) |
| Escenario de servicio | `ESN-NN` | ESN-01 Reserva de parqueadero comunal | [servicio-de-negocio](../plantillas/servicio-de-negocio.md) |
| Caso de uso | `C.U-NN` | C.U-03 Registrar solicitud | [caso-de-uso](../plantillas/caso-de-uso.md) |
| Regla de negocio | `RN-NN` | RN-04 | — |
| Requerimiento no funcional | `RNF-NN` | RNF-01 Rendimiento | [rnf](../plantillas/rnf.md) |
| Mecanismo de diseño | `MEC-NN` | MEC-01 | [mecanismo-de-diseno](../plantillas/mecanismo-de-diseno.md) |
| Decisión de arquitectura | `ADR-NNNN` | ADR-0004 | [adr](../plantillas/adr.md) |
| Escenario de uso y validación | `EUV-NN` / `EUV-Q-NN` | EUV-05 / EUV-Q-01 | [euv](../plantillas/euv.md) · [euv-calidad](../plantillas/euv-calidad.md) |
| Tajada | `TJ-NN` | TJ-01 | [evidencia-tajada](../plantillas/evidencia-tajada.md) |

## Caso de uso (C.U)
Cada C.U se documenta con **flujo básico, flujos alternativos y flujos de excepción**, y se relaciona con las reglas de negocio, los RNF y los EUV que lo validan.

## EUV: niveles de madurez
| Nivel | Contenido mínimo |
|-------|------------------|
| Idea | Nombre del escenario y comportamiento que demuestra |
| Sketch | Pasos principales, precondiciones y resultado esperado |
| Detalle | Datos, micropasos, resultado esperado verificable y caso automatizable |

## Hebras del desarrollo trenzado
El trabajo de cada tajada avanza en cuatro hebras simultáneas: **Conducción**, **Producto**, **Arquitectura y desarrollo**, y **Demostración, evidencia y riesgos**. La asignación del equipo está en [Equipo y hebras](03-equipo-y-hebras.md).
