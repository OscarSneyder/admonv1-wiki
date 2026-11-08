# C.U-08 · Ejecutar sorteo de cupos

> **Alcance:** incluido en la RBS (tajada TJ-03), según la [visión del proyecto](../../00-proyecto/01-vision-del-proyecto.md#5-alcance). El sorteo se ejecuta en cada convocatoria, con la periodicidad que defina el conjunto (SUP-03).
>
> **Dependencia:** solo participan las solicitudes aprobadas en [C.U-07 Aceptar solicitud](C.U-07-aceptar-solicitud.md). El C.U-08 no se puede demostrar de extremo a extremo hasta que el C.U-07 esté terminado.

| Campo | Valor |
|-------|-------|
| Paquete | Convocatorias |
| Actor primario | Administrador |
| Actores secundarios | Residente (recibe el resultado, vía [MEC-05](../../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)) |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 7 |
| Objetivo del actor | Asignar de forma aleatoria, justa y verificable los cupos de parqueadero comunal de una convocatoria cerrada entre las solicitudes aprobadas |
| Resultado observable | Cada solicitud aprobada queda `SELECTED` (con cupo) o `NOT_SELECTED` (en lista de espera), con su posición en el orden del sorteo, y queda un registro de auditoría del sorteo |
| Requisitos funcionales relacionados | RF-12, RF-13 |
| Estado | 🔴 No implementado |

> **Decisiones del equipo (2026-11-07):**
> - Participan todas las solicitudes en estado `APPROVED`, y solo ellas.
> - El sorteo se ejecuta solo con la convocatoria cerrada y **no se puede repetir**.
> - Siempre hay más solicitudes que cupos, por la relación de 1 cupo por cada 7 unidades privadas (SUP-02).
> - Se guarda el orden completo de todos los participantes, para cubrir renuncias con la lista de espera.
> - Se guarda un registro que permita auditar que el sorteo fue justo.
> - Se agregan los estados de solicitud `SELECTED` y `NOT_SELECTED`.

## Precondiciones
1. El administrador inició sesión con `ROLE_ADMIN` ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).
2. La convocatoria existe y está cerrada, ya sea por cierre manual o porque pasó su fecha de fin ([RN-18](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
3. La convocatoria no tiene un sorteo ejecutado ([RN-19](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
4. El administrador terminó de validar las solicitudes de la convocatoria ([C.U-07](C.U-07-aceptar-solicitud.md)).
5. El número de solicitudes `APPROVED` es mayor que el número de cupos de la convocatoria ([RN-14](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar), SUP-02).

## Postcondiciones
1. Existe un único sorteo registrado para la convocatoria, con su registro de auditoría ([RN-21](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
2. Todas las solicitudes `APPROVED` de la convocatoria tienen una posición única en el orden del sorteo (1, 2, …, N).
3. Las solicitudes en las posiciones 1 a *cupos* quedan en estado `SELECTED`; las demás quedan en `NOT_SELECTED` y forman la lista de espera en ese mismo orden ([RN-20](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
4. La convocatoria ya no se puede reabrir ni volver a sortear ([RN-19](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)).
5. Cada residente participante puede consultar su resultado y su posición ([C.U-04](C.U-04-consultar-solicitud.md)).

## Flujo básico
| Paso | Actor / Sistema | Acción |
|:----:|-----------------|--------|
| 1 | Administrador | Ingresa a la administración de convocatorias y selecciona una convocatoria cerrada |
| 2 | Sistema | Muestra el resumen previo al sorteo: número de cupos, número de solicitudes `APPROVED` participantes y número de solicitudes por estado |
| 3 | Administrador | Elige ejecutar el sorteo |
| 4 | Sistema | Pide confirmación e informa que el sorteo es definitivo y no se puede repetir |
| 5 | Administrador | Confirma la ejecución |
| 6 | Sistema | Valida que la convocatoria exista, esté cerrada y no tenga un sorteo previo (RN-18, RN-19) |
| 7 | Sistema | Obtiene las solicitudes `APPROVED` de la convocatoria y verifica que superen el número de cupos (RN-17) |
| 8 | Sistema | Ordena las solicitudes participantes por número de solicitud para fijar la lista de entrada, y calcula su huella digital (*hash*) |
| 9 | Sistema | Genera una semilla aleatoria segura y, con ella, obtiene un orden aleatorio de los participantes (RN-21) |
| 10 | Sistema | Asigna a cada solicitud su posición; las posiciones 1 a *cupos* pasan a `SELECTED` y las demás a `NOT_SELECTED` (RN-20) |
| 11 | Sistema | Registra el sorteo y su auditoría en la misma transacción que el cambio de estado de las solicitudes (RN-21) |
| 12 | Sistema | Notifica el resultado a los residentes participantes ([MEC-05](../../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)) |
| 13 | Sistema | Muestra al administrador el resultado: lista de seleccionados y lista de espera en orden, con los datos de auditoría. Fin |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 2 | Hay solicitudes de la convocatoria sin validar (`REGISTERED` o `PENDING_VALIDATION`) | El sistema advierte cuántas solicitudes no participarán por no estar aprobadas. El administrador puede validarlas primero ([C.U-07](C.U-07-aceptar-solicitud.md)) o continuar sin ellas | Paso 3, o fin si el administrador va a validarlas |
| FA-02 | 5 | El administrador no confirma | El sistema no ejecuta el sorteo ni modifica datos | Paso 2 |
| FA-03 | 1 | La convocatoria ya tiene un sorteo ejecutado | El sistema muestra el resultado y la auditoría registrados, sin opción de ejecutar | Paso 13 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Regla |
|--------|:----:|-----------|-----------------------|-------|
| FE-01 | 6 | La convocatoria no existe | Rechaza la ejecución (`404 CALL_NOT_FOUND`). Fin | [RN-01](../../01-modelo-de-negocio/04-reglas-de-negocio.md) |
| FE-02 | 6 | La convocatoria no está cerrada | Rechaza la ejecución (`409 CALL_NOT_CLOSED`) e informa que debe cerrarse primero. Fin | RN-18 |
| FE-03 | 6 | La convocatoria ya tiene un sorteo, incluso si dos administradores lo ejecutan al mismo tiempo | Rechaza la ejecución (`409 DRAW_ALREADY_EXECUTED`) y conserva el sorteo existente. Fin | RN-19 |
| FE-04 | 7 | Las solicitudes `APPROVED` no superan el número de cupos | Rechaza la ejecución (`409 INSUFFICIENT_APPROVED_APPLICATIONS`) e informa cuántas hay y cuántos cupos tiene la convocatoria. Fin | RN-17 |
| FE-05 | 3 | El usuario no tiene `ROLE_ADMIN` | Rechaza la petición (`403`). Fin | [RNF-02](../03-atributos-de-calidad-y-rnf.md) |
| FE-06 | 11 | Falla el registro del sorteo | Revierte toda la operación: ninguna solicitud cambia de estado y no queda sorteo registrado. Fin | RN-21 |

> FE-04 no debería ocurrir según SUP-02 (siempre hay más solicitudes que cupos), pero se mantiene para que el sistema nunca ejecute un sorteo sin sentido si una convocatoria recibe pocas solicitudes aprobadas.

## Reglas de negocio aplicables
RN-01, RN-14, RN-15 y RN-17 a RN-21 ([Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

### Registro de auditoría del sorteo (RN-21)
Cada sorteo guarda, como mínimo:

| Dato | Para qué sirve |
|------|----------------|
| Convocatoria y número de cupos | Identificar el sorteo |
| Fecha y hora de ejecución (zona America/Bogota) y administrador que lo ejecutó | Trazabilidad |
| Lista de entrada: solicitudes participantes, ordenadas por número de solicitud | Saber exactamente quién participó |
| Huella digital (*hash* SHA-256) de la lista de entrada | Demostrar que la lista no se alteró después |
| Semilla aleatoria y algoritmo usado | Reproducir el sorteo |
| Resultado: posición de cada solicitud | Verificar la asignación y la lista de espera |

**Verificación:** con la lista de entrada, la semilla y el algoritmo, cualquier persona puede volver a calcular el orden y comprobar que coincide con el resultado registrado. El registro no se puede modificar ni borrar.

### Ciclo de vida de la solicitud en el sorteo
```text
APPROVED ──(sorteo, posición ≤ cupos)──► SELECTED
APPROVED ──(sorteo, posición > cupos)──► NOT_SELECTED  (lista de espera, en orden)
```
Las solicitudes `REGISTERED`, `PENDING_VALIDATION`, `REJECTED` y `CANCELLED` no participan y su estado no cambia.

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Solo `ROLE_ADMIN` ejecuta el sorteo (FE-05) |
| RNF-03 Integridad | Un único sorteo por convocatoria, incluso con ejecuciones simultáneas (FE-03); cambios de estado y registro en una sola transacción (FE-06) |
| ✏️ RNF por definir · Auditabilidad | El sorteo se puede reproducir y verificar (RN-21) |

## Fuera del alcance de este C.U
- **Cubrir renuncias con la lista de espera:** el orden queda guardado (RN-20), pero el flujo de renuncia de un seleccionado y la asignación al siguiente de la lista requiere un caso de uso propio, todavía no definido.
- **Vigencia de la asignación:** depende de RN-15, que aún no está en el modelo de la convocatoria.

## Puntos por confirmar
| Punto | Propuesta en esta especificación |
|-------|-----------------------------------|
| Solicitudes que quedan sin validar al ejecutar el sorteo (FA-01) | No participan y conservan su estado; el sistema solo advierte |
| Visibilidad del resultado para el residente | Ve su estado y su posición; no ve los datos de otros participantes |
| Publicación de la auditoría | Visible para el administrador; ✏️ definir si se publica a los residentes |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| ✏️ EUV por crear · Ejecutar sorteo | Flujo básico, FA-03, FE-02, FE-03, FE-04 | Pendiente |
| ✏️ EUV-Q por crear · Sorteo único bajo concurrencia | FE-03 con ejecuciones simultáneas | Pendiente |
| ✏️ EUV-Q por crear · Reproducibilidad del sorteo | RN-21 | Pendiente |
