# Lectura integrada de validación

> **Propósito:** consolidar el estado de validación de los EUV para decidir el siguiente paso.
> **Fuente:** estados registrados en el [catálogo de EUV](../04-modelo-de-validacion/README.md) y en la [matriz de trazabilidad](../08-gobierno/03-matriz-de-trazabilidad.md).

| EUV | C.U / RNF | Evidencia registrada | Resultado |
|-----|-----------|----------------------|-----------|
| [EUV-01](../04-modelo-de-validacion/euv/EUV-01-iniciar-sesion.md) | RF-01 | `test_create_application_end_to_end` (paso 1) | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-02](../04-modelo-de-validacion/euv/EUV-02-consultar-convocatorias.md) | C.U-02 | `test_create_application_end_to_end` | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-03](../04-modelo-de-validacion/euv/EUV-03-verificar-elegibilidad.md) | C.U-03 | `test_create_application_end_to_end` | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-04](../04-modelo-de-validacion/euv/EUV-04-cargar-documentos.md) | C.U-03 | `test_create_application_end_to_end` | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-05](../04-modelo-de-validacion/euv/EUV-05-registrar-postulacion.md) | C.U-03 | `test_create_application_end_to_end` (paso 7), prueba k6 | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-06](../04-modelo-de-validacion/euv/EUV-06-consultar-postulaciones.md) | C.U-04 | `test_create_application_end_to_end` | 🟡 Automatizado; ✏️ registrar ejecución |
| [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) | RNF-01 | k6 300 VU | 🟡 p95 cumple (1508 ms); tasa de fallo 16,7 % no cumple |
| [EUV-Q-02](../04-modelo-de-validacion/euv/EUV-Q-02-acceso-no-autenticado.md) | RNF-02 | — | ✏️ Sin evidencia |
| [EUV-Q-03](../04-modelo-de-validacion/euv/EUV-Q-03-postulacion-duplicada.md) | RNF-03 | — | ✏️ Sin evidencia |
| [EUV-Q-04](../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md) | RNF-04 | Pipeline (CodePipeline, CloudWatch Logs) | ✅ |
| [EUV-Q-05](../04-modelo-de-validacion/euv/EUV-Q-05-carga-documentos-s3.md) | RNF-05 | — | ✏️ Sin evidencia |

## Observación operacional
| Fuente | Qué permite observar |
|--------|----------------------|
| Historial de CodePipeline | Resultado de cada etapa (Source, Build, Test, Deploy) por ejecución |
| CloudWatch Logs (CodeBuild) | Logs de compilación y de las pruebas API de cada ejecución |
| CodeDeploy `ValidateService` | Disponibilidad del backend tras cada despliegue (`/api/health`) |

✏️ **Por completar:** métricas observadas en el entorno desplegado.
