# Instalación y ejecución local

## Prerrequisitos
| Herramienta | Versión |
|-------------|---------|
| JDK | 21 |
| Maven | incluido (`mvnw`) |
| Node.js | ✏️ (compatible con Vite 8) |
| Git | — |
| Credenciales AWS | Perfil con acceso al bucket (solo para la carga de documentos) |

## Backend
```bash
git clone https://github.com/ByteCscript/admonv1-backend.git
cd admonv1-backend
# Definir jwt.secret (Base64) en un application.properties local no versionado
./mvnw spring-boot:run
```
- API: http://localhost:8080/api/health
- Swagger: http://localhost:8080/swagger-ui.html
- H2: http://localhost:8080/h2-console (JDBC `jdbc:h2:mem:parexdb`, usuario `sa`)

## Frontend
```bash
git clone https://github.com/ByteCscript/admonv1-frontend.git
cd admonv1-frontend
npm install
npm run dev
```
- App: http://localhost:5173 (el proxy de Vite redirige `/api` a `:8080`)

## Usuario de prueba
`residente@test.com` · contraseña: ✏️ (consultar al equipo)

## Problemas frecuentes
| Síntoma | Causa probable | Solución |
|---------|---------------|----------|
| Error al arrancar por `jwt.secret` | Placeholder `{KEY_BASE64}` no es Base64 válido | Definir una clave Base64 de ≥ 256 bits |
| 401 en todas las llamadas | Token ausente o expirado | Volver a iniciar sesión |
| Error CORS | `app.frontend.url` distinto del origen | Ajustar la propiedad |
| Falla al subir a S3 | Credenciales o CORS del bucket | ✏️ |
