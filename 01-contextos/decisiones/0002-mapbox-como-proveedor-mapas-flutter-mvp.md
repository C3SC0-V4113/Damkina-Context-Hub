---
id: ADR-0002-mapbox-como-proveedor-mapas-flutter-mvp
tipo: decision
estado: aceptado
fecha_creacion: 2026-04-24
fecha_actualizacion: 2026-04-24
responsables:
  - Francisco Valle
tags:
  - frontend
  - flutter
  - mapas
  - mapbox
  - arquitectura
  - ia-contexto
relacionados:
  - 01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
  - 02-flujos/flujo-ingreso-inicial-mvp.md
  - 02-flujos/flujo-agregar-ubicacion-mvp.md
reemplaza: []
reemplazado_por: []
---

# ADR-0002: Mapbox Como Proveedor De Mapas En Flutter MVP

## Estado

Aceptado.

## Contexto Y Problema

El MVP requiere seleccion de ubicaciones en onboarding y en gestion de
ubicaciones. El ADR-0001 obliga a mantener un adapter de mapas (`MapPicker`)
para no acoplar la UI a un SDK especifico sin una decision formal.

Hasta ahora el proveedor de mapas estaba pendiente, lo que bloquea validar de
forma real los flujos HU-004, HU-005 y HU-006.

## Decision

Damkina adopta Mapbox como proveedor de mapas para el frontend Flutter del MVP,
con las siguientes reglas:

- Se mantiene `MapPicker` como boundary obligatorio entre UI y SDK.
- Onboarding y locations (crear/editar) usan un picker reusable con Mapbox.
- La seleccion principal usa pin fijo al centro y confirmacion explicita.
- Se habilita boton de ubicacion actual con permisos runtime.
- El estilo base del mapa en MVP es `Outdoors`.
- El token se inyecta solo por `--dart-define MAPBOX_ACCESS_TOKEN=...`.
- Si falta token, la app muestra error recuperable sin crash.

## Alternativas Consideradas

| Alternativa | Ventajas | Desventajas | Resultado |
| --- | --- | --- | --- |
| Mantener fake map picker | Costo minimo inmediato. | No valida flujos reales del MVP. | Descartada. |
| Google Maps | Ecosistema conocido. | No hay decision previa ni integracion activa en Damkina. | Descartada en este ciclo. |
| OpenStreetMap u otros | Flexibilidad de proveedor. | Mayor trabajo de evaluacion e integracion inicial. | Descartada en este ciclo. |
| Mapbox con adapter `MapPicker` | Permite flujo real sin romper arquitectura. | Requiere token y setup nativo minimo. | Aceptada. |

## Consecuencias Positivas

- Se desbloquean flujos reales de seleccion de ubicacion en onboarding y locations.
- La arquitectura del ADR-0001 se mantiene por el boundary `MapPicker`.
- Tests siguen estables al poder usar `FakeMapPicker` en pruebas.

## Consecuencias Negativas

- Se incorpora dependencia externa de mapas y configuracion de token.
- Se agrega manejo de permisos de ubicacion en Android/iOS.
- Geocoding y enriquecimiento ambiental siguen pendientes de decision posterior.

## Criterios De Aceptacion

- Onboarding permite guardar la primera ubicacion usando Mapbox.
- Locations permite crear y editar ubicaciones usando el mismo picker reusable.
- El flujo no crashea cuando falta `MAPBOX_ACCESS_TOKEN`.
- La UI no depende directamente del SDK; usa `MapPicker`.
- `flutter analyze` y `flutter test` se mantienen en verde.

## Contextos Relacionados

- `01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md`
- `01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md`
- `02-flujos/flujo-ingreso-inicial-mvp.md`
- `02-flujos/flujo-agregar-ubicacion-mvp.md`

## Notas De Implementacion

Esta decision cubre solo seleccion de ubicacion en mapa. No cubre aun
geocoding, perfil ambiental real, ni proveedor definitivo de datos climaticos.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-24 | Creacion del ADR Mapbox para Flutter MVP | Francisco Valle, Codex |
