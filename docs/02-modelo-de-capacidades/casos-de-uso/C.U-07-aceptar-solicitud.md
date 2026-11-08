# C.U-07 · Aceptar solicitud

> **Alcance:** incluido en la RBS (tajada TJ-02). Usa los estados `PENDING_VALIDATION`, `APPROVED` y `REJECTED` que ya existen en el modelo.
>
> **Avance (revisión del código, 2026-11-07):** no hay endpoints ni pantallas para aprobar o rechazar; otro integrante del equipo está a cargo de terminarlo. El commit `b9a98a1` de `develop` empezó a exponer `reviewedAt` y `rejectionReason` en el detalle de la solicitud, pero la entidad `Application` no tiene esos campos y el backend de `develop` no compila ([R-10](../../06-direccion-y-planeacion/05-riesgos-y-deuda-tecnica.md)).

| Campo | Valor |
|-------|-------|
| Paquete | Convocatorias |
| Actor primario | Administrador |
| Actores secundarios | ✏️ |
| Servicio de negocio | [SN-06](../../01-modelo-de-negocio/05-servicios-de-negocio.md) Reservación de zonas comunes |
| Escenario de servicio | ESN-01 · paso 6 |
| Objetivo del actor | ✏️ |
| Resultado observable | ✏️ |
| Requisitos funcionales relacionados | — |
| Estado | 🔴 No implementado |

## Precondiciones
✏️ **Por completar**

## Postcondiciones
✏️ **Por completar**

## Flujo básico
| Paso | Actor / Sistema | Acción |
|:----:|-----------------|--------|
| 1 | ✏️ | ✏️ |

## Flujos alternativos
| Código | Paso de origen | Condición | Acción | Retorno |
|--------|:--------------:|-----------|--------|---------|
| FA-01 | ✏️ | ✏️ | ✏️ | ✏️ |

## Flujos de excepción
| Código | Condición | Respuesta del sistema |
|--------|-----------|-----------------------|
| FE-01 | ✏️ | ✏️ |

## Reglas de negocio aplicables
✏️ **Por completar** — ver [Reglas de negocio](../../01-modelo-de-negocio/04-reglas-de-negocio.md).

## RNF aplicables
✏️ **Por completar** — ver [Atributos de calidad y RNF](../03-atributos-de-calidad-y-rnf.md).

## EUV asociados
—
