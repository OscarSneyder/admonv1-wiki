# Backlog de tajadas y cronograma

## Backlog único de tajadas
| Tajada | Objetivo | C.U / EUV incluidos | Riesgo que reduce | Prioridad | Estado |
|--------|----------|---------------------|-------------------|-----------|--------|
| TJ-01 | Asegurar y evidenciar lo implementado | C.U-04 (R-01: listado y detalle restringidos al residente del JWT); evidencia de `test_create_application_end_to_end` (EUV-01 a EUV-06); EUV-Q-01 con 300 residentes semilla | Técnica: seguridad y rendimiento | 1 | 🔴 Pendiente |
| TJ-02 | Publicar convocatorias y validar solicitudes | C.U-01 (RN-14, RN-15), C.U-07; EUV por crear | Negocio: validación por el administrador y estados | 2 | 🔴 Pendiente |
| TJ-03 | Asignar cupos por sorteo y notificar | C.U-08, MEC-05; EUV por crear | Negocio: reglas y aceptación del sorteo; completa la RBS | 3 | 🔴 Pendiente |

## Evolución diferida del pipeline
Prevista en [ADR-0007](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md) si aparecen requisitos más fuertes de disponibilidad, escalabilidad o despliegue sin interrupción:

`Staging EC2 → Selenium E2E → Load Balancer → Auto Scaling → Blue/Green`

## Cronograma

> ✏️ **Diagrama por agregar:** `![Cronograma del proyecto](../assets/imagenes/cronograma-gantt.png)`

## Resumen por sprint
| Sprint | Fechas | Objetivo | Historias | Resultado | Retrospectiva |
|--------|--------|----------|-----------|-----------|---------------|
| 1 | ✏️ | ✏️ | ✏️ | ✏️ | ✏️ |
| 2 | ✏️ | ✏️ | ✏️ | ✏️ | ✏️ |
| 3 | ✏️ | ✏️ | ✏️ | ✏️ | ✏️ |

## Métricas
| Sprint | Puntos comprometidos | Puntos completados | Velocidad |
|--------|---------------------|--------------------|-----------|
| ✏️ | | | |
