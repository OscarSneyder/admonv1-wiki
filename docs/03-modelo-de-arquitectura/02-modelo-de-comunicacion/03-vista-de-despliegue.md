# Vista de despliegue

## Producción / nube (backend)
Fuente: `buildspec.yml`, `buildspec-api-test.yml`, `appspec.yml`, `Dockerfile`, `deploy/*.sh`.

> ✏️ **Diagrama por agregar:** `![Despliegue en producción](../../assets/imagenes/despliegue-produccion.png)`

## Nodos
| Nodo | Especificación | Software | Notas |
|------|----------------|----------|-------|
| EC2 | ✏️ tipo de instancia | Docker, Amazon Linux | IP usada en la prueba de carga: `100.26.48.222` |
| Contenedor `backend` | `eclipse-temurin:21-jre` | `app.jar` | `--restart unless-stopped`; puerto publicado solo en `127.0.0.1:8080` |
| Nginx | Proxy inverso en la instancia EC2 | Nginx | Termina HTTPS en :443 y redirige a `127.0.0.1:8080` ([ADR-0008](../04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md)) |
| S3 | bucket `aes-puj-documents` | — | región `us-east-1` |
| Frontend | ✏️ | build de Vite (`dist/`) | ⚠️ confirmar si se usa S3 + CloudFront, Nginx en EC2, Amplify, etc. |

## Acceso de producción
`Internet → HTTPS :443 → Nginx → 127.0.0.1:8080 → Spring Boot (Docker)`

El pipeline actualiza el backend sin cambiar el punto de entrada externo; los puertos internos no se exponen. Estrategia de despliegue: *in-place* ([ADR-0007](../04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md)). Pipeline completo: [Pipeline CI/CD](../../07-codigo-y-despliegue/02-pipeline-ci-cd.md).

## Ciclo de vida de CodeDeploy
| Hook | Script | Acción |
|------|--------|--------|
| `BeforeInstall` | `deploy/before-install.sh` | Detiene y elimina el contenedor y la imagen anteriores |
| Install | — | Copia el artefacto a `/home/ec2-user/backend` (OVERWRITE) |
| `AfterInstall` | `deploy/after-install.sh` | `docker build -t backend-app .` |
| `ApplicationStart` | `deploy/start.sh` | `docker run` con la configuración montada |
| `ValidateService` | `deploy/validate.sh` | Consulta `/api/health` hasta 50 intentos con 1 s de pausa, dentro del límite de 60 s del hook en `appspec.yml` |

## Entorno local
> ✏️ **Diagrama por agregar:** `![Despliegue local](../../assets/imagenes/despliegue-local.png)`
