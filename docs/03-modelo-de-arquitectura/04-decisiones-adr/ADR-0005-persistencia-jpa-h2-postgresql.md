# ADR-0005 · Persistencia con JPA; H2 en desarrollo, PostgreSQL previsto

- **Estado:** Aceptado (parcialmente implementado)
- **Fecha:** ✏️
- **Decisores:** ✏️

## Contexto
Se necesita persistencia relacional con restricciones de integridad (unicidad de solicitud) y un entorno de desarrollo y pruebas rápido de levantar.

## Decisión
- Spring Data JPA / Hibernate como capa de persistencia.
- **H2 en memoria** con `ddl-auto=create` y `data.sql` para desarrollo y para las pruebas API en CodeBuild.
- **PostgreSQL** como motor objetivo (dependencia y propiedades preparadas pero comentadas en `pom.xml` y `application.properties`).
- Integridad reforzada con la restricción `uk_application_apartment_call`.

## Consecuencias
- ➕ Arranque inmediato y datos semilla reproducibles.
- ➖ ⚠️ Hoy el despliegue en EC2 usa H2 en memoria: **los datos se pierden en cada reinicio o despliegue**. Ver R-03.
- ➖ Diferencias de dialecto H2 vs. PostgreSQL (p. ej. `SYSTEM_RANGE` en `data.sql`).
- ✏️ Adoptar migraciones (Flyway) al pasar a PostgreSQL.
