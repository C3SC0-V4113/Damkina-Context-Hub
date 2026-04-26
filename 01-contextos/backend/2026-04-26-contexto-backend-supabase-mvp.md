---
id: CTX-20260426-backend-supabase-mvp
tipo: contexto
estado: vigente
fecha_creacion: 2026-04-26
fecha_actualizacion: 2026-04-26
responsables:
  - Francisco Valle
  - David Ruano
tags:
  - backend
  - supabase
  - database
  - api
  - auth
  - edge-functions
relacionados:
  - 01-contextos/decisiones/0003-supabase-como-backend-mvp.md
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
reemplaza: []
reemplazado_por: []
---

# Contexto Backend Supabase MVP

## Resumen

Este documento describe la implementación técnica del backend usando Supabase para el MVP de Damkina. Cubre schema de base de datos, autenticación, Edge Functions y configuración de seguridad.

## Alcance

- Schema de tablas: profiles, locations, crops, recommendations.
- Autenticación Google via Supabase Auth.
- Edge Functions para motor de recomendaciones.
- RLS policies para seguridad.
- Storage para imágenes de cultivos.

Queda fuera del alcance: APIs externas (geocoding, clima, elevación), que se manejan en Flutter.

## Schema De Base De Datos

### Tabla: profiles

Extiende auth.users con datos adicionales del perfil.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- | --- |
| id | UUID | PK, REFERENCES auth.users | ID del usuario |
| display_name | TEXT | | Nombre de Google o editable |
| custom_name | TEXT | | Nombre personalizado |
| avatar_url | TEXT | | URL del avatar |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: locations

Ubicaciones guardadas por usuario.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- | --- |
| id | UUID | PK | ID de ubicación |
| user_id | UUID | REFERENCES auth.users | Propietario |
| name | TEXT | NOT NULL | Nombre (casa, parcela, etc) |
| latitude | DECIMAL(10,7) | NOT NULL | Latitud |
| longitude | DECIMAL(10,7) | NOT NULL | Longitud |
| altitude | DECIMAL(7,2) | | Altitud en metros |
| koppen_classification | TEXT | | Clasificación Köppen |
| climate_summary | JSONB | | Resumen climático |
| seasonality_summary | JSONB | | Estacionalidad |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: crops

Catálogo de cultivos. Público de lectura.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- | --- |
| id | UUID | PK | ID del cultivo |
| name | TEXT | NOT NULL | Nombre del cultivo |
| slug | TEXT | UNIQUE, NOT NULL | Slug URL-friendly |
| short_description | TEXT | | Descripción corta |
| min_harvest_days | INT | NOT NULL | Días mínimos hasta cosecha |
| max_harvest_days | INT | NOT NULL | Días máximos hasta cosecha |
| difficulty | TEXT | NOT NULL | beginner, intermediate, advanced |
| soil_type | TEXT | | Tipo de suelo preferido |
| soil_ph_min | DECIMAL(4,2) | | pH mínimo del suelo |
| soil_ph_max | DECIMAL(4,2) | | pH máximo del suelo |
| sun_hours_min | INT | | Horas mínimas de sol |
| sun_hours_max | INT | | Horas máximas de sol |
| sunlight_intensity | TEXT | | low, medium, high |
| water_requirement | TEXT | | low, medium, high |
| ideal_temp_min | DECIMAL(5,2) | | Temp mínima ideal °C |
| ideal_temp_max | DECIMAL(5,2) | | Temp máxima ideal °C |
| altitude_min | DECIMAL(7,2) | | Altitud mínima (m) |
| altitude_max | DECIMAL(7,2) | | Altitud máxima (m) |
| preferred_koppen_types | TEXT[] | | Tipos Köppen preferidos |
| images | TEXT[] | | URLs de imágenes |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: recommendations

Recomendaciones cacheadas por ubicación.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- | --- |
| id | UUID | PK | ID de recomendación |
| location_id | UUID | REFERENCES locations | Ubicación |
| crop_id | UUID | REFERENCES crops | Cultivo |
| user_id | UUID | REFERENCES auth.users | Propietario |
| score | DECIMAL(5,2) | NOT NULL | Score 0-100 |
| level | TEXT | NOT NULL | highly_recommended, recommended, possible, not_recommended |
| explanation | TEXT | | | Explicación del score |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

## Autenticación

### Provider: Google

- Configurar en Supabase Dashboard → Authentication → Providers → Google.
- Requiere credentials de Google Cloud Console.
- Flujo: Flutter abre Google Sign-In → Supabase valida → JWT.

### Trigger: Crear perfil automáticamente

Al crear usuario en auth.users, crear registro en profiles:

```sql
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO public.profiles (id, display_name, avatar_url)
  VALUES (NEW.id, NEW.raw_user_meta_data->>'name', NEW.raw_user_meta_data->>'avatar_url');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION public.handle_new_user();
```

## Row Level Security (RLS)

### profiles

```sql
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view own profile"
ON profiles FOR SELECT USING (auth.uid() = id);

CREATE POLICY "Users can update own profile"
ON profiles FOR UPDATE USING (auth.uid() = id);

CREATE POLICY "Users can insert own profile"
ON profiles FOR INSERT WITH CHECK (auth.uid() = id);
```

### locations

```sql
ALTER TABLE locations ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can manage own locations"
ON locations FOR ALL USING (auth.uid() = user_id);
```

### crops

```sql
ALTER TABLE crops ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Anyone can read crops"
ON crops FOR SELECT USING (true);
```

### recommendations

```sql
ALTER TABLE recommendations ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can manage own recommendations"
ON recommendations FOR ALL USING (auth.uid() = user_id);
```

## Edge Functions

### get-crops

Obtiene catálogo de cultivos.

```typescript
// supabase/functions/get-crops/index.ts
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2'

const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
}

Deno.serve(async (req) => {
  if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
  }

  try {
    const supabaseUrl = Deno.env.get('SUPABASE_URL')!
    const supabaseKey = Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!
    const supabase = createClient(supabaseUrl, supabaseKey)

    const { data: crops, error } = await supabase
      .from('crops')
      .select('*')
      .order('name')

    if (error) throw error

    return new Response(JSON.stringify({ crops }), {
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
    })
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      status: 500,
      headers: { ...corsHeaders, 'Content-Type': 'application/json' },
    })
  }
})
```

### get-recommendations

Calcula recomendaciones para una ubicación.

```typescript
// supabase/functions/get-recommendations/index.ts
// Ver documento de Edge Function para lógica completa
```

## Variables De Entorno

### Supabase

- `SUPABASE_URL`: URL del proyecto
- `SUPABASE_ANON_KEY`: clave pública
- `SUPABASE_SERVICE_ROLE_KEY`: clave de servicio (solo servidor)

### Flutter

```yaml
# .env
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=eyJxxx
```

## Consideraciones De Seguridad

1. **Nunca exponer service_role key** en Flutter.
2. **RLS siempre habilitado** en tablas expuestas.
3. **Validar usuario** en Edge Functions con JWT.
4. **app_metadata** para autorizaciones, no user_metadata.
5. **Actualizar JWT expiry** para sesiones sensibles.

## Vacíos O Pendientes

- Datos iniciales de cultivos (por poblar).
- APIs externas integradas en Flutter no afectan schema.
- Exact config de Google Auth en Supabase Dashboard.
- Testing de Edge Functions en staging.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-26 | Creación del contexto backend Supabase MVP | opencode |