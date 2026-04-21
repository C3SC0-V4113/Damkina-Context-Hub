# Índices

Esta carpeta contiene índices manuales para ubicar contexto sin leer todo el repositorio.

## Archivos

| Archivo | Propósito |
| --- | --- |
| `contextos.md` | Inventario general de documentos por tipo, estado, tags y ruta. |
| `decisiones.md` | Registro de decisiones ADR con estado y relaciones. |
| `tags.md` | Catálogo de tags permitidos y significado. |

## Reglas De Mantenimiento

- Cada documento nuevo debe agregarse a `contextos.md`.
- Cada ADR nuevo debe agregarse también a `decisiones.md`.
- Cada tag nuevo debe documentarse en `tags.md`.
- Si un documento cambia de estado, actualiza el índice correspondiente.
- Si un documento reemplaza a otro, refleja la relación en ambos documentos y en los índices.

## Uso Para Agentes

Empieza por estos índices para reducir búsqueda amplia. Si una pregunta menciona un tema, identifica primero su tag y luego consulta los documentos listados para ese tag.
