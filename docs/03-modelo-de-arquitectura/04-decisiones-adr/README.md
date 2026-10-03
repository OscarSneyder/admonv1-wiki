# Registro de decisiones arquitectónicas (ADR)

Formato basado en Michael Nygard. Plantilla: [../../plantillas/adr.md](../../plantillas/adr.md).

## Estados
`Propuesto` → `Aceptado` → (`Deprecado` | `Reemplazado por ADR-XXXX`) · o `Rechazado`

## Índice
| ADR | Título | Estado | Fecha | Atributos |
|-----|--------|--------|-------|-----------|
| [ADR-0001](ADR-0001-spa-y-api-rest-monolitica.md) | SPA + API REST monolítica en capas | Aceptado | ✏️ | Mantenibilidad, simplicidad |
| [ADR-0002](ADR-0002-frontend-clean-architecture-por-features.md) | Frontend con Clean Architecture por features | Aceptado | ✏️ | Mantenibilidad, testabilidad |
| [ADR-0003](ADR-0003-autenticacion-jwt-stateless.md) | Autenticación JWT sin estado | Aceptado | ✏️ | Seguridad, escalabilidad |
| [ADR-0004](ADR-0004-carga-directa-a-s3.md) | Carga directa a S3 con URL prefirmada | Aceptado | ✏️ | Rendimiento, escalabilidad |
| [ADR-0005](ADR-0005-persistencia-jpa-h2-postgresql.md) | Persistencia JPA; H2 en dev, PostgreSQL previsto | Aceptado (parcial) | ✏️ | Portabilidad, integridad |
| [ADR-0006](ADR-0006-cicd-aws-docker-ec2.md) | CI/CD con AWS CodePipeline y Docker en EC2 | Aceptado | ✏️ | Desplegabilidad |
| [ADR-0007](ADR-0007-despliegue-in-place-ec2-persistente.md) | Despliegue in-place sobre una única EC2 persistente | Aceptado | ✏️ | Desplegabilidad, costo |
| [ADR-0008](ADR-0008-nginx-punto-unico-de-entrada-https.md) | Nginx como punto único de entrada HTTPS | Aceptado | ✏️ | Seguridad |
