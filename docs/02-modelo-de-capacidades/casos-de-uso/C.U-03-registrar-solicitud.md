# C.U-03 · Registrar solicitud

| Campo | Valor |
|-------|-------|
| Paquete | Solicitudes |
| Actor primario | Residente |
| Actores secundarios | AWS S3 (almacenamiento de documentos) |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 4 |
| Objetivo del actor | Presentar una solicitud a la convocatoria de parqueadero comunal para participar en la asignación de cupos |
| Resultado observable | Solicitud registrada en la base de participantes, con número único y estado `REGISTERED` |
| Requisitos funcionales relacionados | RF-04, RF-05, RF-06, RF-07 |
| Estado | 🟡 Implementado parcialmente (ver columna *Estado v1*) |

> **Fuente de los flujos:** diagrama *Flujos de Postulación* del equipo (flujo básico y dos flujos de control de excepciones). Los flujos alternativos y las excepciones FE-03 a FE-10 se identificaron en la implementación v1. La columna *Estado v1* compara cada paso con el código actual.
>
> **Decisión del equipo:** por ahora no se piden los datos del vehículo (paso *Ingresar datos de vehículo* del diagrama); solo se cargan sus documentos.

![Flujos de la solicitud (solicitud)](../../assets/imagenes/flujos-solicitud-cu-03.jpg)

## Precondiciones
1. El residente inició sesión ([EUV-01](../../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)).
2. Existe una convocatoria con cupos disponibles ([C.U-02](C.U-02-consultar-convocatoria.md)).
3. El residente tiene una unidad privada asociada.

## Postcondiciones
1. La solicitud queda registrada en estado `REGISTERED`, con número `POST-<UUID>` y fecha de creación.
2. Los documentos cargados quedan vinculados a la solicitud.
3. La unidad privada no puede registrar otra solicitud en la misma convocatoria.

## Flujo básico
| Paso | Actor / Sistema | Acción | Estado v1 |
|:----:|-----------------|--------|-----------|
| 1 | Residente | Ingresa al sistema | 🟢 Inicio de sesión con JWT |
| 2 | Residente | Ingresa a la convocatoria y elige presentar una solicitud | 🟢 La opción solo está habilitada si la convocatoria tiene cupos disponibles (`availableSlots > 0`) |
| 3 | Residente | Ingresa sus datos de residente | 🟡 Los datos se muestran fijos (`constants.js`); no se capturan ni se consultan del backend |
| 4 | Residente | Carga los documentos del vehículo y del residente (licencia de tránsito, SOAT y licencia de conducción obligatorios; revisión técnico-mecánica opcional) | 🟢 Carga directa a S3 ([RNF-05](../03-atributos-de-calidad-y-rnf.md#diagramas-de-secuencia-de-rnf)) |
| 5 | Residente | Ingresa los datos de la unidad privada | 🟡 La unidad se toma del residente autenticado; no se ingresa |
| 6 | Residente | Envía la solicitud | 🟢 `POST /api/applications` |
| 7 | Sistema | Valida que el residente esté a paz y salvo con la administración | 🔴 No implementado ([RN-16](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar)) |
| 8 | Sistema | Valida que la unidad privada tenga una única participación en la convocatoria | 🟢 [RN-04](../../01-modelo-de-negocio/04-reglas-de-negocio.md) |
| 9 | Sistema | Valida los documentos (existen, cargados y obligatorios completos) y registra la solicitud en la base de participantes | 🟢 [RN-05 a RN-07, RN-13](../../01-modelo-de-negocio/04-reglas-de-negocio.md) |
| 10 | Sistema | Muestra la confirmación con el número de la solicitud. Fin | 🟢 |

## Flujos alternativos
> ⚠️ **Por confirmar con Negocio:** identificados en la implementación v1; el diagrama de flujos no incluye flujos alternativos.

| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | 4 | El vehículo no está a nombre del residente | El residente adjunta además el soporte de tenencia (documento opcional del catálogo) | Paso 4 |
| FA-02 | 4 | El residente desea reemplazar un documento ya cargado | Selecciona un nuevo archivo para el mismo tipo; el sistema reemplaza la carga anterior | Paso 4 |
| FA-03 | 4 | El residente desea revisar sus datos antes de enviar | Regresa al paso anterior del formulario (botón *Atrás*) | Paso 3 |

## Flujos de excepción
| Código | Paso | Condición | Respuesta del sistema | Regla | Estado v1 |
|--------|:----:|-----------|-----------------------|-------|-----------|
| FE-01 | 7 | El residente no está a paz y salvo | Rechaza la solicitud e informa que debe estar a paz y salvo. Fin | [RN-16](../../01-modelo-de-negocio/04-reglas-de-negocio.md#reglas-identificadas-por-implementar) | 🔴 No implementado |
| FE-02 | 8 | La unidad privada ya tiene una solicitud en la convocatoria | Rechaza la solicitud por múltiple inscripción (`409 APPLICATION_ALREADY_EXISTS`). Fin | [RN-04](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟡 Implementado; ante solicitudes simultáneas responde 500 ([R-06](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) y el frontend muestra un mensaje genérico |
| FE-03 | 2 | La convocatoria no tiene cupos disponibles | No habilita la solicitud. Fin | — | 🟡 Se evalúa `availableSlots > 0`; el estado `OPEN/CLOSED` no se considera |
| FE-04 | 6 | La convocatoria no existe (`CALL_NOT_FOUND`) | Rechaza la solicitud. Fin | [RN-01](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-05 | 6 | El residente no tiene unidad privada asociada (`APARTMENT_NOT_FOUND`) | Rechaza la solicitud. Fin | [RN-03](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-06 | 4 | El archivo no es PDF (`INVALID_CONTENT_TYPE`) | Rechaza el archivo | [RN-08](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-07 | 4 | El archivo supera 2 MB (`FILE_TOO_LARGE`) | Rechaza el archivo | [RN-09](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-08 | 4 | El archivo no se encuentra en S3 al confirmar la carga (`FILE_NOT_FOUND_IN_S3`) | Marca la carga como fallida | [RN-11](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-09 | 9 | Un documento no terminó de cargarse (`DOCUMENT_NOT_UPLOADED`) | Rechaza la solicitud | [RN-06](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |
| FE-10 | 9 | Faltan documentos obligatorios (`REQUIRED_DOCUMENTS_MISSING`) | Rechaza la solicitud indicando los documentos faltantes | [RN-07](../../01-modelo-de-negocio/04-reglas-de-negocio.md) | 🟢 |

En FE-06 a FE-10 el residente puede corregir y volver al paso 4; FE-01 a FE-05 terminan el caso de uso.

## Reglas de negocio aplicables
RN-01, RN-03 a RN-13 y RN-16 ([Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

## RNF aplicables
| RNF | Aplicación en este C.U |
|-----|------------------------|
| RNF-01 Rendimiento | Pico de solicitudes al abrir la convocatoria |
| RNF-02 Seguridad | El residente se toma del JWT |
| RNF-03 Integridad | Una solicitud por unidad privada y convocatoria (FE-02) |
| RNF-05 Escalabilidad | Carga directa de documentos a S3 (paso 4) |

## EUV asociados
| EUV | Flujo que demuestra | Estado |
|-----|---------------------|--------|
| [EUV-03](../../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) | Verificación previa de única participación (paso 8, FE-02) | Implementado solo en el backend; el frontend no la invoca |
| [EUV-04](../../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) | Paso 4, FA-02, FE-06 a FE-08 | Implementado |
| [EUV-05](../../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | Flujo básico, FE-02, FE-04, FE-05, FE-09, FE-10 | Implementado |
| [EUV-Q-01](../../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) | Flujo básico bajo concurrencia | Implementado |
| [EUV-Q-03](../../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) | FE-02 ante solicitudes simultáneas | Implementado |
| [EUV-Q-05](../../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md) | Paso 4 | Implementado |
| ✏️ EUV por crear | FE-01 paz y salvo | Pendiente |
