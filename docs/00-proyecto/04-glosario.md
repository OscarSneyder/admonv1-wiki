# Glosario

| Término (ES) | Término en código | Definición |
|--------------|-------------------|-----------|
| Convocatoria | `Call` (back) / `Convocation` (front) | Proceso abierto durante un rango de fechas con un número de cupos (`availableSlots`) para parqueaderos comunales. Estados: `OPEN`, `CLOSED`. |
| Solicitud (en la implementación v1: Postulación) | `Application` | Solicitud de un residente, para su unidad privada (apartamento), a una convocatoria. Número con formato `POST-<UUID>`. |
| Residente | `User` con `ROLE_RESIDENT` | Usuario asociado a un apartamento que puede postularse. |
| Administrador | `User` con `ROLE_ADMIN` | Rol definido en los datos semilla; ✏️ funciones por definir. |
| Apartamento | `Apartment` | Unidad habitacional perteneciente a una torre. Máximo una postulación por convocatoria. |
| Torre | `Tower` | Agrupación de apartamentos. |
| Documento | `Document` | Archivo PDF del vehículo o del residente almacenado en S3. Estados: `PENDING`, `UPLOADED`, `FAILED`. |
| Tipo de documento | `DocumentType` | Catálogo: licencia de tránsito, SOAT, revisión técnico-mecánica, licencia de conducción, soporte de tenencia. |
| URL prefirmada | Presigned URL | URL temporal (5 min) con la que el navegador sube el archivo directamente a S3. |
| Elegibilidad | `check` | Verificación de si el residente puede postularse a una convocatoria. |
| JWT | — | Token firmado con HS256 que se emite en el login; expira en 1 h. |
| ADR | — | *Architecture Decision Record*: registro de una decisión arquitectónica. |
| QAS | — | *Quality Attribute Scenario*: escenario de atributo de calidad (fuente, estímulo, artefacto, entorno, respuesta, medida). |

## Términos de la metodología PAREX
| Término | Definición |
|---------|-----------|
| Servicio de negocio (SN) | Valor que el conjunto residencial entrega a un actor de negocio; no es una pantalla ni una funcionalidad del sistema. |
| Escenario de servicio (ESN) | Realización concreta de un servicio de negocio que recorre actores y casos de uso. |
| Modelo semántico | Conceptos del dominio y sus relaciones, que fijan el vocabulario común del producto. |
| Caso de uso (C.U) | Capacidad del sistema orientada a un objetivo del actor, con resultado observable; se documenta con flujo básico, alternativos y de excepción. |
| RNF | Requerimiento no funcional: condición de calidad bajo la cual una capacidad tiene valor. |
| Mecanismo de diseño (MEC) | Solución técnica estándar que responde a uno o varios RNF y que se aplica de forma uniforme en el sistema. |
| EUV | Escenario de Uso y Validación: situación significativa que demuestra un comportamiento del producto y se valida con evidencia. |
| Evidencia | Resultado observable de ejecutar una validación (log, captura, reporte, medición). |
| Tajada (TJ) | Intervención focalizada sobre el producto, pequeña para construir y suficientemente completa para validar. |
| RBS | Realización Básica del Servicio: alcance mínimo, de extremo a extremo, que demuestra el servicio de negocio. |
| Hebra | Línea de responsabilidad del desarrollo trenzado: Conducción, Producto, Arquitectura y desarrollo, Demostración. |
