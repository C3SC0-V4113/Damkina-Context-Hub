---
id: FLUJO-20260329-explorar-cultivo-mvp
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

# Flujo: Explorar Cultivo MVP

## Resumen

Describe cómo el usuario explora cultivos recomendados y revisa el detalle de un cultivo con recomendaciones adaptadas a la ubicación activa.

## Actores

- Usuario final.
- Aplicación móvil Flutter.
- Backend propuesto del MVP, pendiente de ADR formal.
- Motor de recomendación.

## Precondiciones

- El usuario está autenticado.
- Existe una ubicación activa con perfil ambiental.
- Existen cultivos disponibles en el catálogo.
- El sistema puede calcular o recuperar recomendaciones.

## Flujo Principal

1. El usuario entra al home de cultivos.
2. La app muestra una galería de tarjetas ordenadas por score descendente.
3. Cada tarjeta muestra imagen, nombre, nivel de recomendación, dificultad y tiempo mínimo de cosecha.
4. El usuario selecciona un cultivo.
5. La app abre el detalle del cultivo.
6. El usuario revisa información técnica y condiciones ideales.
7. La app muestra factores modificables como suelo, riego, exposición solar y drenaje.
8. La app muestra recomendaciones adaptadas a la ubicación activa.

## Variantes Y Errores

| Caso | Comportamiento Esperado |
| --- | --- |
| No hay ubicación activa | Pedir seleccionar o crear una ubicación. |
| No hay recomendaciones | Mostrar estado vacío con explicación. |
| Error al cargar cultivo | Mostrar error y permitir reintento. |
| Imagen no disponible | Mostrar placeholder o tarjeta sin romper la vista. |
| Score incompleto | Mostrar nivel conservador y explicar que faltan datos. |

## Resultado Esperado

El usuario entiende qué cultivo puede sembrar, por qué fue recomendado y qué factores puede ajustar para mejorar sus probabilidades de éxito.

## Contextos Relacionados

- `01-contextos/producto/2026-03-29-plan-mvp-damkina.md`

## Decisiones Relacionadas

- Pendiente de ADR para motor de recomendación.
- Pendiente de decisión para catálogo de cultivos y fuentes de datos agrícolas.
