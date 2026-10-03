# Aplicación de validación

> **Propósito:** describir el entorno, los datos y las herramientas con que se ejecutan los EUV.

## Entornos de ejecución
| Entorno | Uso | Fuente |
|---------|-----|--------|
| CodeBuild (`buildspec-api-test.yml`) | Pruebas automatizadas de API | [Automatización de EUV](../04-modelo-de-validacion/04-automatizacion-de-euv.md) |
| Instancia EC2 | Prueba de carga k6 (EUV-Q-01) | [EUV-Q-01](../04-modelo-de-validacion/euv/EUV-Q-01-concurrencia-postulaciones.md) |
| Local | Ejecución manual | [Instalación local](../07-codigo-y-despliegue/04-instalacion-local.md) |

## Datos de validación
- Datos semilla del backend: 1 torre con 800 apartamentos y un usuario residente (`id = 1`).
- ✏️ **Por completar:** datos requeridos por cada EUV en nivel *Detalle*.
