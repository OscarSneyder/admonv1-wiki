# Drivers arquitectónicos

## Objetivos de negocio
Ver [Visión del proyecto](../00-proyecto/01-vision-del-proyecto.md).

## Requisitos funcionales clave
RF-04 (elegibilidad), RF-06 (carga de documentos), RF-07 (solicitud). Ver [Requisitos funcionales](../02-modelo-de-capacidades/02-requisitos-funcionales.md).

## Atributos de calidad priorizados
| Prioridad | Atributo | Escenario |
|-----------|----------|-----------|
| 1 | Rendimiento bajo pico | [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) |
| 2 | Integridad (unicidad) | [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) |
| 3 | Seguridad | [EUV-Q-02](../04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md) |
| 4 | Desplegabilidad | [EUV-Q-04](../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md) |
| 5 | Escalabilidad de archivos | [EUV-Q-05](../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md) |
| 6 | Disponibilidad 20/7 | RNF-08 (EUV por crear) |

## Restricciones
| ID | Restricción | Tipo | Origen |
|----|------------|------|--------|
| RES-01 | Nube AWS (S3, EC2, CodePipeline) | Técnica | [ADR-0006](04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md) |
| RES-02 | Java 21 + Spring Boot en el backend | Técnica | [ADR-0001](04-decisiones-adr/ADR-0001-spa-y-api-rest-monolitica.md) |
| RES-03 | React en el frontend | Técnica | [ADR-0001](04-decisiones-adr/ADR-0001-spa-y-api-rest-monolitica.md) |
| RES-04 | Solo documentos PDF de hasta 2 MB | Negocio | Reglas de negocio |
| RES-05 | ✏️ Plazo / tamaño del equipo | Organizacional | ✏️ |

## Supuestos
- En v1 la administración carga las convocatorias directamente en la base de datos (datos semilla). Se hace así para avanzar con la tajada inicial sin depender de [C.U-01 Publicar convocatoria](../02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md), que se incorpora en la tajada TJ-02.
- Supuestos de negocio del conjunto de referencia (SUP-01 a SUP-05): ver [Necesidad del negocio](../01-modelo-de-negocio/01-necesidad-del-negocio.md#supuestos-del-conjunto-de-referencia).
