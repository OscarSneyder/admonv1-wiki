# Configuración y entornos

## Entornos
| Entorno | Backend | Frontend | BD | Uso |
|---------|---------|----------|----|-----|
| Local | `localhost:8080` | `localhost:5173` (proxy `/api`) | H2 mem | Desarrollo |
| CI (CodeBuild, stage Test) | `127.0.0.1:8080` | — | H2 mem | Pruebas API; S3 bajo `ci-tests/` |
| Producción (EC2) | contenedor `backend`, `127.0.0.1:8080` detrás de Nginx HTTPS :443; S3 bajo `documents/` | ✏️ | H2 mem ⚠️ | Usuarios |

## Propiedades del backend
| Propiedad | Valor por defecto | Descripción | ¿Secreto? |
|-----------|-------------------|-------------|:---------:|
| `spring.datasource.url` | `jdbc:h2:mem:parexdb` | URL de BD (`${DB_URL}` previsto) | ✅ en prod |
| `spring.jpa.hibernate.ddl-auto` | `create` | Estrategia de esquema | — |
| `spring.h2.console.enabled` | `true` | Consola H2 (`/h2-console`) | — |
| `aws.s3.region` | `us-east-1` | Región S3 | — |
| `aws.s3.bucket-name` | `aes-puj-documents` | Bucket | — |
| `aws.s3.key-prefix` | `documents/` (producción) · `ci-tests/` (stage Test) | Prefijo de los objetos en el bucket | — |
| `aws.s3.presign-duration-minutes` | `5` | Vigencia de la URL prefirmada | — |
| `jwt.secret` | `{KEY_BASE64}` (placeholder) | Clave HS256 en Base64 | ✅ |
| `jwt.expiration` | `3600000` | Vigencia del token (ms) | — |
| `jwt.issuer` | `adminv1` | Emisor | — |
| `app.frontend.url` | `http://localhost:5173` | Origen permitido por CORS | — |

> **Configuración por entorno:** la configuración específica del stage Test se inyecta como variables de entorno (p. ej. `aws.s3.key-prefix=ci-tests/`, `jwt.secret=${JWT_SECRET}`), sin modificar el código fuente.

> **Regla:** los secretos nunca se versionan; se inyectan con `--spring.config.additional-location` (ver [ADR-0006](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md)).

## Frontend
| Variable | Descripción |
|----------|-------------|
| ✏️ `BASE_URL` / `VITE_*` | Documentar cómo se define la URL de la API en build de producción |
