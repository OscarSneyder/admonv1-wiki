# ADR-0008 · Nginx como punto único de entrada HTTPS

- **Estado:** Aceptado
- **Fecha:** ✏️
- **Decisores:** ✏️
- **Escenarios / requisitos:** RNF-02 · Mecanismo: [MEC-07](../03-mecanismos-de-diseno/README.md)

## Contexto
El backend se ejecuta en un contenedor Docker en la instancia EC2 y debe ser accesible desde Internet de forma segura, sin exponer directamente sus puertos internos.

## Decisión
Usaremos **Nginx como proxy inverso** que termina HTTPS en el puerto 443 y redirige el tráfico al backend, publicado solo localmente en `127.0.0.1:8080`:

`Internet → HTTPS :443 → Nginx → 127.0.0.1:8080 → Spring Boot (Docker)`

El pipeline actualiza el backend sin cambiar este punto de entrada.

## Alternativas consideradas
| Alternativa | Ventajas | Desventajas | Motivo de descarte |
|-------------|----------|-------------|--------------------|
| Exponer el contenedor directamente | Menos componentes | Puertos internos expuestos; TLS dentro de la aplicación | Contradice el aislamiento del runtime |
| ✏️ | | | |

## Consecuencias
- ➕ Centraliza TLS y mantiene el backend sin exposición directa.
- ➕ Los despliegues no alteran el punto de entrada público.
- ➖ Nginx es un componente adicional que se debe configurar y mantener en la instancia.

## Verificación
✏️ **Por completar:** comprobar que el puerto 8080 no es accesible desde Internet y que el certificado TLS es válido.
