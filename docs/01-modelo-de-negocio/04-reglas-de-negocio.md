# Reglas de negocio

Reglas **implementadas** en el backend (fuente: `ApplicationServiceImpl`, `CallServiceImpl`, `DocumentServiceImpl`, `DocumentType`, `Application`; revisión del código de `develop`: 2026-11-07).

| ID | Regla | Dónde se aplica | Código de error |
|----|-------|-----------------|-----------------|
| RN-01 | La convocatoria debe existir | `ApplicationServiceImpl` | `CALL_NOT_FOUND` (HTTP 404) |
| RN-02 | El residente debe existir | `ApplicationServiceImpl` | `RESIDENT_NOT_FOUND` |
| RN-03 | El residente debe tener una unidad privada asociado | `ApplicationServiceImpl` | `APARTMENT_NOT_FOUND` |
| RN-04 | Una unidad privada solo puede tener **una** solicitud por convocatoria | `ApplicationServiceImpl`. Cuenta las solicitudes `PENDING_VALIDATION` y `APPROVED`. ⚠️ Desde 2026-10-02 no hay restricción única en BD ([R-09](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) | `APPLICATION_ALREADY_EXISTS` (HTTP 409) |
| RN-05 | Todos los documentos referenciados deben existir | `ApplicationServiceImpl` | `DOCUMENT_NOT_FOUND` |
| RN-06 | Todos los documentos deben estar en estado `UPLOADED` | `ApplicationServiceImpl` | `DOCUMENT_NOT_UPLOADED` |
| RN-07 | Deben adjuntarse todos los documentos obligatorios | `ApplicationServiceImpl` | `REQUIRED_DOCUMENTS_MISSING` |
| RN-08 | Solo se aceptan archivos `application/pdf` | `DocumentServiceImpl` | `INVALID_CONTENT_TYPE` |
| RN-09 | Tamaño máximo por archivo: 2 MB | `DocumentServiceImpl` | `FILE_TOO_LARGE` |
| RN-10 | El tipo de documento debe pertenecer al catálogo | `DocumentServiceImpl` | `INVALID_DOCUMENT_TYPE` |
| RN-11 | Un documento solo se marca `UPLOADED` si el archivo existe en S3 | `DocumentServiceImpl` | `FILE_NOT_FOUND_IN_S3` |
| RN-12 | La URL prefirmada expira a los 5 minutos | `aws.s3.presign-duration-minutes` | — |
| RN-13 | Toda solicitud nueva inicia en estado `PENDING_VALIDATION` | `ApplicationServiceImpl` | — |
| RN-22 | La convocatoria exige título, fecha de inicio, fecha de fin no anterior a la de inicio y al menos 1 cupo; al crearla, la fecha de inicio no puede haber pasado | `CallServiceImpl.validateRequest` | `CALL_TITLE_REQUIRED`, `CALL_INVALID_DATES`, `CALL_INVALID_SLOTS` (HTTP 400) |
| RN-23 | Una convocatoria cerrada manualmente no se puede modificar ni volver a cerrar; solo se puede reabrir si está cerrada manualmente | `CallServiceImpl` | `CALL_CLOSED`, `CALL_ALREADY_CLOSED`, `CALL_NOT_CLOSED` (HTTP 409) |
| RN-24 | Una convocatoria con solicitudes no se puede eliminar | `CallServiceImpl.deleteCall` | `CALL_HAS_APPLICATIONS` (HTTP 409) |
| RN-25 | El estado de la convocatoria se deriva, en zona America/Bogota: `CLOSED` si se cerró manualmente o pasó su fecha de fin, `SCHEDULED` antes de la fecha de inicio y `OPEN` en otro caso | `CallServiceImpl.deriveStatus` | — |
| RN-26 | Solo el residente que creó la solicitud puede cancelarla | `ApplicationServiceImpl.cancelApplication` | `APPLICATION_FORBIDDEN` |
| RN-27 | Solo se puede cancelar una solicitud en estado `PENDING_VALIDATION` | `ApplicationServiceImpl.cancelApplication` | `APPLICATION_CANNOT_BE_CANCELLED` |
| RN-28 | Solo se puede cancelar una solicitud dentro del periodo de la convocatoria | `ApplicationServiceImpl.cancelApplication` | `APPLICATION_PERIOD_CLOSED` |
| RN-29 | Una convocatoria cerrada manualmente no admite solicitudes nuevas | `ApplicationServiceImpl.createApplication` | `CALL_CLOSED` (HTTP 409) |
| RN-30 | Solo `ROLE_ADMIN` lista y revisa solicitudes en la administración | `SecurityConfig` | HTTP 403 |
| RN-31 | Las transiciones de revisión válidas son `PENDING_VALIDATION` → `APPROVED`, `PENDING_VALIDATION` → `REJECTED` y `APPROVED` → `REJECTED` (revocación) | `AdminApplicationServiceImpl` | `APPLICATION_INVALID_STATE` (HTTP 409) |
| RN-32 | El motivo del rechazo es obligatorio y tiene máximo 500 caracteres | `AdminApplicationServiceImpl.rejectApplication` | `REJECTION_REASON_REQUIRED`, `REJECTION_REASON_TOO_LONG` (HTTP 400) |
| RN-33 | Cada revisión registra, en la misma transacción, el administrador, la fecha y hora y el motivo del rechazo; aprobar deja el motivo vacío | `AdminApplicationServiceImpl` | — |
| RN-34 | Una convocatoria sorteada no admite cambios de revisión en sus solicitudes | `AdminApplicationServiceImpl` | `CALL_ALREADY_DRAWN` (HTTP 409) |
| RN-35 | El administrador consulta los documentos con una URL prefirmada de lectura, válida por 15 minutos; el documento debe pertenecer a la solicitud y existir en S3 | `DocumentServiceImpl.generateReadUrl`, `aws.s3.presign-read-minutes` | `DOCUMENT_NOT_FOUND`, `DOCUMENT_NOT_IN_APPLICATION`, `FILE_NOT_FOUND_IN_S3` (HTTP 404) |
| RN-36 | El criterio para aceptar una solicitud es, principalmente, la validación manual que hace el administrador de los documentos cargados por el residente | Procedimiento del administrador; `AdminApplicationDetailPage` lo recuerda | — |
| RN-37 | El sorteo solo se ejecuta cuando ninguna solicitud de la convocatoria está `PENDING_VALIDATION` | `DrawTransactions` | `APPLICATIONS_PENDING_VALIDATION` (HTTP 409) |
| RN-38 | El residente consulta en su solicitud el motivo por el que fue rechazada | `ApplicationServiceImpl`, `ApplicationDetailPage` | — |

> ⚠️ **Numeración en el código:** los comentarios del backend citan reglas con otra numeración (por ejemplo, "RN-06: solo ADMIN escribe convocatorias", "RN-07: validación de título, fechas y cupos", "RN-09: estado derivado"), que no coincide con esta página. La numeración de referencia es la de esta wiki; conviene alinear los comentarios del código.

## Catálogo de documentos
| Tipo (`DocumentType`) | Nombre | Obligatorio | Formato | Máx. |
|----|----|:--:|----|----|
| `LICENCIA_TRANSITO` | Licencia de tránsito | ✅ | PDF | 2 MB |
| `SOAT_VIGENTE` | SOAT vigente | ✅ | PDF | 2 MB |
| `REVISION_TECNICOMECANICA` | Revisión técnico-mecánica vigente | ❌ | PDF | 2 MB |
| `LICENCIA_CONDUCCION` | Licencia de conducción del residente | ✅ | PDF | 2 MB |
| `SOPORTE_TENENCIA` | Soporte de tenencia (si el vehículo no está a nombre del residente) | ❌ | PDF | 2 MB |

## Reglas identificadas por implementar
Derivadas de los [supuestos del conjunto de referencia](01-necesidad-del-negocio.md#supuestos-del-conjunto-de-referencia) y de los flujos de los casos de uso.

| ID | Regla | Origen | Caso de uso | Estado |
|----|-------|--------|-------------|--------|
| RN-14 | El número de cupos se define en cada convocatoria, según la capacidad de parqueadero comunal del conjunto | SUP-02 | [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | 🟢 Implementado: `totalSlots` se define al publicar la convocatoria (C.U-01, RN-22) y es un valor fijo |
| RN-15 | El periodo de vigencia de la asignación (3, 4 o 6 meses, u otro) se define en cada convocatoria | SUP-03 | [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | 🔴 Por implementar |
| RN-16 | El residente debe estar a paz y salvo con la administración para presentar una solicitud | Diagrama *Flujos de Postulación* | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | 🔴 Por implementar (la interfaz muestra un estado fijo "Paz y Salvo") |
| RN-17 | En el sorteo participan solo las solicitudes `APPROVED` de la convocatoria, y debe haber al menos una (`NO_APPROVED_APPLICATIONS`). Si son iguales o menos que los cupos, todas quedan `SELECTED` (asignación directa, sin lista de espera) | Decisión del equipo (2026-11-07, actualizada el 2026-11-09), SUP-02 | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | 🟢 Implementado |
| RN-18 | El sorteo solo se ejecuta con la convocatoria cerrada, por cierre manual o por fecha de fin vencida (`CALL_NOT_CLOSED`) | Decisión del equipo (2026-11-07) | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | 🟢 Implementado |
| RN-19 | Cada convocatoria tiene como máximo un sorteo; no se puede repetir, y la convocatoria sorteada no se puede reabrir (`DRAW_ALREADY_EXECUTED`) | Decisión del equipo (2026-11-07) | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md), [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | 🟢 Implementado |
| RN-20 | El sorteo asigna a cada participante una posición única; las posiciones 1 a *cupos* quedan `SELECTED` y las demás `NOT_SELECTED`, en lista de espera en ese orden para cubrir renuncias | Decisión del equipo (2026-11-07) | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | 🟢 Implementado |
| RN-21 | Cada sorteo deja un registro de auditoría inmodificable (lista de entrada, *hash*, semilla, algoritmo, resultado, fecha y administrador) que permite reproducirlo y verificar que fue justo | Decisión del equipo (2026-11-07) | [C.U-08](../02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md) | 🟢 Implementado |

## Modelo de cupos
> **Decisión del equipo (2026-11-09):** los cupos de una convocatoria (`totalSlots`) son el número fijo de parqueaderos que se asignan al final del proceso.
>
> - **Registrar una solicitud** no consume cupos, y puede haber más solicitudes que cupos.
> - **Aprobar una solicitud** solo la habilita para participar. **Rechazarla o revocarla** la saca del sorteo, sin devolver ningún cupo.
> - **El cupo se asigna únicamente al ejecutar el sorteo** (C.U-08). Ese paso tampoco modifica `totalSlots`; los cupos que no se asignan quedan registrados en la auditoría (`unassignedSlots`).
> - **Una convocatoria acepta solicitudes según su estado** (RN-25), no según sus cupos.
>
> Esta decisión reemplaza el modelo de "cupos restantes", introducido durante la integración del panel de revisión, en el que aprobar descontaba un cupo y revocar lo devolvía. Ese modelo impedía aprobar más solicitudes que cupos y, por lo tanto, ejecutar el sorteo.

## Ciclos de vida

### Solicitud (`ApplicationStatus`)
> ✏️ **Diagrama por agregar:** `![Estados de la postulación](../assets/imagenes/negocio-estados-postulacion.png)`

### Documento (`DocumentStatus`)
> ✏️ **Diagrama por agregar:** `![Estados del documento](../assets/imagenes/negocio-estados-documento.png)`
