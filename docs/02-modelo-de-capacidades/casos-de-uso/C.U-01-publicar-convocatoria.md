# C.U-01 · Publicar convocatoria

> **Alcance:** incluido en la RBS (tajada TJ-02). La convocatoria define el número de cupos ([RN-14](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)) y el periodo de vigencia de la asignación ([RN-15](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)), porque ambos varían según el conjunto.

| Campo | Valor |
|-------|-------|
| Paquete | Convocatorias |
| Actor primario | Administrador |
| Actores secundarios | — |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 1 |
| Objetivo del actor | Publicar y administrar las convocatorias de parqueadero comunal, con su periodo de inscripción y su número de cupos |
| Resultado observable | Convocatoria publicada y visible para los residentes, con estado derivado `SCHEDULED`, `OPEN` o `CLOSED` |
| Requisitos funcionales relacionados | RF-10 |
| Estado | 🟡 Implementado parcialmente: falta el periodo de vigencia de la asignación (RN-15) |

> **Fuente de los flujos:** implementación v1 (commit *feat: publish call CU 1.1.4*, 2026-10-29). Backend: `CallController`, `CallServiceImpl`. Frontend: `/admin/convocations`, `/admin/convocations/new`, `/admin/convocations/:id/edit`. La columna *Estado v1* compara cada paso con el código de `develop` (revisión 2026-11-07).

## Precondiciones
1. El administrador inició sesión con `ROLE_ADMIN` ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).

## Postcondiciones
1. La convocatoria queda registrada con título, descripción, fecha de inicio, fecha de fin y número de cupos.
2. Su estado se deriva de las fechas y del cierre manual: `SCHEDULED` antes de la fecha de inicio, `OPEN` dentro del periodo y `CLOSED` después de la fecha de fin o si se cerró manualmente ([RN-25](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).
3. La convocatoria es visible para los residentes en [C.U-02](C.U-02-consultar-convocatoria.md).

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Administrador | Ingresa a *Administrar Convocatorias* | 🟢 `/admin/convocations`, visible solo para `ROLE_ADMIN` |
| 2 | Sistema | Muestra las convocatorias con su estado (Abierta, Programada o Cerrada) y sus acciones | 🟢 `GET /api/calls` |
| 3 | Administrador | Elige crear una convocatoria | 🟢 `/admin/convocations/new` |
| 4 | Administrador | Ingresa título, descripción, fecha de inicio, fecha de fin y cupos disponibles | 🟢 Formulario `CallForm` |
| 5 | Administrador | Ingresa el periodo de vigencia de la asignación | 🔴 No implementado ([RN-15](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)): el modelo no tiene el campo |
| 6 | Sistema | Valida los datos ([RN-22](../../01-modelo-de-negocio/04-reglas-de-negocio.md)) | 🟢 |
| 7 | Sistema | Registra la convocatoria y calcula su estado derivado. Fin | 🟢 `POST /api/calls` |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno | Estado v1 |
|--------|:--------------:|-----------|--------|---------|-----------|
| FA-01 | 2 | El administrador desea modificar una convocatoria | Edita los datos; el sistema los valida (sin exigir fecha de inicio futura) y guarda | Paso 2 | 🟢 `PUT /api/calls/{callId}`; botón deshabilitado si la convocatoria está cerrada manualmente |
| FA-02 | 2 | El administrador desea cerrar una convocatoria antes de su fecha de fin | El sistema marca el cierre manual; la convocatoria queda `CLOSED` y no admite solicitudes | Paso 2 | 🟢 `PATCH /api/calls/{callId}/close` |
| FA-03 | 2 | El administrador desea reabrir una convocatoria cerrada manualmente | El sistema retira el cierre manual y recalcula el estado por fechas | Paso 2 | 🟢 `PATCH /api/calls/{callId}/reopen`. ⚠️ Falta impedirlo si la convocatoria ya fue sorteada ([RN-19](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)) |
| FA-04 | 2 | El administrador desea eliminar una convocatoria | El sistema pide confirmación y la elimina | Paso 2 | 🟢 `DELETE /api/calls/{callId}`, modal de confirmación |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Regla | Estado v1 |
|--------|:----:|-----------|-----------------------|-------|-----------|
| FE-01 | 6 | Falta el título | Rechaza (`400 CALL_TITLE_REQUIRED`) | RN-22 | 🟢 |
| FE-02 | 6 | Faltan fechas, la fecha de fin es anterior a la de inicio o, al crear, la fecha de inicio ya pasó | Rechaza (`400 CALL_INVALID_DATES`) | RN-22 | 🟢 |
| FE-03 | 6 | Los cupos faltan o son menores que 1 | Rechaza (`400 CALL_INVALID_SLOTS`) | RN-22, RN-14 | 🟢 |
| FE-04 | FA-01 a FA-04 | La convocatoria no existe | Rechaza (`404 CALL_NOT_FOUND`) | RN-01 | 🟢 |
| FE-05 | FA-01 | La convocatoria está cerrada manualmente | Rechaza la modificación (`409 CALL_CLOSED`) | RN-23 | 🟢 |
| FE-06 | FA-02 | La convocatoria ya está cerrada manualmente | Rechaza (`409 CALL_ALREADY_CLOSED`) | RN-23 | 🟢 |
| FE-07 | FA-03 | La convocatoria no está cerrada manualmente | Rechaza (`409 CALL_NOT_CLOSED`) | RN-23 | 🟢 |
| FE-08 | FA-04 | La convocatoria tiene solicitudes | Rechaza la eliminación (`409 CALL_HAS_APPLICATIONS`) | RN-24 | 🟢 |
| FE-09 | 3 | El usuario no tiene `ROLE_ADMIN` | Rechaza la escritura (`403`); el frontend oculta las rutas de administración | [RNF-02](../03-atributos-de-calidad-y-rnf.md) | 🟢 |

## Reglas de negocio aplicables
RN-01, RN-14, RN-15, RN-19 y RN-22 a RN-25 ([Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-02 Seguridad | Solo `ROLE_ADMIN` crea, modifica, cierra, reabre o elimina convocatorias (`SecurityConfig`) |
| RNF-07 Usabilidad | Estado visible en cada convocatoria (Abierta, Programada, Cerrada) |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| ✏️ EUV por crear · Publicar convocatoria | Flujo básico, FA-01 a FA-04, FE-01 a FE-09 | Pendiente; `test_api.py` no cubre los endpoints de administración |
