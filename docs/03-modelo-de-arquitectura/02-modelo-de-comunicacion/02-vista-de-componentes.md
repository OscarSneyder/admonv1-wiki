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
| Ruta | Página | Protegida |
|------|--------|:---------:|
| `/login` | `LoginPage` | ❌ |
| `/` | `HomePage` | ✅ |
| `/convocations` | `ConvocationsPage` | ✅ |
| `/convocations/:id` | `ConvocationDetailPage` | ✅ |
| `/convocations/:id/apply` | `CreateApplicationPage` | ✅ |
| `/mis-postulaciones` | `ApplicationsPage` | ✅ |
| `/applications/:id` | `ApplicationDetailPage` | ✅ |

### Componentes compartidos
| Componente | Ubicación | Uso |
|-----------|-----------|-----|
| `Navbar` | `src/components/` | Navegación (oculta en `/login`) |
| `Stepper` | `src/components/` | Pasos de la solicitud |
| `Tooltip` | `src/components/` | Ayudas contextuales |
| `UI` (`Loading`, `ErrorMessage`, …) | `src/components/` | Estados de carga y error |
| `authenticatedFetch` | `src/shared/infrastructure/http/` | Añade `Authorization: Bearer` a todas las llamadas |

### Hallazgos (ver [Riesgos y deuda](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md))
- ✅ Resuelto: `ConvocationDetailPage` ya no importa infraestructura de la feature `applications` (D-05).
- `src/constants.js` contiene un `RESIDENT` fijo (`id: 1`, nombre, unidad y estado "Paz y Salvo") que se muestra en la interfaz y se envía como `residentId`; el backend ignora ese valor al registrar (R-01).
- Mapa de dependencias generado: `admonv1-frontend/graphify-out/graph.html`.
