---
id: FLUJO-20260329-agregar-ubicacion-mvp
tipo: flujo
estado: vigente
fecha_creacion: 2026-03-29
fecha_actualizacion: 2026-04-20
responsables:
  - Francisco Valle
  - David Ruano
tags:
  - producto
  - frontend
  - flutter
  - backend
  - api
  - ux
  - ia-contexto
relacionados:
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 05-referencias/referencia-plan-mvp-google-doc.md
reemplaza: []
reemplazado_por: []
---

# Flujo: Agregar Otra Ubicación MVP

## Resumen

Describe cómo un usuario autenticado agrega una ubicación adicional para recibir recomendaciones agrícolas de otro terreno o espacio.

## Actores

- Usuario final.
- Aplicación móvil Flutter.
- Backend propuesto del MVP, pendiente de ADR formal.
- Servicios externos de mapas, elevación, clima o clasificación ambiental.

## Precondiciones

- El usuario está autenticado.
- El usuario tiene acceso a gestión de ubicaciones.
- La app puede abrir un mapa y obtener coordenadas.

## Flujo Principal

1. El usuario entra a gestión de ubicaciones.
2. La app muestra la lista de ubicaciones guardadas.
3. El usuario elige agregar una nueva ubicación.
4. La app abre el mapa.
5. El usuario selecciona un punto o mueve el pin.
6. La app obtiene latitud y longitud.
7. El usuario asigna un nombre a la ubicación.
8. El usuario guarda la ubicación.
9. El sistema enriquece la ubicación con perfil ambiental.
10. La app permite consultar recomendaciones para la nueva ubicación.

## Variantes Y Errores

| Caso | Comportamiento Esperado |
| --- | --- |
| Nombre vacío | Solicitar un nombre antes de guardar. |
| Coordenadas inválidas | Impedir guardado y pedir nueva selección. |
| Error al enriquecer ubicación | Guardar estado pendiente o mostrar error recuperable. |
| Usuario cancela | Volver a la lista sin crear ubicación. |
| Ubicación activa eliminada o cambiada | Actualizar la vista de recomendaciones según el nuevo estado. |

## Resultado Esperado

El usuario tiene una nueva ubicación guardada, identificable por nombre y disponible para recomendaciones.

## Contextos Relacionados

- `01-contextos/producto/2026-03-29-plan-mvp-damkina.md`

## Decisiones Relacionadas

- Pendiente de ADR para stack backend.
- Pendiente de decisión para proveedor de mapas y fuentes ambientales.
