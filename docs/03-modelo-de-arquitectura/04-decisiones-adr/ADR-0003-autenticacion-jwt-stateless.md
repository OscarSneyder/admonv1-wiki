# ADR-0003 · Autenticación JWT sin estado

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️

## Contexto
La API debe proteger los datos de los residentes y poder escalar horizontalmente sin compartir sesiones entre instancias.

## Decisión
- Login con `AuthenticationManager` + BCrypt.
- Emisión de **JWT HS256** con `NimbusJwtEncoder` (claims `sub`, `roles`, `iss = adminv1`, `exp = 1 h`).
- Validación con **Spring OAuth2 Resource Server** (`NimbusJwtDecoder` con clave secreta).
- Token guardado en `localStorage` y enviado como `Authorization: Bearer`.

## Alternativas
| Alternativa | Motivo de descarte |
|-------------|-------------------|
| Sesión HTTP con cookie | Estado en el servidor; complica escalar |
| Proveedor externo (Cognito, Keycloak) | Mayor complejidad para el alcance v1 ✏️ |
| RS256 (clave asimétrica) | ✏️ innecesario con un único emisor y verificador |

## Consecuencias
- ➕ Sin estado; cualquier instancia valida el token.
- ➖ No hay revocación antes de la expiración.
- ➖ `localStorage` es vulnerable a XSS (alternativa: cookie `HttpOnly`).
- ⚠️ No se aplica autorización por rol en los endpoints.
