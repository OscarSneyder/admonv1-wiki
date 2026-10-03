# Equipo y hebras

| Integrante | Rol en el proyecto | Responsabilidades | Contacto / GitHub |
|-----------|--------------------|-------------------|-------------------|
| Cristian Chayanne Gómez | Arquitecto de Software & Desarrollador BackEnd | Contratos API, componentes del backend (Spring Boot), gestión de código y despliegue | [crisAngel23](https://github.com/crisAngel23) |
| Johan Steven Gil | Arquitecto de Software & Desarrollador FrontEnd | Componentes del frontend (React), experiencia de usuario, gestión de código y despliegue | [ByteCscript](https://github.com/ByteCscript) |
| Erick Parrado | Arquitecto de Software & Desarrollador FullStack | Pruebas y validación (EUV, evidencias), integración front–back | [ErickParrado](https://github.com/ErickParrado) |
| Oscar Sneyder Chaparro | Arquitecto de Software & Desarrollador BackEnd | Negocio (servicios, casos de uso y reglas), ADRs, gobierno de la arquitectura y wiki | [OscarSneyder](https://github.com/OscarSneyder) |

Los cuatro integrantes comparten el rol de **Arquitecto de Software**: las decisiones arquitectónicas se proponen como ADR y se revisan en equipo (ver [Gobierno](../08-gobierno/README.md)). Las responsabilidades de producto, pruebas y operación se reparten entre ellos según la matriz RACI.

## Matriz RACI de artefactos
R = Responsable · A = Aprueba · C = Consultado · I = Informado

Matriz alineada con la asignación de [roles del curso y hebras PAREX](#roles-del-curso-y-hebras-parex).

| Artefacto | Cristian Gómez<br/>(BackEnd) | Johan Gil<br/>(FrontEnd) | Erick Parrado<br/>(FullStack) | Oscar Chaparro<br/>(BackEnd) |
|-----------|:---:|:---:|:---:|:---:|
| Requisitos, casos de uso y reglas de negocio | C | C | C | A/R |
| EUV y escenarios de calidad | R | R | A/R | R |
| ADRs, mecanismos y vistas de arquitectura | R | R | R | A/R |
| Contratos API | A/R | C | R | R |
| Componentes backend | A/R | I | R | R |
| Componentes frontend | I | A/R | R | C |
| Gestión de código, pipeline CI/CD y despliegue | A/R | A/R | C | C |
| Plan de pruebas y evidencias (API y carga) | R | R | A/R | R |
| Wiki y gobierno | R | R | R | A/R |

## Roles del curso y hebras PAREX

| Rol del proyecto (curso) | Hebra PAREX | Secciones de la wiki | Integrante responsable |
|--------------------------|-------------|----------------------|------------------------|
| Negocio | Producto | [01 · Modelo de negocio](../01-modelo-de-negocio/05-servicios-de-negocio.md), [02 · Modelo de capacidades](../02-modelo-de-capacidades/01-mapa-de-casos-de-uso.md) | Oscar Chaparro |
| Arquitectura | Arquitectura y desarrollo | [02 · RNF](../02-modelo-de-capacidades/03-atributos-de-calidad-y-rnf.md), [03 · Modelo de arquitectura](../03-modelo-de-arquitectura/README.md) | Todos |
| Desarrollo | Arquitectura y desarrollo | [03 · Diseño detallado](../03-modelo-de-arquitectura/08-diseno-detallado/01-arquitectura-backend.md) | Todos |
| Pruebas y validación | Demostración, evidencia y riesgos | [04 · Modelo de validación](../04-modelo-de-validacion/README.md), [05 · Ejecución y evidencia](../05-ejecucion-y-evidencia/02-lectura-integrada.md) | Erick Parrado |
| Gestión de código y despliegue | Arquitectura y desarrollo | [07 · Código y despliegue](../07-codigo-y-despliegue/01-gitflow.md) | Johan Gil & Cristian Gómez |
| Documentación | Conducción | [06 · Dirección y planeación](../06-direccion-y-planeacion/02-rbs.md), [08 · Gobierno](../08-gobierno/README.md) | Todos |
