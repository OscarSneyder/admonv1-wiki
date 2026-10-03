# ADR-0007 · Despliegue in-place sobre una única instancia EC2 persistente

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️
- **Escenarios / requisitos:** [EUV-Q-04](../../04-modelo-de-validacion/euv/EUV-Q-04-despliegue-automatizado.md), RNF-04 · Mecanismo: [MEC-04](../03-mecanismos-de-diseno/MEC-04-despliegue-automatizado-verificado.md)

## Contexto
La infraestructura de producción cuenta con una sola instancia EC2 persistente. Se requiere actualizar el backend en cada liberación sin crear infraestructura adicional por cada despliegue.

## Decisión
Usaremos **AWS CodeDeploy con despliegue *in-place*** sobre la instancia EC2 existente. No se crea una nueva instancia por cada despliegue; CodeDeploy reemplaza el contenedor mediante los hooks de `appspec.yml` y valida `/api/health`.

## Alternativas consideradas
| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|-------------|----------|-------------|--------------------|
| Load Balancer + Auto Scaling + Blue/Green | Despliegue sin interrupción; escalabilidad | Más componentes y más operación | No es necesario para el alcance actual; se introduciría solo para resolver el despliegue |
| Nueva instancia EC2 por cada despliegue | Entorno limpio en cada liberación | Más infraestructura por release | Se busca evitar infraestructura adicional por cada liberación |

## Consecuencias
- ➕ Arquitectura de menor complejidad y menor costo para el alcance actual.
- ➖ Posible ventana breve de indisponibilidad mientras se detiene el contenedor anterior y se inicia el nuevo ([R-08](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)).
- ➕ Compatible con [RNF-08](../../02-modelo-de-capacidades/03-atributos-de-calidad-y-rnf.md) (disponibilidad 20/7): los despliegues pueden programarse fuera de la franja de operación.
- ⚠️ Si aparecen requisitos más fuertes de disponibilidad, escalabilidad o despliegue sin interrupción, la evolución prevista es: Staging EC2 → Selenium E2E → Load Balancer → Auto Scaling → Blue/Green.

## Verificación
`ValidateService` consulta `/api/health` hasta 50 intentos con 1 s de pausa, dentro del límite de 60 s del hook en `appspec.yml`; si el backend no responde, el despliegue queda en estado FAILED.
