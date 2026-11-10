# EUV-01 · Iniciar sesión

| Campo | Valor |
|-------|-------|
| Requisito | RF-01 |
| Actor | Residente |
| Precondición | El usuario existe y está habilitado (`enabled = true`) |
| Postcondición | El token JWT queda guardado en `localStorage` (`auth_token`) |
| Pantalla | `/login` → `LoginPage` |
| Endpoint | `POST /api/auth/login` (público) |
| Componentes | Front: `LoginPage`, `useLogin`, `Login` (caso de uso), `HttpAuthRepository`, `TokenStorage` · Back: `AuthController`, `AuthService`, `JwtService`, `CustomUserDetailsService` |

## Flujo principal
> ✏️ **Diagrama por agregar:** `![Flujo de inicio de sesión](../../assets/imagenes/esc-f-01-iniciar-sesion.png)`

## Flujos alternos
| # | Condición | Respuesta |
|---|-----------|-----------|
| A1 | Credenciales inválidas | ⚠️ confirmar código HTTP y mensaje |
| A2 | Usuario deshabilitado | ⚠️ confirmar |
| A3 | Ruta protegida sin token | `ProtectedRoute` redirige a `/login` |
| A4 | Usuario sin el rol que exige la ruta (p. ej. el administrador en `/convocations` o `/mis-postulaciones`, o un residente en `/admin/...`) | `ProtectedRoute` redirige a `/`; el menú solo muestra las opciones del rol ([RN-40](../../01-modelo-de-negocio/04-reglas-de-negocio.md)) |

## Criterios de aceptación
- **Dado** un residente registrado, **cuando** envía credenciales válidas, **entonces** recibe HTTP 200 y un token.
- ✏️ Agregar criterios para los flujos alternos.

## Evidencias
- Prueba automatizada: `api-tests/test_api.py::test_create_application_end_to_end` (paso 1)
- ✏️ Captura: `![Login](../../assets/imagenes/ui-login.png)`
