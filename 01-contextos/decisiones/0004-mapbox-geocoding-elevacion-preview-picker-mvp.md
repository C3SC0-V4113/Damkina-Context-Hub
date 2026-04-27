---
id: ADR-0004-mapbox-geocoding-elevacion-preview-picker-mvp
tipo: decision
estado: aceptado
fecha_creacion: 2026-04-27
fecha_actualizacion: 2026-04-27
responsables:
  - Francisco Valle
tags:
  - frontend
  - flutter
  - mapbox
  - geocoding
  - elevacion
  - arquitectura
  - ia-contexto
relacionados:
  - 01-contextos/decisiones/0002-mapbox-como-proveedor-mapas-flutter-mvp.md
  - 01-contextos/decisiones/0003-supabase-como-backend-mvp.md
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
  - 01-contextos/backend/2026-04-26-contexto-backend-supabase-mvp.md
reemplaza: []
reemplazado_por: []
---

# ADR-0004: Mapbox Geocoding Y Elevacion Como Preview En Picker MVP

## Estado

Aceptado.

## Contexto Y Problema

ADR-0002 acepto Mapbox para seleccion de ubicacion con `MapPicker`, pero dejo
fuera geocoding y enriquecimiento ambiental real. El frontend ya usa reverse
geocoding y elevacion de Mapbox durante la seleccion para mostrar informacion
de lectura al usuario.

Sin una decision formal, existe contradiccion entre implementacion y reglas
documentadas.

## Decision

Damkina acepta para el MVP:

- Uso de reverse geocoding de Mapbox para mostrar direccion corta en picker.
- Uso de elevation Tilequery de Mapbox para mostrar altitud estimada en picker.
- Estos datos son solo preview UX durante seleccion; no son fuente canonica de
  perfil ambiental agronomico.
- Fallas de estas llamadas no bloquean guardado de ubicacion.
- Se mantiene boundary dedicado para geodata preview separado de `MapPicker`.

## Alternativas Consideradas

| Alternativa | Ventajas | Desventajas | Resultado |
| --- | --- | --- | --- |
| Mantener geocoding/elevacion como pendiente | Menor documentacion inmediata | Contradiccion con codigo actual | Descartada |
| Ampliar ADR-0002 | Menos documentos | Mezcla decisiones distintas y reduce trazabilidad | Descartada |
| Nuevo ADR-0004 para preview geodata | Trazabilidad clara, alcance acotado | Mas documentacion a mantener | Aceptada |

## Consecuencias Positivas

- Se alinea la implementacion actual con reglas aceptadas.
- Se mantiene el enfoque de `MapPicker` para seleccion de coordenadas.
- Se define que direccion/altitud del picker son datos de ayuda y no verdad
  agronomica final.

## Consecuencias Negativas

- Se agrega dependencia explicita de APIs de Mapbox adicionales.
- Puede haber diferencias entre preview mostrado y perfil ambiental final.

## Criterios De Aceptacion

- El picker muestra direccion/altitud cuando Mapbox responde.
- Si Mapbox falla o falta token, la seleccion y guardado siguen funcionando.
- UI no llama clientes Mapbox directamente; usa boundary dedicado de geodata
  preview.
- Documentacion de frontend y reglas locales quedan alineadas con ADR-0002,
  ADR-0003 y ADR-0004.

## Contextos Relacionados

- `01-contextos/decisiones/0002-mapbox-como-proveedor-mapas-flutter-mvp.md`
- `01-contextos/decisiones/0003-supabase-como-backend-mvp.md`
- `01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md`
- `01-contextos/backend/2026-04-26-contexto-backend-supabase-mvp.md`

## Notas De Implementacion

- Este ADR no define proveedor canonico para geocoding/clima/Koppen del perfil
  ambiental persistente.
- El pipeline canonico de perfil ambiental sigue bajo contratos backend y
  decisiones relacionadas con Supabase/Edge Functions.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-27 | Creacion del ADR para geodata preview en picker | Francisco Valle, Codex |
