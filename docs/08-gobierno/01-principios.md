# Principios de arquitectura

| ID | Principio | Justificación | Implicaciones |
|----|-----------|---------------|---------------|
| P-01 | **Separación por capas y responsabilidades** | Mantenibilidad y testabilidad | Back: Controller → Service → Repository. Front: presentation → application → domain ← infrastructure |
| P-02 | **La API es la única puerta a los datos** | Integridad y seguridad | El frontend nunca accede directamente a la BD; las reglas de negocio viven en el backend |
| P-03 | **Seguro por defecto** | Proteger datos de residentes | Todo endpoint es privado salvo excepción explícita en `SecurityConfig` |
| P-04 | **Sin estado en el servidor** | Escalabilidad horizontal | JWT en lugar de sesiones |
| P-05 | **Los archivos no pasan por la API** | Rendimiento y costo | URL prefirmadas a S3 |
| P-06 | **Configuración externa, no en el código** | Seguridad, portabilidad entre entornos | Secretos y URLs por properties/variables de entorno |
| P-07 | **Todo cambio desplegable es automatizado y verificado** | Desplegabilidad | Pipeline con pruebas y health check |
| P-08 | **Respuestas homogéneas** | Consistencia para el cliente | `ApiResponse<T>` + códigos de error en MAYÚSCULAS |
| P-09 | **Decisiones documentadas** | Gobierno y trazabilidad | Toda decisión significativa tiene un ADR |
| P-10 | **Construir una vez** | Lo que se prueba es lo que se despliega | Build produce el BuildArtifact; Test y Deploy consumen esa misma versión ([MEC-04](../03-modelo-de-arquitectura/03-mecanismos-de-diseno/MEC-04-despliegue-automatizado-verificado.md)) |
| P-11 | **Fallar rápido antes de producción** | Calidad antes del despliegue | Una prueba fallida en Test impide llegar a Deploy |
| P-12 | **Mínimo privilegio** | Seguridad | El rol de CI queda limitado a `ci-tests/*` en S3 ([MEC-06](../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)) |
| P-13 | **Aislamiento del runtime** | Seguridad y reproducibilidad | La aplicación corre en Docker y el backend no se publica directamente ([ADR-0008](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md)) |
| P-14 | **Observabilidad** | Diagnóstico y evidencia | CloudWatch Logs registra las ejecuciones de CodeBuild; `ValidateService` comprueba el estado funcional |
| P-15 | **Separación de responsabilidades en el pipeline** | Localizar fallas | GitHub, CodePipeline, CodeBuild, CodeDeploy y EC2 tienen responsabilidades distintas |
