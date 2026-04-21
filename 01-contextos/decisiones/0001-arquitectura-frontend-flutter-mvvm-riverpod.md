---
id: ADR-0001-arquitectura-frontend-flutter-mvvm-riverpod
tipo: decision
estado: aceptado
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
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 05-referencias/referencia-diseno-figma-mobile.md
reemplaza: []
reemplazado_por: []
---

# ADR-0001: Arquitectura Frontend Flutter MVVM + Riverpod

## Estado

Aceptado.

## Contexto Y Problema

Damkina necesita construir el MVP frontend en Flutter con suficiente orden para permitir trabajo asistido por agentes de IA sin caer en cambios grandes, opacos o difíciles de revisar.

El MVP debe implementar login, onboarding, ubicaciones, recomendaciones, detalle de cultivo, mis ubicaciones y perfil. El backend, proveedor de mapas, autenticación real y motor de recomendación aún no tienen decisiones formales aceptadas, por lo que el frontend debe avanzar sin acoplarse prematuramente a esas elecciones.

## Decisión

El frontend MVP usará:

- Flutter con Android como primera plataforma objetivo.
- Arquitectura MVVM con organización feature-first.
- Riverpod para gestión de estado e inyección de dependencias.
- `AsyncValue` de Riverpod para representar estados de carga, error y datos.
- `go_router` para navegación declarativa.
- `Freezed` + `json_serializable` para modelos inmutables y serialización.
- Repositorios con interfaces y fake/local data mientras backend y APIs no estén cerrados.
- `AuthRepository` como adapter para autenticación, evitando acoplar pantallas a Google Sign-In o Firebase antes de una decisión específica.
- `MapPicker` o abstraction equivalente como adapter de selección de ubicación, evitando acoplar la UI directamente a Google Maps, Mapbox u OpenStreetMap antes de una decisión específica.

## Alternativas Consideradas

| Alternativa | Ventajas | Desventajas | Resultado |
| --- | --- | --- | --- |
| MVVM + Riverpod | Testable, dependencias inyectables, buen equilibrio entre orden y velocidad, adecuado para agentes. | Requiere disciplina en providers y capas. | Aceptada. |
| MVVM + Provider | Simple y conocido. | Menos robusto para inyección, overrides, testing y escalamiento. | Descartada. |
| MVVM + BLoC | Muy explícito para flujos complejos. | Más verboso para el MVP y puede ralentizar iteración. | Descartada. |
| Navigator nativo | Menos dependencias. | Más manual para tabs, rutas declarativas y evolución futura. | Descartada frente a `go_router`. |
| Pantallas con datos locales directos | Muy rápido para prototipos. | Acopla UI a datos temporales y dificulta reemplazo por API. | Descartada. |

## Consecuencias Positivas

- El frontend puede avanzar antes de que backend, mapas y auth real estén cerrados.
- Las pantallas se pueden probar con fake data sin depender de servicios externos.
- Los agentes pueden trabajar por feature con menor riesgo de conflictos.
- Las decisiones de estado, navegación y modelos quedan explícitas para futuras implementaciones.

## Consecuencias Negativas

- Requiere codegen para modelos con `Freezed` y `json_serializable`.
- Requiere mantener interfaces de repositorio aunque inicialmente usen fake data.
- Puede sentirse más estructurado que un prototipo rápido de pantallas.

## Criterios De Aceptación

- Las features se organizan por carpeta y no como pantallas sueltas globales.
- Cada pantalla depende de ViewModels/providers, no de fuentes de datos directas.
- Los estados de carga, error y datos usan `AsyncValue` o una abstracción compatible.
- La navegación principal se define con `go_router`.
- El diseño mobile se implementa desde tokens/componentes base, no copiando estilos por pantalla.
- Cambiar proveedor real de auth, mapas o backend no debe obligar a reescribir las pantallas.

## Contextos Relacionados

- `01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md`
- `01-contextos/producto/2026-03-29-plan-mvp-damkina.md`
- `05-referencias/referencia-diseno-figma-mobile.md`

## Notas De Implementación

No quedan aceptadas en este ADR las decisiones sobre backend, Google Sign-In real, Firebase, proveedor de mapas, proveedor de geocoding, motor de recomendación ni APIs externas. Esas decisiones deben documentarse en ADRs posteriores si se vuelven vinculantes.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-21 | Creación del ADR frontend | Francisco Valle, Codex |
