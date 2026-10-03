# Pruebas especiales: carga

Ubicación: `admonv1-backend/src/main/java/com/administracionback/admonv1/loadtest/` · Herramienta: **k6**.

## Configuración
| Parámetro | Valor |
|-----------|-------|
| Escenario k6 | `per-vu-iterations` |
| Usuarios virtuales | 300 |
| Iteraciones por VU | 1 |
| Duración máxima | 2 min |
| Destino | `http://100.26.48.222:8080` (EC2, ejecución anterior a Nginx; hoy el backend solo escucha en `127.0.0.1:8080` y la entrada pública es HTTPS :443) |
| Solicitud | `POST /api/applications` `{callId: 1, residentId: __VU, documentIds: []}` |
| Umbrales | `p(95) < 2000 ms`, `http_req_failed < 1 %` |
| Contadores | `status_400`, `401`, `403`, `409`, `500`, `other` |

## Ejecuciones
| Archivo de resultados | Fecha | p95 | Tasa de error | Observaciones |
|-----------------------|-------|-----|---------------|---------------|
| `results-concurrency-300.json` | ✏️ | ✏️ | ✏️ | Ejecución inicial |
| `results-concurrency-300-diagnostic.json` | ✏️ | ✏️ | ✏️ | Diagnóstico |
| `results-concurrency-300-final.json` | ✏️ | **1508 ms** ✅ | **16,7 %** ❌ | 250/300 respuestas 200 |

> **Resultados no vigentes:** se obtuvieron antes de que el backend tomara el residente del JWT (el `residentId = __VU` del script ya se ignora) y antes de publicar la API detrás de Nginx. La prueba debe ajustarse (usuarios semilla y token por VU, destino HTTPS) y repetirse en la tajada TJ-01.

Análisis: ver [EUV-Q-01](euv/EUV-Q-01-concurrencia-postulaciones.md).

## Ejecución
```bash
k6 run --summary-export=results.json applications-load.js
```

## Evidencias
✏️ Adjuntar capturas en `assets/imagenes/` y gráficos de latencia.
