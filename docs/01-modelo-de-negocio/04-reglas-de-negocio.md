# Reglas de negocio

Reglas **implementadas** en el backend (fuente: `ApplicationServiceImpl`, `DocumentServiceImpl`, `DocumentType`, `Application`).

| ID | Regla | Dónde se aplica | Código de error |
|----|-------|-----------------|-----------------|
| RN-01 | La convocatoria debe existir | `ApplicationServiceImpl` | `CALL_NOT_FOUND` (HTTP 404) |
| RN-02 | El residente debe existir | `ApplicationServiceImpl` | `RESIDENT_NOT_FOUND` |
| RN-03 | El residente debe tener una unidad privada asociado | `ApplicationServiceImpl` | `APARTMENT_NOT_FOUND` |
| RN-04 | Una unidad privada solo puede tener **una** solicitud por convocatoria | Servicio + restricción `uk_application_apartment_call` | `APPLICATION_ALREADY_EXISTS` (HTTP 409) |
| RN-05 | Todos los documentos referenciados deben existir | `ApplicationServiceImpl` | `DOCUMENT_NOT_FOUND` |
| RN-06 | Todos los documentos deben estar en estado `UPLOADED` | `ApplicationServiceImpl` | `DOCUMENT_NOT_UPLOADED` |
| RN-07 | Deben adjuntarse todos los documentos obligatorios | `ApplicationServiceImpl` | `REQUIRED_DOCUMENTS_MISSING` |
| RN-08 | Solo se aceptan archivos `application/pdf` | `DocumentServiceImpl` | `INVALID_CONTENT_TYPE` |
| RN-09 | Tamaño máximo por archivo: 2 MB | `DocumentServiceImpl` | `FILE_TOO_LARGE` |
| RN-10 | El tipo de documento debe pertenecer al catálogo | `DocumentServiceImpl` | `INVALID_DOCUMENT_TYPE` |
| RN-11 | Un documento solo se marca `UPLOADED` si el archivo existe en S3 | `DocumentServiceImpl` | `FILE_NOT_FOUND_IN_S3` |
| RN-12 | La URL prefirmada expira a los 5 minutos | `aws.s3.presign-duration-minutes` | — |
| RN-13 | Toda solicitud nueva inicia en estado `REGISTERED` | `ApplicationServiceImpl` | — |

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
| RN-14 | El número de cupos se define en cada convocatoria, según la capacidad de parqueadero comunal del conjunto | SUP-02 | [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | 🟡 El modelo ya tiene `availableSlots` en la convocatoria; falta el flujo de publicación |
| RN-15 | El periodo de vigencia de la asignación (3, 4 o 6 meses, u otro) se define en cada convocatoria | SUP-03 | [C.U-01](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md) | 🔴 Por implementar |
| RN-16 | El residente debe estar a paz y salvo con la administración para presentar una solicitud | Diagrama *Flujos de Postulación* | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) | 🔴 Por implementar (la interfaz muestra un estado fijo "Paz y Salvo") |

## Ciclos de vida

### Solicitud (`ApplicationStatus`)
> ✏️ **Diagrama por agregar:** `![Estados de la postulación](../assets/imagenes/negocio-estados-postulacion.png)`

### Documento (`DocumentStatus`)
> ✏️ **Diagrama por agregar:** `![Estados del documento](../assets/imagenes/negocio-estados-documento.png)`
