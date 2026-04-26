---
id: ADR-0003
tipo: decision
estado: aceptado
fecha_creacion: 2026-04-26
fecha_actualizacion: 2026-04-26
responsables:
  - Francisco Valle
  - David Ruano
tags:
  - backend
  - supabase
  - api
  - database
  - arquitectura
  - auth
relacionados:
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
reemplaza: []
reemplazado_por: []
---

# ADR-0003: Supabase Como Backend MVP Para Damkina

## Estado

Aceptado.

## Contexto Y Problema

El MVP de Damkina requiere un backend para gestionar usuarios, ubicaciones, catálogo de cultivos y motor de recomendaciones. El documento de Plan MVP menciona Node.js como propuesta, pero no es una decisión принята. Había necesidad de definir formalmente el stack backend.

## Decisión

Usar Supabase como backend completo para el MVP de Damkina, incluyendo:

- **Auth**: Supabase Auth con Google Provider
- **Database**: PostgreSQL con Row Level Security (RLS)
- **Storage**: Supabase Storage para imágenes de cultivos
- **Edge Functions**: Motor de recomendaciones en Supabase Edge Functions

## Alternativas Consideradas

| Alternativa | Ventajas | Desventajas | Resultado |
| --- | --- | --- | --- |
| Firebase | SDK maduro, integración con Google | Vendor lock-in, pricing variable | Descartado |
| Node.js + PostgreSQL | Control total | Requiere más setup, sin RLS nativo | Descartado |
| Supabase | Todo en uno, RLS, Edge Functions, free tier | Menor control sobre infraestructura | Seleccionado |

## Consecuencias Positivas

- Autenticación Google integrada sin setup adicional.
- RLS proporciona seguridad de datos sin código.
- 500k Edge Function invocations gratis/mes.
- 1 GB storage gratis.
- Flutter SDK oficial (supabase_flutter).
- Menos operadores que mantener.

## Consecuencias Negativas

- Dependencia de un proveedor externo.
- Edge Functions tienen cold starts.
- Límites en free tier que pueden requerir upgrade.
- Menor control sobre lógica de negocio.

## Criterios De Aceptación

- [x] Auth con Google funciona.
- [x] RLS protege datos de usuarios.
- [x] Edge Function calcula recomendaciones.
- [x] Flutter se conecta correctamente.
- [x] Costos dentro del free tier.

## Contextos Relacionados

- `01-contextos/producto/2026-03-29-plan-mvp-damkina.md` - Plan MVP que requiere backend.
- `01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md` - Frontend que se conectará.
- `01-contextos/backend/2026-04-26-contexto-backend-supabase-mvp.md` - Contexto técnico detallado.

## Notas De Implementación

### Schema Inicial

- `profiles`: extensión de usuarios (display_name, custom_name, avatar_url).
- `locations`: ubicaciones por usuario.
- `crops`: catálogo de cultivos (público).
- `recommendations`: recomendaciones por ubicación (privado).

### Edge Functions

- `get-crops`: obtener catálogo.
- `get-recommendations`: calcular recomendaciones para ubicación.
- `save-recommendations`: guardar recomendaciones en cache.

### RLS

- `profiles`: usuario ve/actualiza solo el propio.
- `locations`: CRUD por usuario propio.
- `crops`: público lectura.
- `recommendations`: usuario ve solo las propias.

### Flutter

- Usar `supabase_flutter` para conexión.
- Configurar URL y ANON_KEY en variables de entorno.
- No exponer service_role key.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-26 | Creación del ADR | opencode |