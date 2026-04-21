---
id: CTX-20260421-contexto-frontend-flutter-mvp
tipo: contexto
estado: vigente
fecha_creacion: 2026-04-21
fecha_actualizacion: 2026-04-21
responsables:
  - Francisco Valle
tags:
  - frontend
  - flutter
  - arquitectura
  - ux
  - ia-contexto
relacionados:
  - 01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 02-flujos/flujo-ingreso-inicial-mvp.md
  - 02-flujos/flujo-agregar-ubicacion-mvp.md
  - 02-flujos/flujo-explorar-cultivo-mvp.md
  - 05-referencias/referencia-diseno-figma-mobile.md
reemplaza: []
reemplazado_por: []
---

# Contexto Frontend Flutter MVP

## Resumen

El MVP frontend de Damkina será una aplicación Flutter con Android como primera plataforma objetivo. El diseño disponible es mobile y proviene de Figma; no existe aún diseño específico para tablets o dispositivos intermedios.

La implementación debe seguir MVVM feature-first con Riverpod, `go_router`, modelos inmutables y repositorios desacoplados de proveedores reales. La IA puede asistir en la codificación, pero bajo cambios pequeños, revisables y trazables a este contexto o al ADR frontend.

## Decisión Arquitectónica Vigente

La arquitectura aceptada está definida en `01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md`.

Resumen operativo:

- MVVM con organización feature-first.
- Riverpod para estado e inyección de dependencias.
- `AsyncValue` para estados `loading`, `error` y `data`.
- `go_router` para navegación.
- `Freezed` + `json_serializable` para modelos y DTOs.
- Repositorios con fake/local data hasta cerrar backend y APIs.
- Adapters para auth y mapas para evitar acoplamiento temprano.

## Estructura Sugerida

```text
lib/
  core/
    config/
    routing/
    theme/
    errors/
    utils/
  shared/
    widgets/
    models/
    providers/
  features/
    auth/
      presentation/
      application/
      domain/
      data/
    onboarding/
      presentation/
      application/
      domain/
      data/
    locations/
      presentation/
      application/
      domain/
      data/
    crops/
      presentation/
      application/
      domain/
      data/
    profile/
      presentation/
      application/
      domain/
      data/
```

Uso esperado por capa:

- `presentation`: views, widgets específicos, layouts y binding visual.
- `application`: ViewModels/providers Riverpod y coordinación de casos de uso.
- `domain`: modelos de dominio, interfaces de repositorio y reglas puras.
- `data`: DTOs, fake data sources, API data sources futuras y mappers.

## Flujo De Datos

```text
View
  -> ViewModel Riverpod
  -> Repository interface
  -> Fake/local data source o API data source
  -> Mapper/Model
  -> AsyncValue en ViewModel
  -> Render en View
```

Reglas:

- Las views no deben leer directamente fake data, clientes HTTP, SDKs de mapas ni SDKs de auth.
- Los ViewModels exponen estado y comandos orientados a UI.
- Los repositorios deben poder cambiar de fake/local a API real sin reescribir pantallas.
- Los errores deben transformarse a mensajes de UI claros antes de llegar a la vista.

## Pantallas Mobile Observadas En Figma

El diseño Figma inspeccionado incluye al menos:

- Login simple.
- Onboarding paso 1: confirmación o edición de nombre.
- Onboarding paso 2: selección de ubicación con mapa.
- Agregar ubicación.
- Editar ubicación.
- Recomendaciones o home de cultivos.
- Detalle de cultivo.
- Mis ubicaciones.
- Perfil.
- Navegación inferior móvil con secciones de cultivos, ubicaciones y perfil.

## Precedencia Entre MVP Y Figma

- El Plan MVP define comportamiento, alcance funcional e historias.
- Figma define composición visual mobile, jerarquía visual y estilo base.
- Si hay conflicto, se debe preservar el comportamiento del MVP y adaptar la UI siguiendo el espíritu del diseño mobile.
- Si Figma muestra una pantalla no descrita con detalle en el MVP, debe tratarse como referencia visual hasta documentar su comportamiento.

## Diseño Mobile-First Flexible

Reglas para implementar antes de tener diseños tablet:

- Priorizar fidelidad visual en mobile.
- Usar constraints, `SafeArea`, scrolling y layouts flexibles para evitar overflow.
- No crear diseño tablet específico sin nueva definición.
- En pantallas más anchas, centrar o limitar el ancho del contenido principal cuando sea necesario.
- Evitar tamaños rígidos que rompan dispositivos Android con alturas o densidades distintas.

## Sistema Visual

El diseño debe traducirse a tokens manuales en Flutter:

- Colores principales y semánticos.
- Tipografías y pesos.
- Espaciado.
- Radios.
- Sombras.
- Componentes base: botones, inputs, tarjetas de cultivo, badges, bottom navigation, app bars y estados vacíos/error/loading.

No se deben copiar estilos pantalla por pantalla si pueden convertirse en tokens o componentes compartidos.

## Navegación Recomendada

Usar `go_router` con rutas declarativas.

Rutas iniciales sugeridas:

- `/login`
- `/onboarding/name`
- `/onboarding/location`
- `/crops`
- `/crops/:cropId`
- `/locations`
- `/locations/new`
- `/locations/:locationId/edit`
- `/profile`

La navegación con bottom navigation debe cubrir cultivos, ubicaciones y perfil. Onboarding y login quedan fuera del shell principal.

## Datos Y Repositorios

Mientras backend y APIs no estén cerrados:

- Usar fake/local data sources.
- Mantener interfaces de repositorio desde el inicio.
- No acoplar pantallas a Node.js, Firebase, Google Sign-In, Google Maps, Mapbox u OpenStreetMap.
- Documentar cualquier cambio de proveedor como ADR si pasa a ser decisión aceptada.

Interfaces conceptuales esperadas:

- `AuthRepository`
- `UserRepository`
- `LocationRepository`
- `CropRepository`
- `RecommendationRepository`
- `MapPicker`

## Trabajo Con Agentes De IA

La IA debe usarse como herramienta agentica con revisión humana estricta, no como vibecoding.

Reglas:

- Trabajar en cambios pequeños y revisables.
- Citar contexto o ADR antes de modificar arquitectura, estado, navegación o estructura.
- Revisar cada diff manualmente.
- Ejecutar checks o tests relevantes antes de aceptar cambios.
- No introducir dependencias nuevas sin justificar contra el ADR o crear una nueva decisión.
- No reescribir pantallas completas si el cambio solicitado es localizado.
- Mantener trazabilidad entre tarea, historia de usuario, flujo y archivo modificado.

## Orden Recomendado De Implementación

1. Core de proyecto: tema, tokens, routing base, estructura feature-first y fake data.
2. Auth y onboarding: login fake/dev, confirmación de nombre y primera ubicación.
3. Ubicaciones: lista, agregar, editar, eliminar y ubicación activa.
4. Cultivos: galería de recomendaciones con fake data.
5. Detalle de cultivo: factores modificables y recomendaciones contextualizadas.
6. Perfil: edición básica y vista de cuenta.
7. Integraciones reales: solo después de ADRs o contratos claros.

## Pruebas Mínimas

Estándar recomendado para MVP:

- Unit tests para ViewModels y repositorios.
- Widget smoke tests para pantallas críticas: login, onboarding, cultivos, detalle y ubicaciones.
- Tests de routing básico para rutas principales.
- Validación manual visual contra Figma en Android.

Golden tests quedan fuera del estándar mínimo inicial porque el diseño mobile aún puede cambiar y no hay diseño tablet definido.

## Vacíos O Decisiones Pendientes

- Backend oficial y contrato API.
- Proveedor real de autenticación Google.
- Proveedor de mapas y geocoding.
- Motor de recomendación real.
- Fuente de imágenes de cultivos.
- Tokens finales si Figma expone variables de diseño más adelante.
- Diseño específico para tablets o tamaños intermedios.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-21 | Definición inicial del contexto frontend Flutter MVP | Francisco Valle, Codex |
