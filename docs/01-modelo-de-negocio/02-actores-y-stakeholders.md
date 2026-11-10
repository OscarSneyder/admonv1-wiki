# Actores y stakeholders

## Actores de negocio
| Actor | Servicios de negocio |
|-------|----------------------|
| Copropietario | SN-01 a SN-05 ([catálogo](05-servicios-de-negocio.md)) |
| Residente | SN-06 a SN-08 ([catálogo](05-servicios-de-negocio.md)) |

## Actores del sistema
| Actor | Tipo | Descripción | Interacciones |
|-------|------|-------------|---------------|
| Residente | Humano primario | Usuario con `ROLE_RESIDENT` y unidad privada asociada | Login, consultar convocatorias, cargar documentos, presentar, consultar y cancelar solicitudes, consultar el resultado de sus solicitudes en el sorteo (C.U-02, C.U-03, C.U-04, C.U-06) |
| Administrador | Humano | Usuario con `ROLE_ADMIN` | Publicar y administrar convocatorias, revisar (aceptar o rechazar) solicitudes y ejecutar el sorteo de cupos (C.U-01, C.U-07, C.U-08), implementados en v1; configurar zonas comunes (C.U-09) por implementar. No tiene las opciones del residente: *Convocatorias* ni *Mis Postulaciones* (RN-40) |
| AWS S3 | Sistema externo | Almacenamiento de documentos | Recibe archivos vía URL prefirmada; el backend verifica su existencia |
| Pipeline AWS | Sistema externo | CodePipeline / CodeBuild / CodeDeploy | Construye, prueba y despliega el backend |

## Diagrama de casos de uso
Ver el [mapa de casos de uso](../02-modelo-de-capacidades/01-mapa-de-casos-de-uso.md) en el Modelo de capacidades.

## Stakeholders
| Stakeholder | Interés / preocupación | Atributo de calidad asociado |
|-------------|------------------------|-----------------------------|
| Residentes | Presentar una solicitud fácil y rápido; que la solicitud quede registrada | Usabilidad, disponibilidad, rendimiento |
| Administración del conjunto | Equidad (1 por unidad privada), documentos válidos | Integridad, seguridad |
| Docente / evaluador | Arquitectura clara y justificada | Mantenibilidad, trazabilidad |
| Equipo de desarrollo | Despliegue automatizado y confiable | Desplegabilidad, testabilidad |
