# Recursos (assets)

| Carpeta | Contenido | Formatos |
|---------|-----------|----------|
| [diagramas/](diagramas) | Archivos **fuente** editables de diagramas | `.drawio`, `.puml`, `.mmd`, `.dsl` (Structurizr) |
| [imagenes/](imagenes) | Capturas de pantalla y exportaciones de diagramas | `.png`, `.svg`, `.jpg` |
| [documentos/](documentos) | Presentaciones, hojas de cálculo, OpenAPI exportado, PDF | `.pdf`, `.xlsx`, `.pptx`, `.json` |

## Imágenes sugeridas para completar la entrega
| Archivo sugerido | Dónde se usa |
|------------------|--------------|
| `ui-login.png` | EUV-01 |
| `ui-convocatorias.png`, `ui-detalle-convocatoria.png` | EUV-02, EUV-03 |
| `ui-carga-documentos.png` | EUV-04 |
| `ui-postulacion-resumen.png` | EUV-05 |
| `ui-mis-postulaciones.png` | EUV-06 |
| `pruebas-k6-300vu.png` | EUV-Q-01 |
| `pruebas-api-codebuild.png` | Pruebas API |
| `aws-codepipeline.png` | CI/CD |
| `swagger-ui.png` | Contratos API |

## Diagramas incorporados
| Archivo | Diagrama | Página |
|---------|----------|--------|
| `parex-modelo-semantico-v3.jpg` | Modelo semántico v3 | [03-modelo-semantico](../01-modelo-de-negocio/03-modelo-semantico.md) |
| `parex-servicios-de-negocio-v2.jpg` | Servicios de negocio v2 | [05-servicios-de-negocio](../01-modelo-de-negocio/05-servicios-de-negocio.md) |
| `parex-escenarios-de-servicio-v2.jpg` | Escenarios de servicio v2 | [06-escenarios-de-servicio](../01-modelo-de-negocio/06-escenarios-de-servicio.md) |
| `parex-casos-de-uso-v2.jpg` | Casos de uso v2 | [01-mapa-de-casos-de-uso](../02-modelo-de-capacidades/01-mapa-de-casos-de-uso.md) |
| `rnf-02-autenticacion-jwt.png` | Secuencia RNF-02 (fuente `.puml` en diagramas/) | [EUV-Q-02](../04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md) |
| `rnf-03-unicidad-solicitud.png` | Secuencia RNF-03 (fuente `.puml` en diagramas/) | [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) |
| `rnf-04-despliegue-in-place.png` | Secuencia RNF-04 (fuente `.puml` en diagramas/) | [EUV-Q-04](../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md) |
| `rnf-05-carga-directa-s3.png` | Secuencia RNF-05 (fuente `.puml` en diagramas/) | [EUV-Q-05](../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md) |
| `rnf-07-ayuda-contextual.png` | Secuencia RNF-07 (fuente `.puml` en diagramas/) | [Atributos de calidad y RNF](../02-modelo-de-capacidades/03-atributos-de-calidad-y-rnf.md) |
| `flujos-solicitud-cu-03.jpg` | Flujos de Postulación (básico y excepciones) | [C.U-03](../02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md) |
| `cicd-pipeline-backend.png` | Pipeline CI/CD del backend | [02-pipeline-ci-cd](../07-codigo-y-despliegue/02-pipeline-ci-cd.md) |


## Diagramas por agregar
Cada página tiene un marcador `✏️ Diagrama por agregar` con la ruta esperada. Al guardar la imagen con este nombre en `imagenes/`, reemplazar el marcador por la línea de imagen que contiene.

| Archivo | Diagrama | Página |
|---------|----------|--------|
| `negocio-proceso-to-be.png` | Proceso de negocio TO-BE | [01-necesidad-del-negocio](../01-modelo-de-negocio/01-necesidad-del-negocio.md) |
| `negocio-estados-postulacion.png` | Estados de la postulación | [04-reglas-de-negocio](../01-modelo-de-negocio/04-reglas-de-negocio.md) |
| `negocio-estados-documento.png` | Estados del documento | [04-reglas-de-negocio](../01-modelo-de-negocio/04-reglas-de-negocio.md) |
| `esc-q-01-concurrencia-postulaciones.png` | Escenario EUV-Q-01 | [EUV-Q-01-concurrencia-postulaciones](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) |
| `esc-f-01-iniciar-sesion.png` | Flujo de inicio de sesión | [EUV-01-iniciar-sesion](../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md) |
| `esc-f-02-consultar-convocatorias.png` | Flujo de consulta de convocatorias | [EUV-02-consultar-convocatorias](../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) |
| `esc-f-03-verificar-elegibilidad.png` | Flujo de verificación de elegibilidad | [EUV-03-verificar-elegibilidad](../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) |
| `esc-f-04-cargar-documentos.png` | Flujo de carga de documentos | [EUV-04-cargar-documentos](../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) |
| `esc-f-05-registrar-postulacion.png` | Flujo de registro de postulación | [EUV-05-registrar-postulacion](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) |
| `esc-f-06-consultar-postulaciones.png` | Flujo de consulta de postulaciones | [EUV-06-consultar-postulaciones](../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) |
| `escenarios-flujo-residente.png` | Flujo completo del residente | [README](../04-modelo-de-validacion/README.md) |
| `frontend-capas-feature.png` | Capas de una feature | [02-vista-de-componentes](../03-modelo-de-arquitectura/02-modelo-de-comunicacion/02-vista-de-componentes.md) |
| `frontend-componentes-por-feature.png` | Componentes por feature | [02-vista-de-componentes](../03-modelo-de-arquitectura/02-modelo-de-comunicacion/02-vista-de-componentes.md) |
| `modelo-entidad-relacion.png` | Diagrama entidad-relación | [05-modelo-de-datos](../03-modelo-de-arquitectura/05-modelo-de-datos.md) |
| `despliegue-produccion.png` | Despliegue en producción | [03-vista-de-despliegue](../03-modelo-de-arquitectura/02-modelo-de-comunicacion/03-vista-de-despliegue.md) |
| `despliegue-local.png` | Despliegue local | [03-vista-de-despliegue](../03-modelo-de-arquitectura/02-modelo-de-comunicacion/03-vista-de-despliegue.md) |
| `git-flujo-de-ramas.png` | Flujo de ramas | [02-estandares-y-convenciones](../08-gobierno/02-estandares-y-convenciones.md) |
| `riesgos-matriz.png` | Matriz de riesgos | [05-riesgos-y-deuda-tecnica](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md) |
| `gobierno-modelo.png` | Modelo de gobierno | [README](../08-gobierno/README.md) |
| `pruebas-piramide.png` | Pirámide de pruebas | [01-estrategia-y-cobertura](../04-modelo-de-validacion/01-estrategia-y-cobertura.md) |
| `cronograma-gantt.png` | Cronograma del proyecto | [03-backlog-de-tajadas](../06-direccion-y-planeacion/03-backlog-de-tajadas.md) |
