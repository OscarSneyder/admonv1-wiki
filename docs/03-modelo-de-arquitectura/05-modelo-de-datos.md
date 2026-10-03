# Modelo de datos

Fuente: entidades JPA en `model/` (esquema generado con `ddl-auto=create`) y datos semilla en `data.sql`.

## Diagrama entidad-relación
> ✏️ **Diagrama por agregar:** `![Diagrama entidad-relación](../assets/imagenes/modelo-entidad-relacion.png)`

## Restricciones relevantes
| Restricción | Tabla | Propósito |
|------------|-------|-----------|
| `uk_application_apartment_call (apartment_id, call_id)` | applications | RN-04: una solicitud por unidad privada y convocatoria |
| `application_number` único | applications | Identificador legible |
| `s3_key` único | documents | Un objeto S3 por documento |
| `email` único | users | Identidad de login |

## Datos semilla (`data.sql`)
| Entidad | Cantidad | Detalle |
|---------|----------|---------|
| Torres | 1 | `Torre 1` |
| Unidades privadas | 800 | números 101 … 900 |
| Roles | 2 | `ROLE_ADMIN`, `ROLE_RESIDENT` |
| Usuarios | 1 | `residente@test.com` (unidad privada 1, `ROLE_RESIDENT`) |
| Convocatorias | 2 | 200 y 180 cupos, `OPEN` |

## Pendientes
- ✏️ Diccionario de datos detallado (se puede exportar desde la consola H2 en `/h2-console`).
- ⚠️ Estrategia de migraciones para PostgreSQL (Flyway/Liquibase), ya que `ddl-auto=create` recrea el esquema en cada arranque.
