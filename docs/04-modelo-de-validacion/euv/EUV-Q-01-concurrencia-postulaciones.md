# EUV-Q-01 · Rendimiento: 300 solicitudes concurrentes

| Parte del QAS | Descripción |
|---------------|-------------|
| **Fuente** | 300 residentes |
| **Estímulo** | Envían `POST /api/applications` al mismo tiempo (apertura de convocatoria) |
| **Artefacto** | Backend API + base de datos |
| **Entorno** | Operación normal en producción (entrada HTTPS :443 por Nginx) |
| **Respuesta** | Cada solicitud se procesa (se registra o se rechaza con un error de negocio) sin fallos de infraestructura |
| **Medida** | p95 de `http_req_duration` < 2000 ms · `http_req_failed` < 1 % |

> ✏️ **Diagrama por agregar:** `![Escenario EUV-Q-01](../../assets/imagenes/esc-q-01-concurrencia-postulaciones.png)`

## Resultado de la ejecución (`results-concurrency-300-final.json`)
| Métrica | Valor | Umbral | Cumple |
|---------|-------|--------|:------:|
| Solicitudes | 300 (14,2 req/s) | — | — |
| Latencia promedio | 873 ms | — | — |
| Latencia mediana | 966 ms | — | — |
| p90 | 1468 ms | — | — |
| **p95** | **1508 ms** | < 2000 ms | ✅ |
| Máxima | 2453 ms | — | — |
| **Tasa de fallo** | **16,7 % (50/300)** | < 1 % | ❌ |
| Checks `HTTP 200` | 250 / 300 (83,3 %) | — | — |

> **Resultados no vigentes:** la ejecución usó `http://100.26.48.222:8080` y `residentId = __VU`, antes de que el backend tomara el residente del JWT y de publicar la API detrás de Nginx. Con un solo usuario semilla, hoy todas las solicitudes corresponderían a la misma unidad privada. Se debe repetir con 300 residentes semilla y un token por usuario virtual (TJ-01).

## Tácticas arquitectónicas relacionadas
- Carga de archivos fuera de la API (ADR-0004), para que la solicitud solo lleve metadatos.
- Restricción única en BD como control ante la concurrencia (ADR-0005).
- ✏️ Pool de conexiones, índices, escalado horizontal.

## Evidencias
- Script: `admonv1-backend/src/main/java/.../loadtest/applications-load.js`
- ✏️ Captura del resumen de k6: `![k6](../../assets/imagenes/pruebas-k6-300vu.png)`
