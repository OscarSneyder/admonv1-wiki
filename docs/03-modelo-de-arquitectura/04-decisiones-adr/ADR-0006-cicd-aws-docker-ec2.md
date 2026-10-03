# ADR-0006 · CI/CD con AWS CodePipeline y Docker en EC2

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️
- **Relacionados:** [ADR-0007](ADR-0007-despliegue-in-place-ec2-persistente.md), [ADR-0008](ADR-0008-nginx-punto-unico-de-entrada-https.md), [MEC-04](../03-mecanismos-de-diseno/MEC-04-despliegue-automatizado-verificado.md)
- **Escenario:** [EUV-Q-04](../../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md)

## Contexto
Se requiere desplegar el backend de forma repetible y verificada en la nube AWS.

## Decisión
Pipeline orquestado por **CodePipeline** (`Source → Build → Test → Deploy`) a partir de la rama `main` de `ByteCscript/admonv1-backend`:
- **CodeBuild** (`buildspec.yml`): compila con Maven (Corretto 21) y genera `app.jar`.
- **CodeBuild** (`buildspec-api-test.yml`): arranca el mismo JAR con H2 y ejecuta `pytest` (`test_create_application_end_to_end`: flujo de integración de 8 pasos).
- **CodeDeploy** (`appspec.yml`): copia el artefacto a EC2 y, mediante hooks, reconstruye la imagen Docker (`eclipse-temurin:21-jre`) y el contenedor, validando `/api/health` hasta 50 intentos con 1 s de pausa, dentro del límite de 60 s del hook en `appspec.yml`.
- La configuración sensible se monta desde el host (`/config/application.properties`, solo lectura).

## Rationale de las decisiones
| Decisión | Implementación | Rationale |
|----------|----------------|-----------|
| Orquestación CI/CD | CodePipeline | Centraliza y secuencia Source → Build → Test → Deploy |
| Compilación | CodeBuild | Evita usar EC2 como máquina de build y separa build de runtime |
| Pruebas antes de producción | CodeBuild Test | Permite bloquear el despliegue cuando una validación falla |
| Promoción del artefacto | BuildArtifact | El mismo `app.jar` probado es el que se entrega a Deploy |
| Despliegue | CodeDeploy | Automatiza la actualización de la instancia existente mediante hooks |
| Cómputo productivo | EC2 existente | Evita crear infraestructura adicional por cada release ([ADR-0007](ADR-0007-despliegue-in-place-ec2-persistente.md)) |
| Estrategia | In-place | Adecua complejidad y costo a una solución de una sola instancia ([ADR-0007](ADR-0007-despliegue-in-place-ec2-persistente.md)) |
| Runtime | Docker | Mantiene un entorno de ejecución reproducible |
| Entrada pública | Nginx HTTPS | Centraliza TLS y mantiene el backend sin exposición directa ([ADR-0008](ADR-0008-nginx-punto-unico-de-entrada-https.md)) |
| S3 para pruebas | Prefijo `ci-tests/` | Aísla los objetos de CI de `documents/` en producción |
| IAM para CI | Permisos sobre `ci-tests/*` | Aplica mínimo privilegio en lugar de acceso general al bucket |
| Validación posterior al despliegue | `/api/health` | Comprueba que la aplicación realmente quedó disponible |
| Selenium / E2E | No incluido actualmente | Se mantiene separado de las pruebas API y queda como evolución futura |

## Alternativas
| Alternativa | Motivo de descarte |
|-------------|-------------------|
| GitHub Actions + SSH | ✏️ |
| Compilar en la instancia EC2 | Mezcla build y runtime en el servidor de producción |
| ECS / Fargate | ✏️ mayor complejidad y costo |
| Elastic Beanstalk | ✏️ |

## Consecuencias
- ➕ Despliegue automatizado con una puerta de calidad (pruebas API) y un health check.
- ➖ La imagen se construye en la instancia de destino (no hay registro de imágenes, p. ej. ECR).
- ➖ Despliegue *in-place* con tiempo de inactividad durante el reemplazo del contenedor.
- ✏️ Pipeline equivalente para el frontend.
