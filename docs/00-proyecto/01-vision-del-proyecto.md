# Visión del proyecto

> **Propósito:** expresar la necesidad de negocio que justifica ADMON, el valor esperado y la forma de evidenciarlo, como punto de partida del producto según PAREX (cap. 2 *La necesidad del negocio como punto de partida* y cap. 17 *De la estrategia al valor real*).
> **Responsable:** ✏️ (hebra Producto) · **Última revisión:** 2026-10-02 · **Estado:** 🟡

## 1. Declaración de visión
> **Para** los residentes y la administración de un conjunto residencial de propiedad horizontal, **que** necesitan asignar los cupos de parqueadero comunal de forma **equitativa, transparente y verificable**, **ADMON** soporta el servicio de [Reservación de zonas comunes](../01-modelo-de-negocio/05-servicios-de-negocio.md), empezando por la [reserva de parqueadero comunal](../01-modelo-de-negocio/06-escenarios-de-servicio.md). **A diferencia de** una asignación sin un mecanismo que aplique las reglas de manera uniforme ni deje registro de cada decisión, **el producto** hace cumplir las reglas de cada convocatoria (una solicitud por unidad privada y documentos obligatorios del vehículo), se adapta a la cantidad de cupos y a la periodicidad que defina cada conjunto, y conserva la trazabilidad de cada solicitud hasta la asignación del cupo.

## 2. Necesidad de negocio (resumen)
La formulación completa y los supuestos del conjunto de referencia están en [Necesidad del negocio](../01-modelo-de-negocio/01-necesidad-del-negocio.md).

| Campo PAREX | Contenido |
|-------------|-----------|
| Título de la necesidad | Asegurar la asignación equitativa, transparente y trazable de los cupos de parqueadero comunal |
| Servicio de negocio involucrado | [SN-06 Reservación de zonas comunes](../01-modelo-de-negocio/05-servicios-de-negocio.md) · [ESN-01 Reserva de parqueadero comunal](../01-modelo-de-negocio/06-escenarios-de-servicio.md) |
| Actores impactados | Residente, Copropietario, Administrador ([actores](../01-modelo-de-negocio/02-actores-y-stakeholders.md)) |
| Situación que la origina | Los cupos de parqueadero comunal son menos que las unidades privadas (≈ 1 cupo por cada 7 unidades) y se reasignan periódicamente (cada 3, 4 o 6 meses según el conjunto) mediante convocatoria y sorteo ([SUP-02, SUP-03](../01-modelo-de-negocio/01-necesidad-del-negocio.md#supuestos-del-conjunto-de-referencia)) |
| Evidencia disponible | Análisis del equipo sobre conjuntos conocidos; sin datos medidos de un conjunto específico ([supuestos](../01-modelo-de-negocio/01-necesidad-del-negocio.md#supuestos-del-conjunto-de-referencia)) |
| Justificación | La asignación es recurrente (2 a 4 veces al año), involucra a todas las unidades privadas y debe cumplir el reglamento de propiedad horizontal |
| Riesgos si no se atiende | Percepción de inequidad y conflictos de convivencia; cupos asignados a vehículos sin documentos vigentes; más de un cupo por unidad privada; ausencia de soporte ante reclamos en asamblea |

## 3. Rol del software
El software es un **habilitador necesario** del servicio: la necesidad exige controlar de forma sistemática reglas, estados, parámetros por conjunto y trazabilidad, con 800 unidades privadas en el conjunto de referencia y convocatorias recurrentes.

| Necesidad de control | Cómo la atiende el producto | Referencia |
|----------------------|-----------------------------|------------|
| Una sola solicitud por unidad privada y convocatoria | Regla de negocio y restricción única en base de datos | [RN-04](../01-modelo-de-negocio/04-reglas-de-negocio.md), [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) |
| Documentos obligatorios del vehículo completos y válidos | Validación de tipo, tamaño y obligatoriedad | [RN-06 a RN-11](../01-modelo-de-negocio/04-reglas-de-negocio.md), [EUV-04](../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) |
| Cupos y periodicidad propios de cada conjunto | Parámetros definidos en cada convocatoria | [RN-14, RN-15](../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar), [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) |
| Estado y trazabilidad de cada solicitud hasta la asignación | Estados de la solicitud, número único `POST-<UUID>` y sorteo registrado | [RN-13](../01-modelo-de-negocio/04-reglas-de-negocio.md), [C.U-07](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md), [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) |

## 4. Valor esperado y evidencia de mejora
| ID | Valor esperado | Elemento del modelo | Evidencia de mejora (medida) | EUV | Estado |
|----|----------------|---------------------|------------------------------|-----|--------|
| OBJ-01 | Gestionar en línea las solicitudes a convocatorias de parqueadero comunal | [C.U-03 Registrar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | 100 % de las solicitudes de la convocatoria piloto registradas en ADMON, sin recepción por canales alternos | [EUV-05](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | 🟢 Implementado · ✏️ sin evidencia |
| OBJ-02 | Garantizar una solicitud por unidad privada y convocatoria | [RN-04](../01-modelo-de-negocio/04-reglas-de-negocio.md) · RNF-03 | 0 duplicados (restricción `uk_application_apartment_call`) | [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) | 🟢 Implementado · ✏️ sin evidencia |
| OBJ-03 | Asegurar documentos obligatorios del vehículo | [RN-07](../01-modelo-de-negocio/04-reglas-de-negocio.md) | 100 % de solicitudes con documentos obligatorios | [EUV-05](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | 🟢 Implementado · ✏️ sin evidencia |
| OBJ-04 | Soportar picos de concurrencia al abrir convocatorias | RNF-01 | p95 < 2 s y errores < 1 % con 300 usuarios concurrentes | [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) | 🟡 p95 cumple; tasa de fallo 16,7 % no cumple |
| OBJ-05 | Asegurar la trazabilidad de la asignación | [C.U-07](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md), [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | 100 % de las solicitudes con historial de estados consultable, desde el registro hasta la asignación | ✏️ EUV por crear | 🔴 No implementado |

El estado consolidado de la evidencia se mantiene en la [lectura integrada de validación](../05-ejecucion-y-evidencia/02-lectura-integrada.md).

## 5. Alcance
La RBS cubre de extremo a extremo el escenario ESN-01 ([RBS](../06-direccion-y-planeacion/02-rbs.md)).

| Caso de uso | Escenario de servicio | Implementado en v1 | RBS |
|-------------|-----------------------|:------------------:|:---:|
| [C.U-01 Publicar convocatoria](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | ESN-01 · paso 1 | ❌ | ✅ |
| [C.U-02 Consultar convocatoria](../02-modelo-de-capacidades/casos-de-uso/C.U-02-consultar-convocatoria.md) | ESN-01 · paso 3 | ✅ | ✅ |
| [C.U-03 Registrar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | ESN-01 · paso 4 | 🟡 Sin validación de paz y salvo (RN-16) | ✅ |
| [C.U-04 Consultar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-04-consultar-solicitud.md) | — | 🟡 Residente | ✅ |
| [C.U-07 Aceptar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md) | ESN-01 · paso 6 | ❌ | ✅ |
| [C.U-08 Ejecutar sorteo de cupos](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | ESN-01 · paso 7 | ❌ | ✅ |
| Autenticación (JWT) | Transversal | ✅ | ✅ |
| Notificaciones ([MEC-05](../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)) | ESN-01 · pasos 2 y 5 | ❌ | ✅ |
| [C.U-05 Ajustar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-05-ajustar-solicitud.md), [C.U-06 Cancelar solicitud](../02-modelo-de-capacidades/casos-de-uso/C.U-06-cancelar-solicitud.md), [C.U-09 Configurar zona común](../02-modelo-de-capacidades/casos-de-uso/C.U-09-configurar-zona-comun.md) | — | ❌ | Diferidos |
| ESN-02 y servicios SN-01 a SN-05, SN-07, SN-08 | — | ❌ | Fuera del alcance |

## 6. Incertidumbres
| Tipo | Incertidumbre | Tratamiento | Referencia |
|------|---------------|-------------|------------|
| Negocio | Reglas del sorteo (criterios, prioridades, lista de espera) | TJ-03 | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) |
| Negocio | Criterios con que el administrador acepta o rechaza una solicitud | TJ-02 | [C.U-07](../02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md) |
| Técnica | Causa de la tasa de fallo del 16,7 % bajo 300 usuarios concurrentes | TJ-01 | [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) |
| Técnica | La consulta de solicitudes no se restringe al residente autenticado (R-01 parcial; la creación ya usa el JWT) | TJ-01 | [R-01](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md) |
| Técnica | Persistencia definitiva: H2 actual frente a PostgreSQL previsto | ✏️ | [ADR-0005](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0005-persistencia-jpa-h2-postgresql.md) |

## 7. Primera tajada y RBS
| Tajada | Contenido | Incertidumbre que reduce |
|--------|-----------|--------------------------|
| TJ-01 | Completar R-01 (listado y detalle restringidos al residente del JWT); diagnosticar EUV-Q-01 con 300 residentes semilla; registrar la evidencia de la prueba de extremo a extremo (EUV-01 a EUV-06) | Técnica: seguridad y rendimiento; evidencia de lo implementado |
| TJ-02 | C.U-01 Publicar convocatoria (con cupos y periodo parametrizables) y C.U-07 Aceptar solicitud | Negocio: validación por el administrador y estados |
| TJ-03 | C.U-08 Ejecutar sorteo de cupos y MEC-05 Notificaciones | Negocio: reglas y aceptación del sorteo; completa la RBS |

Detalle en el [backlog de tajadas](../06-direccion-y-planeacion/03-backlog-de-tajadas.md).

## 8. Validación de la necesidad
Autoevaluación con los criterios PAREX de necesidad suficientemente validada (cap. 2.15), con la **opción de conjunto de referencia y supuestos explícitos**.

| Criterio | Estado | Observación |
|----------|:------:|-------------|
| Situación de negocio clara | 🟡 | Descrita con supuestos SUP-01 a SUP-04 |
| Origen identificado | 🟢 | Riesgo operativo y de convivencia; cumplimiento del reglamento |
| Problema y solución diferenciados | 🟢 | La visión se formula sobre la asignación de cupos, no sobre la herramienta |
| Actores impactados reconocidos | 🟢 | Residente, Copropietario, Administrador |
| Servicio de negocio involucrado | 🟢 | SN-06 / ESN-01 |
| Valor esperado explícito | 🟢 | OBJ-01 a OBJ-05 con medida |
| Evidencia disponible | 🟡 | Análisis del equipo sobre conjuntos conocidos; sin medición directa |
| Disposición o justificación suficiente | 🟡 | Justificación por recurrencia y reglamento |
| Rol del software aclarado | 🟢 | Habilitador necesario (sección 3) |
| Riesgos visibles | 🟢 | Riesgos de negocio y técnicos identificados |
| Evidencia esperada de mejora | 🟢 | Medidas OBJ-01 a OBJ-05 |
| Primera tajada posible | 🟢 | TJ-01 |

## 9. Stack tecnológico
El stack se documenta en el [Modelo de arquitectura](../03-modelo-de-arquitectura/README.md#stack-tecnológico).
