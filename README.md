# ADMON v1 — Wiki

Documentación del proyecto ADMON v1 (Sistema de Administración de Conjuntos Residenciales de Propiedades Horizontales), publicada en
**https://oscarsneyder.github.io/admonv1-wiki/**.

- Código: [admonv1-backend](https://github.com/ByteCscript/admonv1-backend) · [admonv1-frontend](https://github.com/ByteCscript/admonv1-frontend)
- Contenido: carpeta [`docs/`](docs/) — cada carpeta es una sección del menú.
- Guía para contribuir: [docs/CONTRIBUIR.md](docs/CONTRIBUIR.md)

## Estructura

| Ruta | Para qué sirve |
|---|---|
| `docs/**` | Páginas Markdown (el menú sigue el orden de las carpetas `00-`, `01-`, …) |
| `docs/**/.pages` | Título de la sección en el menú y orden opcional de sus páginas |
| `docs/assets/` | Imágenes, diagramas y documentos referenciados desde las páginas |
| `mkdocs.yml` | Configuración del sitio (tema, búsqueda, extensiones Markdown) |
| `.github/workflows/pages.yml` | Publica el sitio en GitHub Pages en cada push a `main` |

## Vista previa local

Requiere Python 3.9+.

```bash
pip install -r requirements.txt
mkdocs serve          # http://127.0.0.1:8000, se recarga al guardar
mkdocs build --strict # igual que la publicación, pero falla ante enlaces rotos
```

## Agregar una página

1. Crear el `.md` en la carpeta de su sección (partir de `docs/plantillas/`).
2. Aparece sola en el menú, ordenada por nombre de archivo. Para cambiar el orden o el título de una sección, editar el `.pages` de esa carpeta.
3. Hacer push a `main`; el sitio se actualiza en ~1 minuto.
