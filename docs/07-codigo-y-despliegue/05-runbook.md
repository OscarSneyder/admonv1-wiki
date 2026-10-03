# Runbook de operación

## Comandos en la instancia EC2
| Acción | Comando |
|--------|---------|
| Ver el estado | `docker ps -f name=backend` |
| Ver logs | `docker logs -f backend` |
| Salud | `curl http://127.0.0.1:8080/api/health` |
| Reiniciar | `docker restart backend` |
| Redesplegar manualmente | `cd /home/ec2-user/backend && bash deploy/before-install.sh && bash deploy/after-install.sh && bash deploy/start.sh && bash deploy/validate.sh` |

## Incidentes comunes
| Incidente | Diagnóstico | Acción |
|-----------|------------|--------|
| Pipeline detenido en la etapa Test | CloudWatch Logs de CodeBuild | Corregir en `develop` y volver a integrar en `main` |
| El despliegue falla en `ValidateService` | `docker logs backend` | Revisar la configuración montada en `/config` |
| Datos perdidos tras un despliegue | H2 en memoria (R-03) | Esperado hasta migrar a PostgreSQL |
| ✏️ | | |

## Contactos de escalamiento
✏️
