# Cómo contribuir a la Wiki

## Principios
1. **Fuente de verdad = código.** Si la wiki y el código difieren, se corrige la wiki (o se abre un ADR si el código está mal).
2. **Una página, un propósito.** Si una página crece demasiado, se divide.
3. **Todo lo no verificado se marca** con `✏️ Por completar` o `⚠️ Por confirmar`.
4. **Diagramas como imagen.** No se usa Mermaid: cada diagrama se publica como imagen en `assets/imagenes/` y se conserva su archivo fuente en `assets/diagramas/`.

## Convenciones de nombres
| Elemento | Convención | Ejemplo |
|----------|-----------|---------|
| Carpetas de sección | `NN-nombre-seccion` | `03-modelo-de-arquitectura` |
| Páginas | `NN-tema-en-minusculas.md` | `05-modelo-de-datos.md` |
| Caso de uso | `C.U-NN-nombre.md` | `C.U-03-registrar-solicitud.md` |
| Mecanismo de diseño | `MEC-NN-nombre.md` | `MEC-01-autenticacion-jwt.md` |
| Evidencia de tajada | `TJ-NN-evidencia.md` | `TJ-01-evidencia.md` |
| EUV funcional | `EUV-NN-nombre.md` | `EUV-05-registrar-postulacion.md` |
| EUV de calidad | `EUV-Q-NN-nombre.md` | `EUV-Q-01-concurrencia-postulaciones.md` |
| Decisión arquitectónica | `ADR-NNNN-titulo.md` | `ADR-0004-carga-directa-s3.md` |
| Acta | `AAAA-MM-DD-tema.md` | `2026-09-20-revision-sprint-3.md` |
| Imagen | `seccion-descripcion.png` | `arquitectura-despliegue-aws.png` |

## Cómo insertar artefactos

### Imágenes
Guardar en `assets/imagenes/` y referenciar con ruta relativa:

    ![Pantalla de login](../assets/imagenes/ui-login.png)

### Diagramas
- Exportar el diagrama (Draw.io, PlantUML, Structurizr, etc.) a `.png` o `.svg` en `assets/imagenes/` e insertarlo como imagen.
- Guardar el archivo fuente editable en `assets/diagramas/` para poder modificarlo después.

### Documentos (PDF, Excel, presentaciones)
Guardar en `assets/documentos/` y enlazar: `[Presentación Sprint 3](../assets/documentos/sprint-3.pdf)`.

### Tablas
Usar tablas Markdown. Para matrices grandes (> 15 columnas) adjuntar `.xlsx`/`.csv` en `assets/documentos/` y dejar un resumen en la página.

### Avisos
    > **Nota:** información complementaria.
    > ⚠️ **Por confirmar:** dato pendiente de validar con el equipo.
    > ✏️ **Por completar:** sección pendiente.

## Plantillas disponibles
| Plantilla | Uso |
|-----------|-----|
| [pagina-generica.md](plantillas/pagina-generica.md) | Cualquier página nueva |
| [servicio-de-negocio.md](plantillas/servicio-de-negocio.md) | Servicios (SN) y escenarios de servicio (ESN) |
| [caso-de-uso.md](plantillas/caso-de-uso.md) | Casos de uso C.U (flujo básico, alternativos y de excepción) |
| [rnf.md](plantillas/rnf.md) | Requerimientos no funcionales (ISO/IEC 25010) |
| [mecanismo-de-diseno.md](plantillas/mecanismo-de-diseno.md) | Mecanismos de diseño (MEC) |
| [euv.md](plantillas/euv.md) | EUV funcionales |
| [euv-calidad.md](plantillas/euv-calidad.md) | EUV de calidad (QAS) |
| [evidencia-tajada.md](plantillas/evidencia-tajada.md) | Evidencia de ejecución por tajada |
| [adr.md](plantillas/adr.md) | Decisiones arquitectónicas |
| [acta.md](plantillas/acta.md) | Actas de reunión |

## Flujo de cambios
1. Crear rama `docs/<tema>` (ver [GitFlow](07-codigo-y-despliegue/01-gitflow.md)).
2. Editar/crear páginas usando las plantillas.
3. Si se agregan páginas, actualizar el índice ([README](README.md)). El menú del sitio se genera solo a partir de las carpetas; para cambiar el título u orden de una sección, editar su archivo `.pages`.
4. Pull Request con al menos 1 revisor. Mensaje de commit: `docs: <descripción>`.
