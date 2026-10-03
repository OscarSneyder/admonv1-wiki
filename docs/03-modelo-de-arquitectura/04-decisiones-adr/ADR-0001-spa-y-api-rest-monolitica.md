# ADR-0001 · SPA + API REST monolítica en capas

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️

## Contexto
El sistema tiene un dominio pequeño (convocatorias, solicitudes, documentos), un equipo reducido y un plazo académico. Se necesita separar la interfaz de las reglas de negocio y desplegar de forma sencilla.

## Decisión
- Frontend como **SPA React** en un repositorio independiente.
- Backend como **monolito Spring Boot** con arquitectura en capas (controller → service → repository → model), expuesto como API REST bajo `/api`.

## Alternativas consideradas
| Alternativa | Por qué no |
|-------------|-----------|
| Microservicios | Sobrecosto operativo sin necesidad de escalado independiente |
| Aplicación renderizada en servidor (Thymeleaf) | Menor separación de responsabilidades; experiencia de usuario menos fluida |
| ✏️ | |

## Consecuencias
- ➕ Simplicidad de desarrollo, despliegue y depuración.
- ➕ Frontend y backend evolucionan y se despliegan por separado.
- ➖ Escalado solo vertical u horizontal del monolito completo.
- ➖ Requiere gestionar CORS y un contrato de API estable.
