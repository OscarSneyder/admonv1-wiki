# Mecanismos de diseño

> **Propósito:** registrar las soluciones técnicas estándar que responden a los RNF. En PAREX, un mecanismo se formaliza solo si tiene un RNF de origen; un RNF puede originar varios mecanismos y un mecanismo puede atender varios RNF.
> Plantilla: [mecanismo-de-diseno](../../plantillas/mecanismo-de-diseno.md).

## Catálogo
> ⚠️ **Por confirmar:** candidatos identificados a partir de las tácticas ya documentadas en los EUV de calidad y los ADR; formalizar cada uno con la plantilla.

| ID | Mecanismo (candidato) | RNF de origen | ADR relacionado | EUV que lo valida | Estado |
|----|-----------------------|---------------|-----------------|-------------------|--------|
| MEC-01 | Autenticación y autorización con JWT sin estado | RNF-02 | [ADR-0003](../04-decisiones-adr/ADR-0003-autenticacion-jwt-stateless.md) | [EUV-Q-02](../../04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md) | Candidato |
| MEC-02 | Control de unicidad de solicitud (servicio + restricción única en BD) | RNF-03 | [ADR-0005](../04-decisiones-adr/ADR-0005-persistencia-jpa-h2-postgresql.md) | [EUV-Q-03](../../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) | Candidato |
| MEC-03 | Carga directa de documentos a almacenamiento con URL prefirmada | RNF-05 | [ADR-0004](../04-decisiones-adr/ADR-0004-carga-directa-a-s3.md) | [EUV-Q-05](../../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md) | Candidato |
| [MEC-04](MEC-04-despliegue-automatizado-verificado.md) | Despliegue automatizado y verificado (construir una vez, barrera de pruebas, validación de salud) | RNF-04 | [ADR-0006](../04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md), [ADR-0007](../04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md) | [EUV-Q-04](../../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md) | Formalizado |
| MEC-05 | Notificación de eventos de convocatoria (apertura de convocatoria, solicitud nueva, resultado del sorteo) | ✏️ RNF por definir | — | ✏️ | Candidato |
| MEC-06 | Aislamiento de datos de prueba: prefijo `ci-tests/` en S3 y permisos IAM de CI limitados a `ci-tests/*` | RNF-02 | [ADR-0006](../04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md) | ✏️ | Candidato |
| MEC-07 | Terminación TLS en proxy inverso (Nginx :443 → `127.0.0.1:8080`) | RNF-02 | [ADR-0008](../04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md) | ✏️ | Candidato |
| MEC-NN | ✏️ Mecanismo para RNF-01 (rendimiento bajo pico) | RNF-01 | ✏️ | [EUV-Q-01](../../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) | ✏️ |

## Diferencia con otros elementos técnicos
| Elemento | Qué es | Dónde se documenta |
|----------|--------|--------------------|
| Mecanismo | Solución estándar con regla de aplicación, nacida de un RNF | Esta página |
| Decisión (ADR) | Elección justificada entre alternativas | [Decisiones ADR](../04-decisiones-adr/README.md) |
| Componente | Pieza del sistema que implementa responsabilidades | [Vista de componentes](../02-modelo-de-comunicacion/02-vista-de-componentes.md) |
| Estándar | Convención de código o de trabajo | [Estándares](../../08-gobierno/02-estandares-y-convenciones.md) |
