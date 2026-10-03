# MEC-04 · Despliegue automatizado y verificado

| Campo | Valor |
|-------|-------|
| RNF de origen | RNF-04 Desplegabilidad |
| Versión / estado | 1.0 · Formalizado |
| Decisiones relacionadas | [ADR-0006](../04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md), [ADR-0007](../04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md) |
| Componentes involucrados | CodePipeline, CodeBuild (Build y Test), BuildArtifact, CodeDeploy, CodeDeploy Agent, EC2, Docker |
| EUV que lo valida | [EUV-Q-04](../../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md) |

## Regla de aplicación
Todo cambio del backend que llega a `main` se libera únicamente a través del pipeline, siguiendo estas reglas:

1. **Construir una vez:** el `app.jar` se compila una sola vez en la etapa Build y viaja en el BuildArtifact.
2. **Probar el mismo artefacto:** la etapa Test ejecuta las pruebas de integración API sobre ese mismo `app.jar`.
3. **Barrera de calidad:** `TEST PASS → DEPLOY`; `TEST FAIL → PIPELINE DETENIDO`.
4. **Promover sin recompilar:** CodeDeploy despliega el mismo BuildArtifact que superó las pruebas.
5. **Verificar después de desplegar:** `ValidateService` consulta `/api/health` hasta 50 intentos con 1 s de pausa, dentro del límite de 60 s del hook en `appspec.yml`; sin respuesta, el despliegue falla.

## Responsabilidades técnicas
| Componente | Responsabilidad |
|------------|-----------------|
| CodePipeline | Secuenciar las etapas y detener el flujo ante un fallo |
| CodeBuild – Build | Producir el BuildArtifact |
| CodeBuild – Test | Emitir PASS o FAIL sobre el artefacto |
| CodeDeploy | Ejecutar los hooks de `appspec.yml` en la instancia |

## Condiciones y restricciones
- Despliegue *in-place* sobre una única instancia: posible ventana breve de indisponibilidad ([ADR-0007](../04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md)).
- La etapa Test no incluye pruebas de navegador (Selenium/E2E).

## Criterio de incorporación correcta
- No existen despliegues a producción por fuera del pipeline.
- Una prueba fallida en Test impide la etapa Deploy (verificable en el historial de CodePipeline y en CloudWatch Logs).
- Un despliegue cuyo `/api/health` no responde tras 50 intentos (o al vencer el límite de 60 s del hook) queda en estado FAILED.
