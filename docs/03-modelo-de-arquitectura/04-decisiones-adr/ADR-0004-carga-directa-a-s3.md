# ADR-0004 · Carga directa a S3 con URL prefirmada

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️
- **Escenario:** [EUV-Q-05](../../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md)

## Contexto
Cada solicitud requiere hasta 5 PDF de hasta 2 MB. Si los archivos pasaran por la API, en picos (EUV-Q-01) se consumirían memoria, hilos y ancho de banda del backend.

## Decisión
Patrón de carga en tres pasos:
1. `POST /api/documents/presigned-url` → la API valida los metadatos, crea `Document(PENDING)` y devuelve una URL `PUT` prefirmada (5 min).
2. El navegador hace `PUT` directamente a S3.
3. `POST /api/documents/{id}/complete` → la API verifica con `HEAD` que el objeto exista y lo marca `UPLOADED`.

## Alternativas
| Alternativa | Motivo de descarte |
|-------------|-------------------|
| `multipart/form-data` a la API y reenvío a S3 | Doble transferencia; carga en el backend |
| Guardar los archivos en la BD (BLOB) | Crecimiento de la BD, peor rendimiento |

## Consecuencias
- ➕ La API no procesa bytes de archivos.
- ➕ Validación de tipo y tamaño antes de emitir la URL.
- ➖ Requiere CORS en el bucket y manejo de la expiración.
- ➖ Pueden quedar documentos `PENDING` huérfanos (definir limpieza).
- ➕ La URL prefirmada incluye el `Content-Type` y el `Content-Length` declarados (`PutObjectRequest`), por lo que la carga debe coincidir con los metadatos validados.
