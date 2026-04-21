---
id: CTX-20260329-plan-mvp-damkina
tipo: contexto
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
  - auth
  - database
  - arquitectura
  - ux
  - ia-contexto
relacionados:
  - 01-contextos/producto/2026-03-09-definicion-negocio-damkina.md
  - 02-flujos/flujo-ingreso-inicial-mvp.md
  - 02-flujos/flujo-agregar-ubicacion-mvp.md
  - 02-flujos/flujo-explorar-cultivo-mvp.md
  - 05-referencias/referencia-plan-mvp-google-doc.md
reemplaza: []
reemplazado_por: []
---

# Plan MVP De Damkina

## Resumen

Este documento sintetiza el plan de acción del MVP de Damkina. El MVP busca demostrar que la aplicación puede convertir una ubicación real en una experiencia útil de recomendación agrícola.

El producto se plantea como una app móvil Flutter que permite iniciar sesión, registrar ubicaciones, enriquecerlas con perfil ambiental y mostrar cultivos recomendados con explicación y detalle técnico.

## Nota Técnica Sobre Backend

El documento fuente menciona backend en Node.js como propuesta técnica para el MVP. Esta mención no debe interpretarse como decisión aceptada.

Antes de implementar o asumir Node.js como stack backend oficial, debe crearse una decisión ADR en `01-contextos/decisiones/` que compare alternativas, consecuencias y criterios de aceptación.

## Objetivo Del MVP

Construir una app móvil que permita al usuario:

- Iniciar sesión con Google.
- Personalizar su nombre visible.
- Registrar una o varias ubicaciones desde un mapa.
- Generar un perfil ambiental de cada ubicación.
- Visualizar cultivos recomendados para una ubicación activa.
- Consultar el detalle de cada cultivo con enfoque en factores modificables.

El valor central del MVP es validar que Damkina puede transformar datos de ubicación y entorno en recomendaciones agrícolas comprensibles y accionables.

## Alcance Incluido

### Autenticación

- Inicio de sesión con Google.
- Creación automática del usuario.
- Pantalla opcional para confirmar o editar nombre visible luego del login.
- Persistencia de sesión.

### Gestión De Perfil

- Nombre visible editable.
- Lista de ubicaciones guardadas.
- Selección de ubicación activa.

### Gestión De Ubicaciones

- Agregar ubicación desde un mapa.
- Obtener coordenadas.
- Guardar varias ubicaciones por usuario.
- Asignar nombre a cada ubicación, como `Casa`, `Terreno de Juayúa` o `Parcela 1`.
- Consultar datos no mutables o semiestables de la ubicación: coordenadas, altitud, clasificación Köppen, clima histórico resumido y estacionalidad.

### Recomendaciones De Cultivos

- Lista ordenada desde el cultivo más recomendable hasta el menos recomendable.
- Vista tipo galería con tarjetas.
- Tarjetas con imagen representativa, nombre, nivel de recomendación, dificultad y tiempo mínimo de cosecha.

### Detalle De Cultivo

- Galería de imágenes.
- Nombre del cultivo.
- Tiempo estimado hasta cosecha.
- Tipo de suelo recomendado.
- Horas e intensidad de sol.
- Agua requerida.
- Factores modificables.
- Recomendaciones basadas en la ubicación seleccionada.

## Fuera Del MVP

Por propósito de MVP, quedan fuera principalmente elementos de monetización y funcionalidades avanzadas:

- Pagos.
- Suscripciones premium activas.
- Publicidad real.
- Patrocinadores administrables.
- Favoritos avanzados entre ubicaciones.
- Planificación de cosecha.
- Recordatorios.
- Notificaciones push.
- Diagnóstico por IA.
- Comunidad o red social.
- Panel administrativo completo.

## Estructura Funcional

### Módulo 1. Autenticación Y Usuario

Debe permitir login con Google, persistencia de sesión y edición simple del nombre visible.

Datos mínimos sugeridos del usuario:

- `id`
- `providerId`
- `email`
- `displayName`
- `customName`
- `avatarUrl`

### Módulo 2. Ubicaciones

La ubicación es una entidad central para Damkina. No representa solo un pin en el mapa, sino un perfil agroambiental.

Datos mínimos sugeridos por ubicación:

- `id`
- `userId`
- `name`
- `latitude`
- `longitude`
- `altitude`
- `koppenClassification`
- `climateSummary`
- `seasonalitySummary`
- `currentSeason`
- `createdAt`

### Módulo 3. Perfil Ambiental

Cuando el usuario guarda una ubicación, el backend debe enriquecerla con datos no mutables o semiestables.

Información sugerida:

- Coordenadas.
- Altitud.
- Zona climática.
- Clasificación Köppen.
- Temperatura promedio anual.
- Rango térmico.
- Precipitación anual estimada.
- Estacionalidad.
- Temporada actual.
- Humedad estimada regional.

Este perfil será la base para el score de recomendación.

### Módulo 4. Catálogo De Cultivos

Cada cultivo debe tener información suficiente para compararse con una ubicación.

Datos mínimos sugeridos:

- `id`
- `name`
- `slug`
- `shortDescription`
- `minHarvestDays`
- `maxHarvestDays`
- `difficulty`
- `soilType`
- `soilPhMin`
- `soilPhMax`
- `sunHoursMin`
- `sunHoursMax`
- `sunlightIntensity`
- `waterRequirement`
- `idealTempMin`
- `idealTempMax`
- `altitudeMin`
- `altitudeMax`
- `preferredKoppenTypes`
- `images`

### Módulo 5. Motor De Recomendación

El motor debe tomar ubicación activa, temporada actual, clima histórico resumido, clasificación Köppen y altitud. Luego debe comparar esos datos con los requerimientos de cada cultivo.

Salida esperada:

- Score numérico.
- Nivel de recomendación.
- Explicación resumida.

Clasificación sugerida:

- Altamente recomendado.
- Recomendado.
- Posible con manejo.
- Poco recomendable.

### Módulo 6. Vista De Cultivos

La pantalla principal debe mostrar una galería de cultivos ordenada por score descendente.

Cada tarjeta debe mostrar:

- Imagen.
- Nombre.
- Tag de recomendación.
- Tag de dificultad.
- Tiempo mínimo de cosecha.

### Módulo 7. Detalle De Cultivo

La ficha de cultivo debe orientar al usuario, no solo mostrar datos.

Secciones sugeridas:

- Galería de imágenes.
- Nombre y resumen del cultivo.
- Tiempo hasta cosecha.
- Dificultad.
- Condiciones ideales.
- Factores modificables.
- Recomendaciones específicas para la ubicación seleccionada.

Factores modificables a mostrar:

- Tipo de suelo.
- Corrección de pH.
- Drenaje.
- Cantidad de riego.
- Horas de exposición solar.
- Necesidad de malla sombra.
- Uso de maceta o cama elevada.

Ejemplos de recomendaciones contextualizadas:

- La ubicación es apta para este cultivo por altitud y clima.
- Se recomienda mejorar drenaje si el suelo es arcilloso.
- Durante temporada seca necesitará riego complementario.
- La intensidad solar de la zona puede requerir sombra parcial en etapas tempranas.

## Fuentes De Datos Externas

Para el MVP será necesario enriquecer cada ubicación con datos externos o precalculados.

Datos posibles:

- Geocoding inverso.
- Elevación.
- Clima histórico resumido.
- Clasificación Köppen.

La clasificación Köppen probablemente no vendrá lista desde una API simple en muchos casos. Puede requerir dataset externo, fuente raster/geoespacial o tabla precalculada por zonas.

Para el MVP se recomienda simplificar y evitar precisión científica extrema al inicio, siempre que la experiencia sea útil y honesta sobre sus límites.

## Historias De Usuario

| ID | Historia | Criterios De Aceptación Sintetizados |
| --- | --- | --- |
| HU-001 | Como usuario quiero iniciar sesión con Google para acceder sin crear contraseña manual. | Botón de Google, creación automática si es nuevo, reutilización si existe, entrada al flujo inicial si el login es exitoso. |
| HU-002 | Como usuario autenticado quiero editar mi nombre visible para personalizar mi perfil. | Pantalla inicial de confirmación o edición, guardado del nombre y uso del nombre personalizado en adelante. |
| HU-003 | Como usuario quiero mantener mi sesión iniciada para no autenticarme cada vez. | Entrar directamente con token válido y solicitar login si la sesión expiró. |
| HU-004 | Como usuario quiero agregar una ubicación desde el mapa para registrar un lugar de cultivo. | Selección o movimiento de pin, obtención de latitud/longitud y guardado de la ubicación. |
| HU-005 | Como usuario quiero asignar nombre a una ubicación para identificarla fácilmente. | Campo de nombre después de seleccionar el punto; ejemplos como Casa, Parcela Norte o Terreno de Juayúa. |
| HU-006 | Como usuario quiero guardar varias ubicaciones para consultar distintos terrenos. | Múltiples ubicaciones por usuario y lista visible de ubicaciones guardadas. |
| HU-007 | Como usuario quiero seleccionar una ubicación activa para ver recomendaciones de ese lugar. | Selección desde lista y actualización de cultivos recomendados según la ubicación activa. |
| HU-008 | Como usuario quiero ver el perfil ambiental de una ubicación para entender su contexto agrícola. | Mostrar coordenadas, altitud, Köppen, resumen climático y temporada actual. |
| HU-009 | Como usuario quiero editar el nombre de una ubicación para organizar mis lugares. | Cambiar nombre sin perder coordenadas ni perfil ambiental. |
| HU-010 | Como usuario quiero eliminar una ubicación para mantener limpia mi lista. | Confirmación previa y manejo si se elimina la ubicación activa. |
| HU-011 | Como usuario quiero ver cultivos recomendados para mi ubicación activa para saber qué sembrar. | Lista dependiente de la ubicación activa y ordenada del más recomendable al menos recomendable. |
| HU-012 | Como usuario quiero ver cultivos en galería para explorar opciones rápidamente. | Tarjetas con imagen, nombre, nivel de recomendación, dificultad y tiempo mínimo de cosecha. |
| HU-013 | Como usuario quiero entender por qué un cultivo fue recomendado para confiar en la app. | Explicación breve basada en clima, altitud, temporada o clasificación de zona. |
| HU-014 | Como usuario quiero ver el detalle completo de un cultivo para decidir si sembrarlo. | Ficha con galería, nombre, tiempo estimado, dificultad y descripción breve. |
| HU-015 | Como usuario quiero ver factores modificables del cultivo para adaptar mi entorno. | Mostrar suelo, horas de sol, intensidad solar, agua, drenaje y recomendaciones de manejo. |
| HU-016 | Como usuario quiero recomendaciones basadas en mi ubicación para saber qué ajustes hacer. | Mensajes contextualizados sobre altitud, riego, drenaje o sombra parcial. |
| HU-017 | Como usuario quiero navegar entre ubicaciones, cultivos y detalle sin confusión. | Navegación clara entre perfil, ubicaciones, home de cultivos y detalle. |
| HU-018 | Como usuario quiero ver estados de carga y error para entender qué ocurre. | Estados de loading, vacío y error; manejo de falta de internet, login, carga de ubicación y recomendaciones. |

## Flujos Relacionados

- `02-flujos/flujo-ingreso-inicial-mvp.md`
- `02-flujos/flujo-agregar-ubicacion-mvp.md`
- `02-flujos/flujo-explorar-cultivo-mvp.md`

## Vacíos O Decisiones Pendientes

- Decisión formal de stack backend.
- Selección de proveedor o estrategia de autenticación con Google.
- Proveedor de mapas y geocoding.
- Fuente confiable de elevación, clima histórico y clasificación Köppen.
- Diseño exacto del algoritmo de score.
- Modelo de datos definitivo y persistencia.
- Manejo de privacidad, consentimiento y permisos de ubicación.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-03-29 | Definición del plan MVP en documento fuente | Francisco Valle, David Ruano |
| 2026-04-20 | Incorporación como contexto técnico de producto | Codex |
