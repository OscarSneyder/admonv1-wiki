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

## Criterios de aceptación
- **Dado** un residente registrado, **cuando** envía credenciales válidas, **entonces** recibe HTTP 200 y un token.
- ✏️ Agregar criterios para los flujos alternos.

## Evidencias
- Prueba automatizada: `api-tests/test_api.py::test_create_application_end_to_end` (paso 1)
- ✏️ Captura: `![Login](../../assets/imagenes/ui-login.png)`
