# Vista de componentes

## Componentes del backend

Paquete raíz: `com.administracionback.admonv1` · Estilo: **arquitectura en capas**.

### Catálogo de componentes
| Paquete | Componente | Responsabilidad |
|---------|-----------|-----------------|
| `auth` | `AuthController`, `AuthService`, `JwtService`, `JwtConfig`, `LoginRequestDTO`, `LoginResponseDTO` | Login y emisión/validación de JWT |
| `config` | `SecurityConfig`, `CorsConfig`, `CustomUserDetailsService`, `OpenApiConfig`, `S3Config`, `ApplicationConfg` | Configuración transversal |
| `controller` | `CallController`, `ApplicationController`, `DocumentController`, `HealthController` | Adaptadores REST |
| `service` | `I*Service` + `*ServiceImpl` | Reglas de negocio (RN-01…RN-13) |
| `repository` | Repositorios Spring Data JPA + `ApplicationSpecification` | Persistencia y filtros dinámicos |
| `model` | Entidades JPA y enums | Modelo de dominio |
| `dto` | Records de entrada/salida, `ApiResponse<T>`, `PageResponseDTO<T>` | Contratos de la API |
| `loadtest` | Script k6 y resultados | ⚠️ Ubicado en `src/main/java` (ver deuda D-03) |

### Reglas de dependencia
- `controller` → `service` (vía interfaz `I*Service`) → `repository` → `model`.
- Los controladores no acceden a repositorios.
- ⚠️ Los servicios devuelven `ResponseEntity` (acoplamiento con HTTP); ver deuda D-04.


## Componentes del frontend

Estilo: **Clean Architecture por *features*** ([ADR-0002](../04-decisiones-adr/ADR-0002-frontend-clean-architecture-por-features.md)). Cada feature (`auth`, `convocations`, `applications`) tiene cuatro capas y un archivo `*.dependencies.js` que actúa como raíz de composición (inyección de dependencias manual).

### Capas de una feature
> ✏️ **Diagrama por agregar:** `![Capas de una feature](../../assets/imagenes/frontend-capas-feature.png)`

### Vista de componentes por feature
> ✏️ **Diagrama por agregar:** `![Componentes por feature](../../assets/imagenes/frontend-componentes-por-feature.png)`

### Rutas
`ProtectedRoute` redirige a `/login` sin token y a `/` si el usuario no tiene el rol que exige la ruta. El `Navbar` muestra solo las opciones del rol de la sesión ([RN-40](../../01-modelo-de-negocio/04-reglas-de-negocio.md)).

| Ruta | Página | Acceso |
|------|--------|--------|
| `/login` | `LoginPage` | Público |
| `/` | `HomePage` | Autenticado |
| `/convocations` | `ConvocationsPage` | `ROLE_RESIDENT` |
| `/convocations/:id` | `ConvocationDetailPage` | `ROLE_RESIDENT` |
| `/convocations/:id/apply` | `CreateApplicationPage` | `ROLE_RESIDENT` |
| `/mis-postulaciones` | `ApplicationsPage` | `ROLE_RESIDENT` |
| `/applications/:id` | `ApplicationDetailPage` | `ROLE_RESIDENT` |
| `/calls/:id/draw/results` | `ResidentDrawResultsPage` | `ROLE_RESIDENT` |
| `/admin/convocations` · `/new` · `/:id/edit` | `AdminCallsPage`, `CreateCallPage`, `EditCallPage` | `ROLE_ADMIN` |
| `/admin/convocations/:id/draw` | `CallDrawPage` | `ROLE_ADMIN` |
| `/admin/calls/:id/draw/results` | `CallDrawResultsPage` | `ROLE_ADMIN` |
| `/admin/applications` · `/:id` | `AdminApplicationsPage`, `AdminApplicationDetailPage` | `ROLE_ADMIN` |

### Componentes compartidos
| Componente | Ubicación | Uso |
|-----------|-----------|-----|
| `Navbar` | `src/components/` | Navegación (oculta en `/login`). Residente: *Inicio*, *Convocatorias*, *Mis Postulaciones*. Administrador: *Inicio*, *Administrar Convocatorias*, *Gestionar Postulaciones* |
| `Stepper` | `src/components/` | Pasos de la solicitud |
| `Tooltip` | `src/components/` | Ayudas contextuales |
| `UI` (`Loading`, `ErrorMessage`, …) | `src/components/` | Estados de carga y error |
| `authenticatedFetch` | `src/shared/infrastructure/http/` | Añade `Authorization: Bearer` a todas las llamadas |

### Hallazgos (ver [Riesgos y deuda](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md))
- ✅ Resuelto: `ConvocationDetailPage` ya no importa infraestructura de la feature `applications` (D-05).
- `src/constants.js` expone `getCurrentResident()`, que arma los datos del residente (nombre, unidad privada y torre) a partir de la sesión guardada por el login (`AuthSession`). Solo el estado de cuenta "Paz y Salvo" sigue fijo, porque el backend no lo expone (RN-16). El frontend ya no envía `residentId`: el backend toma el residente del JWT.
- Mapa de dependencias generado: `admonv1-frontend/graphify-out/graph.html`.
