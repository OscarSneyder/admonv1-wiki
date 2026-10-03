# Vista de composición

## Elementos del sistema y su entorno
| Elemento | Responsabilidad | Interfaz |
|----------|-----------------|----------|
| Residente | Usuario final | Navegador web |
| Administrador | Gestiona convocatorias, solicitudes y sorteo (C.U-01, C.U-07, C.U-08, C.U-09) | Navegador web (por implementar) |
| ADMON v1 | Sistema bajo diseño | HTTPS / REST |
| AWS S3 | Almacenamiento de objetos | API S3 |
| AWS CodePipeline | Integración y despliegue continuos | Fuente GitHub `ByteCscript/admonv1-backend`, rama `main` ([Pipeline CI/CD](../../07-codigo-y-despliegue/02-pipeline-ci-cd.md)) |


## Unidades desplegables
| Unidad | Tecnología | Responsabilidades | Puerto | Repositorio |
|-----------|-----------|-------------------|--------|-------------|
| Frontend SPA | React 19, Vite 8 | UI, enrutamiento, guarda del token, orquestación de la carga a S3 | 5173 (dev) | admonv1-frontend |
| Backend API | Spring Boot 4.1, Java 21 | Autenticación, reglas de negocio, persistencia, URL prefirmadas, OpenAPI | 8080 | admonv1-backend |
| Base de datos | H2 / PostgreSQL | Persistencia relacional | embebida | — |
| Nginx | Proxy inverso en EC2 | Terminación TLS y redirección al backend ([ADR-0008](../04-decisiones-adr/ADR-0008-nginx-punto-unico-de-entrada-https.md)) | 443 | — |
| AWS S3 | Servicio gestionado | Almacenamiento de PDF | 443 | — |

## Comunicación
| Origen → destino | Protocolo | Formato | Autenticación |
|------------------|-----------|---------|---------------|
| Internet → Nginx → API | HTTPS :443 → HTTP `127.0.0.1:8080` | — | — |
| SPA → API | HTTPS/REST | JSON `ApiResponse<T>` | JWT Bearer |
| SPA → S3 | HTTPS PUT | binario PDF | Firma en la URL prefirmada (5 min) |
| API → S3 | AWS SDK | — | Credenciales IAM (⚠️ confirmar: rol de instancia EC2) |
| API → BD | JDBC | SQL | usuario/contraseña |
