---
id: CTX-20260426-backend-supabase-mvp
tipo: contexto
estado: vigente
fecha_creacion: 2026-04-26
fecha_actualizacion: 2026-04-27
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
  - storage
relacionados:
  - 01-contextos/decisiones/0003-supabase-como-backend-mvp.md
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
  - 01-contextos/frontend-flutter/2026-04-21-contexto-frontend-flutter-mvp.md
reemplaza: []
reemplazado_por: []
---

# Contexto Backend Supabase MVP

## Resumen

Este documento describe la implementación técnica del backend usando Supabase para el MVP de Damkina. Cubre schema de base de datos, autenticación, Edge Functions, Storage y configuración de seguridad.

## Alcance

- Schema de tablas: profiles, locations, location_environment_profiles, crops, crop_images, crop_koppen_preferences, crop_management_factors, location_crop_recommendations.
- Storagebucket público para imágenes de cultivos.
- Edge Functions: get-crops, get-recommendations.
- RLS policies para seguridad.
- APIs externas (clima, geocoding, elevación) manejadas en Flutter.

Queda fuera del alcance: Configuración de Google Auth (pendiente).

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
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: location_environment_profiles

Perfil ambiental separado de la ubicación. Se crea automáticamente cuando Flutter guarda datos del clima.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- | --- |
| id | UUID | PK | ID del perfil |
| location_id | UUID | REFERENCES locations | Ubicación asociada |
| altitude_m | DECIMAL(7,2) | | Altitud en metros |
| koppen_classification | TEXT | | Clasificación Köppen |
| climate_summary | JSONB | | Resumen climático |
| seasonality_summary | JSONB | | Estacionalidad |
| current_season | TEXT | | Temporada actual |
| avg_temp_c | DECIMAL(5,2) | | Temperatura promedio °C |
| temp_range_min_c | DECIMAL(5,2) | | Temp mínima del rango |
| temp_range_max_c | DECIMAL(5,2) | | Temp máxima del rango |
| annual_precip_mm | DECIMAL(7,2) | | Precipitación anual mm |
| regional_humidity_pct | DECIMAL(5,2) | | Humedad regional % |
| data_source | TEXT | | Fuente de datos |
| profile_version | INT | DEFAULT 1 | Versión del perfil |
| calculated_at | TIMESTAMPTZ | | Fecha de cálculo |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |
| updated_at | TIMESTAMPTZ | DEFAULT now() | Fecha de actualización |

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
| ideal_temp_min_c | DECIMAL(5,2) | | Temp mínima ideal °C |
| ideal_temp_max_c | DECIMAL(5,2) | | Temp máxima ideal °C |
| altitude_min_m | DECIMAL(7,2) | | Altitud mínima (m) |
| altitude_max_m | DECIMAL(7,2) | | Altitud máxima (m) |
| preferred_koppen_types | TEXT[] | | Tipos Köppen preferidos |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: crop_images

Imágenes de cultivos. Se guardan en Supabase Storage.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- |
| id | UUID | PK | ID de imagen |
| crop_id | UUID | REFERENCES crops | Cultivo asociado |
| image_url | TEXT | NOT NULL | URL de la imagen |
| alt_text | TEXT | | Texto alternativo |
| sort_order | INT | DEFAULT 0 | Orden de display |
| is_cover | BOOLEAN | DEFAULT false | Es imagen principal |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

### Tabla: crop_koppen_preferences

Preferencias de clasificación Köppen por cultivo.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- |
| id | UUID | PK | ID |
| crop_id | UUID | REFERENCES crops | Cultivo |
| koppen_code | TEXT | NOT NULL | Código Köppen |
| preference_weight | DECIMAL(3,2) | DEFAULT 1.00 | Peso de preferencia |

### Tabla: crop_management_factors

Factores modificables por cultivo (riego, suelo, sombra, etc).

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- |
| id | UUID | PK | ID |
| crop_id | UUID | REFERENCES crops | Cultivo |
| factor_key | TEXT | NOT NULL | Clave (ej: soil_ph) |
| factor_label | TEXT | NOT NULL | Etiqueta (ej: pH del suelo) |
| recommendation | TEXT | NOT NULL | Recomendación |
| is_modifiable | BOOLEAN | DEFAULT true | Es modificable |
| sort_order | INT | DEFAULT 0 | Orden |

### Tabla: location_crop_recommendations

Recomendaciones calculadas por ubicación.

| Columna | Tipo | Restricciones | Descripción |
| --- | --- | --- |
| id | UUID | PK | ID de recomendación |
| location_id | UUID | REFERENCES locations | Ubicación |
| crop_id | UUID | REFERENCES crops | Cultivo |
| user_id | UUID | REFERENCES auth.users | Propietario |
| score | DECIMAL(5,2) | NOT NULL | Score 0-100 |
| recommendation_level | TEXT | NOT NULL | highly_recommended, recommended, possible, not_recommended |
| explanation | TEXT | | Explicación del score |
| rank_position | INT | | Posición en el ranking |
| created_at | TIMESTAMPTZ | DEFAULT now() | Fecha de creación |

## Storage

### Bucket: crops

Bucket público para almacenar imágenes de cultivos.

| Propiedad | Valor |
| --- | --- |
| ID | crops |
| Nombre | crops |
| Público | true (lectura por path, no listing) |
| políticas | service_role: uploads / DELETE |

URL base: `https://lzbnowumqugdgupxupur.supabase.co/storage/v1/object/public/crops/`

## Autenticación

### Provider: Google (pendiente)

- Configurar en Supabase Dashboard → Authentication → Providers → Google.
- Requiere credentials de Google Cloud Console.
- JWT Expiry: 3600 segundos (1 hora).

### Trigger: handle_new_user

```sql
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  IF NEW.raw_user_meta_data->>'name' IS NOT NULL THEN
    INSERT INTO public.profiles (id, display_name, avatar_url)
    VALUES (NEW.id, NEW.raw_user_meta_data->>'name', NEW.raw_user_meta_data->>'avatar_url');
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER
SET search_path = 'public';

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION public.handle_new_user();
```

## Row Level Security (RLS)

| Tabla | Policy | Permiso |
| --- | --- | --- |
| profiles | Users can view own profile | SELECT (propio) |
| profiles | Users can update own profile | UPDATE (propio) |
| profiles | Users can insert own profile | INSERT (propio) |
| locations | Users manage own locations | ALL (propio) |
| location_environment_profiles | Users view own location profiles | SELECT (propio) |
| crops | Anyone can read crops | SELECT (público) |
| crop_images | Anyone can read crop images | SELECT (público) |
| crop_koppen_preferences | Anyone can read koppen preferences | SELECT (público) |
| crop_management_factors | Anyone can read management factors | SELECT (público) |
| location_crop_recommendations | Users manage own recommendations | ALL (propio) |

## Edge Functions

### get-crops

Obtiene catálogo completo de cultivos con imágenes y factores.

**URL:** `https://lzbnowumqugdgupxupur.supabase.co/functions/v1/get-crops`

**Método:** GET

**Respuesta:**

```json
{
  "crops": [
    {
      "id": "uuid",
      "name": "Tomate",
      "slug": "tomate",
      "difficulty": "beginner",
      "min_harvest_days": 60,
      "max_harvest_days": 90,
      "ideal_temp_min_c": 18,
      "ideal_temp_max_c": 27,
      "images_list": [...],
      "koppen_preferences": [...],
      "management_factors": [...]
    }
  ],
  "count": 10
}
```

### get-recommendations

Calcula recomendaciones para una ubicación específica.

**URL:** `https://lzbnowumqugdgupxupur.supabase.co/functions/v1/get-recommendations`

**Método:** POST

**Input:**

```json
{
  "location_id": "uuid-de-la-ubicación"
}
```

**Lógica del score:**

| Factor | Puntos | Descripción |
| --- | --- | --- |
| Temperatura | 0-25 | Diferencia entre temp promedio y rango ideal del cultivo |
| Altitud | 0-25 | Si la altitud está dentro del rango del cultivo |
| Köppen | 0-25 | Si la clasificación es compatible |
| Agua | 0-15 | Requerimiento vs temporada actual |
| Base | 10 | Score base |

**Niveles:**

| Level | Score |
| --- | --- |
| highly_recommended | 80-100 |
| recommended | 60-79 |
| possible | 40-59 |
| not_recommended | 0-39 |

**Respuesta:**

```json
{
  "location_id": "uuid",
  "recommendations": [
    {
      "crop_id": "uuid",
      "crop_name": "Tomate",
      "crop_slug": "tomate",
      "difficulty": "beginner",
      "min_harvest_days": 60,
      "max_harvest_days": 90,
      "score": 95,
      "recommendation_level": "highly_recommended",
      "explanation": "Compatible: temperatura ideal (24°C), altitud adecuada (450m), clima Köppen Aw compatible.",
      "rank": 1
    }
  ],
  "count": 10
}
```

## Variables De Entorno

### Supabase

- `SUPABASE_URL`: `https://lzbnowumqugdgupxupur.supabase.co`
- `SUPABASE_ANON_KEY`: clave pública (en Dashboard)
- `SUPABASE_SERVICE_ROLE_KEY`: clave de servicio (solo servidor)

### Flutter (.env)

```yaml
SUPABASE_URL: https://lzbnowumqugdgupxupur.supabase.co
SUPABASE_ANON_KEY: eyJxxx
```

## Flujo de Datos

```
Flutter App                    Supabase
───────────────────────       ──────────────────────────────
1. Usuario selecciona punto
   en el mapa
                             
2. Flutter llama APIs       →   (geocoding, clima, elevación)
   externas (no Supabase)
                             
3. Flutter guarda         →    locations (coords)
   ubicación              →    location_environment_profiles (climate)
                             
4. Flutter llama        →    Edge Function: get-recommendations
   recomendaciones           Input: { location_id }
   
5. Recibe ◀───────────       Recomendaciones con scores
```

## Consideraciones De Seguridad

1. **Nunca exponer service_role key** en Flutter.
2. **RLS siempre habilitado** en todas las tablas.
3. **JWT verify** off en Edge Functions (público por ahora).
4. **app_metadata** para autorizaciones, no user_metadata.
5. **JWT expiry** 3600 segundos (1 hora).
6. **Storage** bucket crops con política optimizada (no listar todo).
7. **Índices** en foreign keys para performance.

## Vacíos O Pendientes

- **Google Auth**: Pendiente de configurar en Dashboard.
- **JWT Expiry**: ✅ Configurado (3600s).
- **Bucket Security**: ✅ Optimizado (política por path).
- **RLS Performance**: ✅ Optimizado (SELECT auth.uid).
- **Índices**: ✅ Creados en FKs.
- **Datos iniciales de cultivos**: Por poblar.
- **Testing**: Edge Functions en staging.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-04-26 | Creación del contexto backend Supabase MVP | opencode |
| 2026-04-27 | Actualización con nuevo schema, Storage y Edge Functions | opencode |
| 2026-04-27 | Optimizaciones: RLS, índices, bucket security | opencode |
| 2026-04-27 | Optimizaciones: RLS, índices, bucket security | opencode |