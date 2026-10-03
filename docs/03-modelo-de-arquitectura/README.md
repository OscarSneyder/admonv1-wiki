# Modelo de arquitectura — Visión general

Esta sección documenta el **Modelo de Arquitectura del Software** de ADMON v1 según PAREX: el modelo de comunicación (vistas de composición, componentes y despliegue), los mecanismos de diseño, las decisiones (ADR) y las vistas complementarias de datos, contratos y conceptos transversales.

## Resumen arquitectónico
| Aspecto | Decisión | ADR |
|---------|----------|-----|
| Estilo global | Cliente-servidor: SPA + API REST monolítica | [ADR-0001](04-decisiones-adr/ADR-0001-spa-y-api-rest-monolitica.md) |
| Backend | Arquitectura en capas (Controller → Service → Repository → Model) | ADR-0001 |
| Frontend | Clean Architecture por *features* (domain / application / infrastructure / presentation) | [ADR-0002](04-decisiones-adr/ADR-0002-frontend-clean-architecture-por-features.md) |
| Seguridad | JWT HS256 sin estado (Spring OAuth2 Resource Server) | [ADR-0003](04-decisiones-adr/ADR-0003-autenticacion-jwt-stateless.md) |
| Archivos | Carga directa a S3 con URL prefirmada | [ADR-0004](04-decisiones-adr/ADR-0004-carga-directa-a-s3.md) |
| Persistencia | JPA; H2 en desarrollo y pruebas, PostgreSQL previsto | [ADR-0005](04-decisiones-adr/ADR-0005-persistencia-jpa-h2-postgresql.md) |
| Despliegue | AWS CodePipeline + CodeBuild + CodeDeploy, Docker en EC2; *in-place* detrás de Nginx HTTPS ([ADR-0007](04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md), [ADR-0008](04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md)) | [ADR-0006](04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md) |

## Vistas
| Vista | Pregunta que responde | Página |
|-------|-----------------------|--------|
| Drivers | ¿Qué fuerzas moldean la arquitectura? | [01-drivers-arquitectonicos](01-drivers-arquitectonicos.md) |
| Composición | ¿Con qué actores y sistemas se relaciona, qué unidades desplegables hay y cómo se comunican? | [01-vista-de-composicion](02-modelo-de-comunicacion/01-vista-de-composicion.md) |
| Componentes | ¿Cómo están organizados el backend y el frontend? | [02-vista-de-componentes](02-modelo-de-comunicacion/02-vista-de-componentes.md) |
| Datos | ¿Qué entidades y relaciones existen? | [05-modelo-de-datos](05-modelo-de-datos.md) |
| Despliegue | ¿Dónde se ejecuta cada cosa? | [03-vista-de-despliegue](02-modelo-de-comunicacion/03-vista-de-despliegue.md) |
| Transversal | Seguridad, errores, configuración, CORS | [07-conceptos-transversales](07-conceptos-transversales.md) |
| Mecanismos de diseño | ¿Qué soluciones estándar responden a los RNF? | [03-mecanismos-de-diseno](03-mecanismos-de-diseno/README.md) |
| Contratos | ¿Qué expone la API? | [06-contratos-api](06-contratos-api.md) |
| Arquitectura del backend | Stack, capas, estructura del proyecto y ejecución local del backend | [01-arquitectura-backend](08-diseno-detallado/01-arquitectura-backend.md) |
| Arquitectura del frontend | Estilo por features, capas de Clean Architecture y flujo de peticiones | [02-arquitectura-frontend](08-diseno-detallado/02-arquitectura-frontend.md) |
| Dinámica | ¿Cómo colaboran los componentes? | [Diagramas de secuencia de RNF](../02-modelo-de-capacidades/03-atributos-de-calidad-y-rnf.md#diagramas-de-secuencia-de-rnf) |

## Stack tecnológico
| Capa | Tecnología |
|------|-----------|
| Frontend | React 19, React Router 7, Vite 8 |
| Backend | Java 21, Spring Boot 4.1 (Web MVC, Data JPA, Security, OAuth2 Resource Server), Lombok, springdoc OpenAPI |
| Datos | H2 en memoria (actual) · PostgreSQL (previsto, configuración comentada) |
| Almacenamiento de archivos | AWS S3 (SDK v2) |
| CI/CD | AWS CodePipeline + CodeBuild + CodeDeploy, Docker en EC2 |
| Entrada pública | Nginx HTTPS :443 (proxy inverso) |
| Pruebas | pytest + requests (API), k6 (carga) |

## Inventario de componentes
Responsables según [Equipo y hebras](../00-proyecto/03-equipo-y-hebras.md).

| ID | Componente | Tipo | Tecnología | Repositorio | Responsable |
|----|-----------|------|-----------|-------------|-------------|
| CMP-01 | Frontend SPA | Contenedor | React 19 + Vite | admonv1-frontend | Johan Gil |
| CMP-02 | Backend API | Contenedor | Spring Boot 4.1 / Java 21 | admonv1-backend | Cristian Gómez |
| CMP-03 | Base de datos | Contenedor | H2 (mem) / PostgreSQL | — | Cristian Gómez |
| CMP-04 | Almacén de documentos | Servicio externo | AWS S3 `aes-puj-documents` | — | Johan Gil y Cristian Gómez |
| CMP-05 | Pipeline CI/CD | Servicio externo | AWS CodePipeline / CodeBuild / CodeDeploy | admonv1-backend | Johan Gil y Cristian Gómez |
