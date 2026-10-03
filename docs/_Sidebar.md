**[🏠 Inicio](README.md)**

**00 · Proyecto**
- [Visión del proyecto](00-proyecto/01-vision-del-proyecto.md)
- [Metodología PAREX en ADMON v1](00-proyecto/02-metodologia-parex.md)
- [Equipo y hebras](00-proyecto/03-equipo-y-hebras.md)
- [Glosario](00-proyecto/04-glosario.md)

**01 · Modelo de negocio**
- [Necesidad del negocio (contexto y problema)](01-modelo-de-negocio/01-necesidad-del-negocio.md)
- [Actores y stakeholders](01-modelo-de-negocio/02-actores-y-stakeholders.md)
- [Modelo semántico](01-modelo-de-negocio/03-modelo-semantico.md)
- [Reglas de negocio](01-modelo-de-negocio/04-reglas-de-negocio.md)
- [Servicios de negocio](01-modelo-de-negocio/05-servicios-de-negocio.md)
- [Escenarios de servicio](01-modelo-de-negocio/06-escenarios-de-servicio.md)

**02 · Modelo de capacidades**
- [Mapa de casos de uso (C.U)](02-modelo-de-capacidades/01-mapa-de-casos-de-uso.md)
  - [C.U-01 · Publicar convocatoria](02-modelo-de-capacidades/casos-de-uso/C.U-01-publicar-convocatoria.md)
  - [C.U-02 · Consultar convocatoria](02-modelo-de-capacidades/casos-de-uso/C.U-02-consultar-convocatoria.md)
  - [C.U-03 · Registrar solicitud](02-modelo-de-capacidades/casos-de-uso/C.U-03-registrar-solicitud.md)
  - [C.U-04 · Consultar solicitud](02-modelo-de-capacidades/casos-de-uso/C.U-04-consultar-solicitud.md)
  - [C.U-05 · Ajustar solicitud](02-modelo-de-capacidades/casos-de-uso/C.U-05-ajustar-solicitud.md)
  - [C.U-06 · Cancelar solicitud](02-modelo-de-capacidades/casos-de-uso/C.U-06-cancelar-solicitud.md)
  - [C.U-07 · Aceptar solicitud](02-modelo-de-capacidades/casos-de-uso/C.U-07-aceptar-solicitud.md)
  - [C.U-08 · Ejecutar sorteo de cupos](02-modelo-de-capacidades/casos-de-uso/C.U-08-ejecutar-sorteo-de-cupos.md)
  - [C.U-09 · Configurar zona común](02-modelo-de-capacidades/casos-de-uso/C.U-09-configurar-zona-comun.md)
- [Requisitos funcionales implementados](02-modelo-de-capacidades/02-requisitos-funcionales.md)
- [Atributos de calidad y requerimientos no funcionales (RNF)](02-modelo-de-capacidades/03-atributos-de-calidad-y-rnf.md)

**03 · Modelo de arquitectura**
- [Modelo de arquitectura — Visión general](03-modelo-de-arquitectura/README.md)
- [Drivers arquitectónicos](03-modelo-de-arquitectura/01-drivers-arquitectonicos.md)
  - [Vista de composición](03-modelo-de-arquitectura/02-modelo-de-comunicacion/01-vista-de-composicion.md)
  - [Vista de componentes](03-modelo-de-arquitectura/02-modelo-de-comunicacion/02-vista-de-componentes.md)
  - [Vista de despliegue](03-modelo-de-arquitectura/02-modelo-de-comunicacion/03-vista-de-despliegue.md)
  - [Mecanismos de diseño](03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)
  - [MEC-04 · Despliegue automatizado y verificado](03-modelo-de-arquitectura/03-mecanismos-de-diseno/MEC-04-despliegue-automatizado-verificado.md)
  - [Registro de decisiones arquitectónicas (ADR)](03-modelo-de-arquitectura/04-decisiones-adr/README.md)
  - [ADR-0001 · SPA + API REST monolítica en capas](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0001-spa-y-api-rest-monolitica.md)
  - [ADR-0002 · Frontend con Clean Architecture por features](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0002-frontend-clean-architecture-por-features.md)
  - [ADR-0003 · Autenticación JWT sin estado](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0003-autenticacion-jwt-stateless.md)
  - [ADR-0004 · Carga directa a S3 con URL prefirmada](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0004-carga-directa-a-s3.md)
  - [ADR-0005 · Persistencia con JPA; H2 en desarrollo, PostgreSQL previsto](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0005-persistencia-jpa-h2-postgresql.md)
  - [ADR-0006 · CI/CD con AWS CodePipeline y Docker en EC2](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0006-cicd-aws-docker-ec2.md)
  - [ADR-0007 · Despliegue in-place sobre una única instancia EC2 persistente](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md)
  - [ADR-0008 · Nginx como punto único de entrada HTTPS](03-modelo-de-arquitectura/04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md)
- [Modelo de datos](03-modelo-de-arquitectura/05-modelo-de-datos.md)
- [Contratos de la API REST](03-modelo-de-arquitectura/06-contratos-api.md)
- [Conceptos transversales](03-modelo-de-arquitectura/07-conceptos-transversales.md)
  - [Administración Backend](03-modelo-de-arquitectura/08-diseno-detallado/01-arquitectura-backend.md)
  - [Arquitectura del Frontend](03-modelo-de-arquitectura/08-diseno-detallado/02-arquitectura-frontend.md)

**04 · Modelo de validación**
- [Modelo de validación — Catálogo de EUV](04-modelo-de-validacion/README.md)
- [Estrategia y cobertura de validación](04-modelo-de-validacion/01-estrategia-y-cobertura.md)
  - [EUV-01 · Iniciar sesión](04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md)
  - [EUV-02 · Consultar convocatorias y su detalle](04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md)
  - [EUV-03 · Verificar elegibilidad para presentar una solicitud](04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md)
  - [EUV-04 · Cargar documentos a S3](04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md)
  - [EUV-05 · Registrar solicitud](04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md)
  - [EUV-06 · Consultar mis solicitudes y su detalle](04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md)
  - [EUV-Q-01 · Rendimiento: 300 solicitudes concurrentes](04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md)
  - [EUV-Q-02 · Seguridad: acceso no autenticado a la API](04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md)
  - [EUV-Q-03 · Integridad: solicitud duplicada de la misma unidad privada](04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md)
  - [EUV-Q-04 · Desplegabilidad: despliegue automatizado y verificado](04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md)
  - [EUV-Q-05 · Escalabilidad: carga de archivos sin pasar por la API](04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md)
- [Revisiones técnicas](04-modelo-de-validacion/02-revisiones-tecnicas.md)
- [Pruebas especiales: carga](04-modelo-de-validacion/03-pruebas-especiales.md)
- [Automatización de EUV (pruebas de API)](04-modelo-de-validacion/04-automatizacion-de-euv.md)

**05 · Ejecución y evidencia**
- [Aplicación de validación](05-ejecucion-y-evidencia/01-aplicacion-de-validacion.md)
  - [Evidencias por tajada](05-ejecucion-y-evidencia/evidencias/README.md)
- [Lectura integrada de validación](05-ejecucion-y-evidencia/02-lectura-integrada.md)
- [Aprendizajes](05-ejecucion-y-evidencia/03-aprendizajes.md)

**06 · Dirección y planeación**
- [Estimación del servicio](06-direccion-y-planeacion/01-estimacion-del-servicio.md)
- [Realización Básica del Servicio (RBS)](06-direccion-y-planeacion/02-rbs.md)
- [Backlog de tajadas y cronograma](06-direccion-y-planeacion/03-backlog-de-tajadas.md)
- [Seguimiento a la inversión](06-direccion-y-planeacion/04-seguimiento.md)
- [Riesgos y deuda técnica](06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)
- [Entregables](06-direccion-y-planeacion/06-entregables.md)
  - [Actas de reunión](06-direccion-y-planeacion/actas/README.md)

**07 · Código y despliegue**
- [GitFlow y gestión de cambios](07-codigo-y-despliegue/01-gitflow.md)
- [Pipeline CI/CD](07-codigo-y-despliegue/02-pipeline-ci-cd.md)
- [Configuración y entornos](07-codigo-y-despliegue/03-configuracion-y-entornos.md)
- [Instalación y ejecución local](07-codigo-y-despliegue/04-instalacion-local.md)
- [Runbook de operación](07-codigo-y-despliegue/05-runbook.md)

**08 · Gobierno**
- [Gobierno de la arquitectura](08-gobierno/README.md)
- [Principios de arquitectura](08-gobierno/01-principios.md)
- [Estándares y convenciones](08-gobierno/02-estandares-y-convenciones.md)
- [Matriz de trazabilidad](08-gobierno/03-matriz-de-trazabilidad.md)

**Otros**
- [Plantillas](plantillas/pagina-generica.md)
- [Cómo contribuir](CONTRIBUIR.md)
