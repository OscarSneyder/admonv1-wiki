# Estrategia y cobertura de validación

## Pirámide de pruebas
> ✏️ **Diagrama por agregar:** `![Pirámide de pruebas](../assets/imagenes/pruebas-piramide.png)`

## Tipos de prueba
| Tipo | Herramienta | Alcance | Dónde corre | Estado |
|------|-------------|---------|-------------|--------|
| Unitarias backend | JUnit 5 / Spring Boot Test | Servicios, reglas RN-* | Local, ✏️ CI | 🔴 Solo `contextLoads` |
| Unitarias frontend | ✏️ Vitest / Testing Library | Casos de uso y hooks | ✏️ | 🔴 |
| API | pytest + requests | [Flujo de solicitud de extremo a extremo (8 pasos)](04-automatizacion-de-euv.md#flujo-de-integración-del-pipeline) | CodeBuild (`buildspec-api-test.yml`) | 🟡 1 prueba de extremo a extremo; sin casos negativos |
| Carga | k6 | `POST /api/applications` | Manual contra EC2 | 🟡 |
| E2E | Selenium (evolución futura) | Flujo del residente | Staging EC2 (futuro) | 🔴 No incluido; separado de las pruebas API ([ADR-0007](../03-modelo-de-arquitectura/04-decisiones-adr/ADR-0007-despliegue-in-place-ec2-persistente.md)) |
| Seguridad | ✏️ OWASP ZAP | API | ✏️ | 🔴 |

## Criterios de entrada y salida
| Criterio | Definición |
|----------|------------|
| Entrada | Build exitoso; datos semilla cargados |
| Salida | 100 % de pruebas API aprobadas; escenarios de calidad dentro de su medida |

## Casos de prueba sugeridos (backlog de QA)
| ID | Caso | Escenario | Tipo |
|----|------|-----------|------|
| CP-01 | Login correcto devuelve un token | EUV-01 | API ✅ (cubierto por `test_create_application_end_to_end`, paso 1) |
| CP-02 | Login con contraseña incorrecta | EUV-01 | API ✏️ |
| CP-03 | `GET /api/calls` sin token → 401 | EUV-Q-02 | API ✏️ |
| CP-04 | Presigned URL con `.jpg` → `INVALID_CONTENT_TYPE` | EUV-04 | API ✏️ |
| CP-05 | Presigned URL > 2 MB → `FILE_TOO_LARGE` | EUV-04 | API ✏️ |
| CP-06 | Solicitud sin documentos obligatorios → `REQUIRED_DOCUMENTS_MISSING` | EUV-05 | API ✏️ |
| CP-07 | Segunda solicitud → 409 | EUV-Q-03 | API ✏️ |
| CP-08 | 2 solicitudes simultáneas → 1 persiste | EUV-Q-03 | Concurrencia ✏️ |
