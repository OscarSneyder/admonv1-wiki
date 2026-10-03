# ADR-0002 · Frontend con Clean Architecture por features

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️

## Contexto
El frontend consume varios recursos (auth, convocatorias, solicitudes, documentos). Sin una estructura clara, la lógica de negocio y las llamadas HTTP tienden a mezclarse con los componentes de React.

## Decisión
Organizar `src/features/<feature>/` en cuatro capas:
- `domain/` — entidades y **interfaces** de repositorio.
- `application/use-cases/` — casos de uso que dependen solo del dominio.
- `infrastructure/` — clientes HTTP (`*.api.js`), implementaciones `Http*Repository` y almacenamiento.
- `presentation/` — páginas, componentes y hooks.

La composición se hace en `<feature>.dependencies.js` (inyección de dependencias manual). El código HTTP transversal vive en `shared/infrastructure/http/authenticatedFetch.js`.

## Consecuencias
- ➕ Casos de uso probables de forma aislada (se puede sustituir el repositorio por un doble de prueba).
- ➕ Cambiar el backend o el transporte solo afecta a `infrastructure/`.
- ➖ Más archivos y *boilerplate* para features simples.
- ➕ El incumplimiento detectado en `ConvocationDetailPage` (importaba infraestructura de otra feature) fue corregido. Ver deuda D-05.
