---
id: FLUJO-20260329-ingreso-inicial-mvp
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
  - auth
  - ux
  - ia-contexto
relacionados:
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 05-referencias/referencia-plan-mvp-google-doc.md
reemplaza: []
reemplazado_por: []
---

# Flujo: Ingreso Inicial MVP

## Resumen

Describe el primer ingreso del usuario al MVP, desde la apertura de la app hasta ver cultivos recomendados para su primera ubicación.

## Actores

- Usuario final.
- Aplicación móvil Flutter.
- Proveedor de autenticación con Google.
- Backend propuesto del MVP, pendiente de ADR formal.

## Precondiciones

- El usuario tiene conexión a internet.
- El usuario puede autenticarse con una cuenta de Google.
- La app puede solicitar permisos o interacción para seleccionar una ubicación.

## Flujo Principal

1. El usuario abre la aplicación y ve el splash.
2. La app muestra login con Google.
3. El usuario inicia sesión.
4. Si el usuario es nuevo, se crea automáticamente su cuenta.
5. La app muestra una pantalla para confirmar o editar el nombre visible.
6. El usuario selecciona su primera ubicación en un mapa.
7. La app obtiene coordenadas de la ubicación.
8. El usuario guarda la ubicación con un nombre.
9. El sistema genera o solicita el perfil ambiental de la ubicación.
10. La app muestra el home con cultivos recomendados.

## Variantes Y Errores

| Caso | Comportamiento Esperado |
| --- | --- |
| Login fallido | Mostrar error claro y permitir reintentar. |
| Sesión existente válida | Saltar login y continuar al estado correspondiente. |
| Error al guardar ubicación | Mostrar error y conservar la selección para reintentar. |
| Perfil ambiental no disponible | Mostrar estado de error o pendiente sin inventar recomendaciones. |
| Sin internet | Mostrar estado offline o mensaje de conectividad. |

## Resultado Esperado

El usuario queda autenticado, con nombre visible confirmado, una ubicación guardada y cultivos recomendados visibles para esa ubicación.

## Contextos Relacionados

- `01-contextos/producto/2026-03-29-plan-mvp-damkina.md`

## Decisiones Relacionadas

- Pendiente de ADR para stack backend.
- Pendiente de ADR o decisión técnica para proveedor de autenticación y mapas.
