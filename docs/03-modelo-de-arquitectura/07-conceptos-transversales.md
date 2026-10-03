# Conceptos transversales

## 1. Seguridad
| Aspecto | Implementación |
|---------|----------------|
| Autenticación | `POST /api/auth/login` → `AuthenticationManager` + `CustomUserDetailsService` + contraseñas BCrypt |
| Token | JWT HS256 (Nimbus); claims: `sub` = email, `roles`, `iss` = `adminv1`; expira en 3 600 000 ms (1 h) |
| Validación | Spring OAuth2 Resource Server (`oauth2ResourceServer().jwt()`) |
| Almacenamiento en el cliente | `localStorage` clave `auth_token` (`TokenStorage`) |
| Envío | `authenticatedFetch` añade `Authorization: Bearer <token>` |
| Autorización | Solo autenticado / no autenticado; ⚠️ no hay reglas por rol |
| CSRF | Deshabilitado (API sin estado) |
| Secretos | `jwt.secret` en Base64 inyectado por configuración externa (`/config/application.properties` en EC2, `JWT_SECRET` en CodeBuild) |

## 2. Formato de respuesta y manejo de errores
Todas las respuestas usan el *record* `ApiResponse<T>`:

```json
{
  "message": "Solicitud registrada correctamente",
  "data": { },
  "error": null
}
```
```json
{
  "message": "La unidad privada ya tiene una solicitud para esta convocatoria",
  "data": null,
  "error": "APPLICATION_ALREADY_EXISTS"
}
```

| HTTP | Uso |
|------|-----|
| 200 | Éxito |
| 400 | Validación (`INVALID_REQUEST`, `INVALID_CONTENT_TYPE`, `FILE_TOO_LARGE`, `REQUIRED_DOCUMENTS_MISSING`, …) |
| 401 | Sin token o token inválido |
| 404 | Recurso inexistente (`CALL_NOT_FOUND`, `DOCUMENT_NOT_FOUND`, …) |
| 409 | Conflicto (`APPLICATION_ALREADY_EXISTS`) |
| 500 | Error no controlado; hoy ocurre ante una violación de la restricción única por solicitudes simultáneas ([R-06](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)) |

Catálogo completo de códigos: [Reglas de negocio](../01-modelo-de-negocio/04-reglas-de-negocio.md).

## 3. CORS
`CorsConfig` permite `/api/**` desde `app.frontend.url` (dev: `http://localhost:5173`) con los métodos GET, POST, PUT, DELETE, PATCH y OPTIONS.

## 4. Configuración externa
Ver [Configuración y entornos](../07-codigo-y-despliegue/03-configuracion-y-entornos.md).

## 5. Paginación
`PageResponseDTO<T>` con los parámetros `page`, `size`, `sortBy` y `direction`; filtros dinámicos con JPA `Specification`.

## 6. Documentación de la API
springdoc OpenAPI: `/swagger-ui.html` y `/v3/api-docs` (públicos). `OpenApiConfig` define el esquema de seguridad *bearer*.

## 7. Observabilidad
| Aspecto | Estado |
|---------|--------|
| Health check | `GET /api/health` → `UP` · `GET /api/db` (prueba de conexión) |
| Logs | stdout del contenedor (`docker logs backend`) |
| Métricas / trazas | ✏️ No implementadas |
