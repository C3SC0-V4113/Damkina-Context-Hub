# Indice De Decisiones ADR

Registro manual de decisiones significativas.

| ADR | Titulo | Estado | Fecha | Tags | Reemplazado Por | Ruta |
| --- | --- | --- | --- | --- | --- | --- |
| ADR-0001 | Arquitectura frontend Flutter MVVM + Riverpod | aceptado | 2026-04-21 | `frontend`, `flutter`, `arquitectura`, `ux` | N/A | `01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md` |
| ADR-0002 | Mapbox como proveedor de mapas Flutter MVP | aceptado | 2026-04-24 | `frontend`, `flutter`, `mapas`, `mapbox`, `arquitectura` | N/A | `01-contextos/decisiones/0002-mapbox-como-proveedor-mapas-flutter-mvp.md` |

## Estados Esperados

- `propuesto`: la decision esta en discusion.
- `aceptado`: la decision esta aprobada y debe respetarse.
- `deprecated`: la decision ya no se recomienda, pero no tiene reemplazo definitivo.
- `superseded`: la decision fue reemplazada por otra decision.

## Instrucciones

- Cada ADR vive en `01-contextos/decisiones/`.
- Usa numeracion secuencial de cuatro digitos: `0001`, `0002`, `0003`.
- Si un ADR cambia a `superseded`, completa la columna `Reemplazado Por`.
- Si se acepta una decision que afecta otros contextos, actualiza los documentos relacionados.
