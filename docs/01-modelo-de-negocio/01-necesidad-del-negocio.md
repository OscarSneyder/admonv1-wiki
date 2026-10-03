# Necesidad del negocio (contexto y problema)

> **Propósito:** formular la necesidad de negocio que justifica intervenir con software, según PAREX (cap. 2.7, formulación de la necesidad).
> **Enfoque:** conjunto residencial de referencia con supuestos explícitos. Los datos de esta página provienen del análisis del equipo sobre conjuntos conocidos y de los datos semilla del sistema; no son mediciones de un conjunto específico.

## Conjunto de referencia
| Característica | Valor de referencia | Supuesto |
|----------------|---------------------|----------|
| Torres | 1 | SUP-01 |
| Unidades privadas | 800 | SUP-01 |
| Cupos de parqueadero comunal | ≈ 114 (1 cupo por cada 7 unidades privadas) | SUP-02 |
| Periodicidad de la convocatoria | Cada 3, 4 o 6 meses (2 a 4 convocatorias por año) | SUP-03 |

## Supuestos del conjunto de referencia
| ID | Supuesto | Fuente | Implicación en el producto |
|----|----------|--------|----------------------------|
| SUP-01 | El conjunto de referencia tiene 1 torre con 800 unidades privadas | Datos semilla del backend | Volumen de usuarios para RNF-01 y EUV-Q-01 |
| SUP-02 | La relación es de aproximadamente 1 cupo de parqueadero comunal por cada 7 unidades privadas; aumenta o disminuye según el conjunto | Análisis del equipo sobre conjuntos conocidos | El número de cupos se define en cada convocatoria ([RN-14](04-reglas-de-negocio.md#reglas-identificadas-por-implementar)) |
| SUP-03 | Las convocatorias o sorteos se realizan cada 3, 4 o 6 meses, según el conjunto | Análisis del equipo sobre conjuntos conocidos | El periodo de vigencia de la asignación se define en cada convocatoria ([RN-15](04-reglas-de-negocio.md#reglas-identificadas-por-implementar)) |
| SUP-04 | La demanda de cupos supera la oferta, por lo que la asignación se hace por convocatoria y sorteo | Modelo de negocio (C.U-08 Ejecutar sorteo de cupos) y SUP-02 | El sorteo debe ser verificable y trazable ([C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md)) |
| SUP-05 | Las reglas de asignación (una solicitud por unidad privada, documentos del vehículo) se derivan del reglamento de propiedad horizontal y de las decisiones de la asamblea | Modelo semántico (Reglamento de Propiedad Horizontal) | Las reglas deben ser explícitas y aplicarse de manera uniforme |

## Formulación de la necesidad
| Campo PAREX | Contenido |
|-------------|-----------|
| Título de la necesidad | Asegurar la asignación equitativa, transparente y trazable de los cupos de parqueadero comunal |
| Situación que la origina | El conjunto dispone de menos cupos de parqueadero comunal que unidades privadas (SUP-02) y debe reasignarlos periódicamente (SUP-03) mediante convocatoria y sorteo (SUP-04). En cada convocatoria la administración debe recibir las solicitudes, verificar que cada unidad privada presente una sola, revisar los documentos del vehículo y ejecutar el sorteo |
| Origen de la necesidad | Riesgo operativo y de convivencia, y cumplimiento del reglamento de propiedad horizontal (SUP-05) |
| Problema real asociado | Sin un mecanismo que aplique las reglas de manera uniforme y registre cada decisión, la asignación queda expuesta a solicitudes duplicadas, documentos incompletos o vencidos y resultados difíciles de verificar |
| Consecuencias actuales | Reclamos y desconfianza de los residentes frente al resultado; reprocesos de la administración en cada convocatoria; dificultad para soportar decisiones ante la asamblea |
| Actores impactados | Residente, Copropietario, Administrador, Consejo y Asamblea ([modelo semántico](03-modelo-semantico.md)) |
| Evidencia disponible | Análisis del equipo sobre conjuntos conocidos (SUP-02, SUP-03); sin medición directa de un conjunto específico |
| Necesidad sentida o justificación estratégica | La asignación es recurrente (2 a 4 veces al año) e involucra a todas las unidades privadas del conjunto; cada convocatoria repite la carga operativa y el riesgo de conflicto |
| Servicio de negocio involucrado | [SN-06 Reservación de zonas comunes](05-servicios-de-negocio.md) · [ESN-01 Reserva de parqueadero comunal](06-escenarios-de-servicio.md) |
| Valor esperado | Asignaciones equitativas, verificables y trazables, con menos carga operativa para la administración en cada convocatoria |
| Riesgos si no se atiende | Percepción de inequidad y conflictos de convivencia; cupos asignados a vehículos sin documentos vigentes; más de un cupo por unidad privada; ausencia de soporte ante reclamos en asamblea |
| Condiciones para una buena solución | Parametrizar cupos y periodo por convocatoria; controlar una solicitud por unidad privada; validar documentos obligatorios; gestionar estados de la solicitud; ejecutar un sorteo verificable; conservar trazabilidad; notificar a los actores |
| Evidencia esperada de mejora | Objetivos OBJ-01 a OBJ-05 de la [visión](../00-proyecto/01-vision-del-proyecto.md#4-valor-esperado-y-evidencia-de-mejora) |
| Primera tajada posible | TJ-01 del [backlog de tajadas](../06-direccion-y-planeacion/03-backlog-de-tajadas.md) |

## Proceso de negocio (TO-BE)
> ✏️ **Diagrama por agregar:** `![Proceso de negocio TO-BE](../assets/imagenes/negocio-proceso-to-be.png)`

## Proceso actual (AS-IS)
✏️ **Por completar:** diagrama o descripción del proceso manual previo.
