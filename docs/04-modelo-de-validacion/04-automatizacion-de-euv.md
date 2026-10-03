# Automatización de EUV (pruebas de API)

Ubicación: `admonv1-backend/api-tests/test_api.py` · Herramientas: `pytest`, `requests`.

## Variables de entorno
| Variable | Por defecto |
|----------|-------------|
| `BASE_URL` | `http://127.0.0.1:8080` |
| `TEST_EMAIL` | `residente@test.com` |
| `TEST_PASSWORD` | ✏️ (ver datos semilla; no documentar contraseñas reales) |

## Pruebas actuales
| Prueba | Verifica |
|--------|----------|
| `test_create_application_end_to_end` | Flujo de integración de 8 pasos de una solicitud, de extremo a extremo (detalle abajo) |

## Flujo de integración del pipeline
> **Estado:** implementado en `api-tests/test_api.py` (integrado en `main`); se ejecuta en la etapa Test con `aws.s3.key-prefix=ci-tests`. Cubre el camino exitoso; no incluye casos negativos ([D-01](../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)).

| Paso | Validación | EUV que automatiza | Estado |
|:----:|------------|--------------------|--------|
| 1 | `POST /api/auth/login`: obtiene el JWT y valida la respuesta | [EUV-01](euv/EUV-01-iniciar-sesion.md) | 🟢 Implementado |
| 2 | `GET /api/calls`: obtiene las convocatorias disponibles | [EUV-02](euv/EUV-02-consultar-convocatorias.md) | 🟢 Implementado |
| 3 | `GET /api/applications/check?callId=...`: busca una convocatoria elegible | [EUV-03](euv/EUV-03-verificar-elegibilidad.md) | 🟢 Implementado |
| 4 | `POST /api/documents/presigned-url`: genera URL prefirmadas | [EUV-04](euv/EUV-04-cargar-documentos.md) | 🟢 Implementado |
| 5 | `PUT` a S3 mediante la URL prefirmada: carga los archivos | [EUV-04](euv/EUV-04-cargar-documentos.md), [EUV-Q-05](euv/EUV-Q-05-carga-documentos-s3.md) | 🟢 Implementado |
| 6 | `POST /api/documents/{documentId}/complete`: verifica la presencia del objeto | [EUV-04](euv/EUV-04-cargar-documentos.md) | 🟢 Implementado |
| 7 | `POST /api/applications`: crea la solicitud con los documentos | [EUV-05](euv/EUV-05-registrar-postulacion.md) | 🟢 Implementado |
| 8 | `GET /api/applications/{applicationId}`: verifica la solicitud creada | [EUV-06](euv/EUV-06-consultar-postulaciones.md) (parcial) | 🟢 Implementado |

### Aislamiento y seguridad del ambiente de prueba
- Las pruebas de documentos usan el bucket `aes-puj-documents` con el prefijo aislado `ci-tests/`; producción usa `documents/` ([MEC-06](../03-modelo-de-arquitectura/03-mecanismos-de-diseno/README.md)).
- Durante la prueba, el backend firma el acceso a S3 con el rol de CodeBuild; el script de prueba no necesita credenciales AWS porque usa la URL prefirmada.

| Recurso / permiso | Acceso de CI/Test |
|-------------------|-------------------|
| Bucket `aes-puj-documents` | Solo objetos bajo `ci-tests/*` |
| `s3:PutObject` | Necesario para cargar el objeto de prueba |
| `s3:GetObject` | Necesario para el `headObject` usado en `completeUpload` |
| `s3:ListBucket` | No requerido por el flujo |
| `s3:DeleteObject` | No requerido actualmente |

## Ejecución local
```bash
cd admonv1-backend
python -m pip install pytest requests
python -m pytest api-tests -v
```

## En el pipeline
Se ejecutan en `buildspec-api-test.yml` contra el mismo `app.jar` que se desplegará (barrera de calidad antes de CodeDeploy, [MEC-04](../03-modelo-de-arquitectura/03-mecanismos-de-diseno/MEC-04-despliegue-automatizado-verificado.md)). Los logs de cada ejecución quedan en CloudWatch Logs.

## Resultados
| Fecha | Build | Resultado | Evidencia |
|-------|-------|-----------|-----------|
| ✏️ | ✏️ | ✏️ | `![](../assets/imagenes/pruebas-api-codebuild.png)` |
