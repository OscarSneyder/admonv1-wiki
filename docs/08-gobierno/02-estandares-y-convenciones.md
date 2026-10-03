# Estándares y convenciones

## Gestión de código
Repositorios, flujo de ramas y mensajes de commit: ver [GitFlow](../07-codigo-y-despliegue/01-gitflow.md).

## Código backend
- Paquetes por capa: `controller`, `service` (interfaz `I*Service` + `*ServiceImpl`), `repository`, `model`, `dto`, `config`, `auth`.
- DTO como `record` de Java.
- Respuestas envueltas en `ApiResponse<T>`; códigos de error en `MAYUSCULAS_CON_GUION_BAJO`.
- Mensajes para el usuario en español.

## Código frontend
- Estructura por feature y capas (ver [ADR-0002](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0002-frontend-clean-architecture-por-features.md)).
- Hooks `useX`, casos de uso en PascalCase (`GetApplications`), repositorios `HttpXRepository`.
- ESLint (`npm run lint`) obligatorio antes del PR.
- `data-testid` para elementos que se prueban.

## API
- Recursos en plural e inglés: `/api/calls`, `/api/applications`, `/api/documents`.
- Acciones no CRUD como subrecurso verbal: `/documents/{id}/complete`, `/applications/check`.

## Pull Requests
- Mínimo 1 aprobación. ✏️ Plantilla de PR.
- Checklist de conformidad: ver [Gobierno](README.md#checklist-de-conformidad-en-cada-pr-con-impacto-arquitectónico).
