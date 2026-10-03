# GitFlow y gestión de cambios

> **Propósito:** definir cómo se gobiernan los cambios en los repositorios del proyecto.

## Modelo de ramas GitFlow
> **Backend:** el desarrollo ocurre en `develop` y el merge hacia `main` marca la versión candidata a producción y dispara el [pipeline](02-pipeline-ci-cd.md). ✏️ Protección de ramas por definir.

| Rama | Propósito | Se crea desde | Se integra en |
|------|-----------|---------------|---------------|
| `main` | Versión desplegada | — | — |
| `develop` | Integración | `main` | `release/*` |
| `feature/<tema>` | Nueva funcionalidad | `develop` | `develop` |
| `release/<versión>` | Preparación de una versión | `develop` | `main` y `develop` |
| `hotfix/<tema>` | Corrección urgente | `main` | `main` y `develop` |

## Repositorios
| Repositorio | Contenido | Rama principal |
|-------------|-----------|----------------|
| `ByteCscript/admonv1-backend` | API Spring Boot, pipeline AWS | `main` (liberación) · `develop` (integración) |
| `ByteCscript/admonv1-frontend` | SPA React | `main` (liberación) · `develop` (integración, vía Pull Request) |
| Wiki | Esta documentación | ✏️ |

## Flujo de ramas (observado en el historial)
> ✏️ **Diagrama por agregar:** `![Flujo de ramas](../assets/imagenes/git-flujo-de-ramas.png)`

| Tipo de rama | Patrón | Ejemplo |
|--------------|--------|---------|
| Funcionalidad | `feature/<tema>` | `feature/auth-login`, `feature/documentos` |
| Corrección | `fix/<tema>` | ✏️ |
| Documentación | `docs/<tema>` | `docs/wiki-arquitectura` |

## Mensajes de commit
Se recomienda **Conventional Commits** (ya usado en el backend):

| Prefijo | Uso | Ejemplo real |
|---------|-----|-------------|
| `feat:` | Nueva funcionalidad | `feat: jwt implementation` |
| `fix:` | Corrección | `fix: CORS implementation` |
| `ci:` | Pipeline | `ci: add AWS CodePipeline deployment` |
| `test:` | Pruebas | `test: add API tests for deployment artifact` |
| `docs:` | Documentación | — |
| `refactor:` | Refactorización | — |

