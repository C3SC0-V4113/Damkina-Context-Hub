---
id: CTX-20260309-definicion-negocio-damkina
tipo: contexto
estado: vigente
fecha_creacion: 2026-03-09
fecha_actualizacion: 2026-04-20
responsables:
  - Francisco Valle
  - David Ruano
tags:
  - producto
  - dominio
  - ux
  - ia-contexto
relacionados:
  - 01-contextos/producto/2026-03-29-plan-mvp-damkina.md
reemplaza: []
reemplazado_por: []
---

# Definición De Negocio De Damkina

## Resumen Ejecutivo

Damkina es una aplicación móvil inteligente para recomendar cultivos según el entorno geográfico del usuario. Su propuesta inicial se resume en: "Tu entorno. Tus cultivos."

La aplicación busca ayudar a personas sin experiencia agrícola, aficionados a la jardinería urbana y pequeños productores a identificar qué cultivos pueden sembrar exitosamente en su ubicación y en el momento adecuado del año. Para lograrlo, Damkina interpreta datos disponibles desde el dispositivo o servicios asociados, como ubicación geográfica, altitud, clima, temporada y condiciones ambientales relevantes.

El producto está pensado como una plataforma freemium: funciones esenciales gratuitas para facilitar adopción y accesibilidad, junto con funcionalidades avanzadas bajo suscripción premium. También contempla ingresos por publicidad integrada y patrocinios de proveedores agrícolas, siempre que las recomendaciones sigan siendo relevantes y útiles para el usuario.

## Problema Real

Las personas interesadas en cultivar plantas, alimentos o jardines domésticos suelen enfrentar incertidumbre al elegir qué sembrar. La mayoría de recursos disponibles ofrecen recomendaciones generales que no consideran diferencias importantes entre regiones, clima, altitud o temporada.

Las dificultades principales son:

- Desconocimiento sobre qué cultivos son adecuados para el clima local.
- Falta de claridad sobre temporadas apropiadas de siembra.
- Dificultad para interpretar requisitos técnicos de las plantas.
- Ausencia de herramientas que conecten condiciones ambientales reales con cultivos específicos.

Como resultado, muchos intentos de cultivo fallan por seleccionar plantas incompatibles con el entorno o por no considerar condiciones mínimas para su desarrollo.

## Solución Propuesta

Damkina propone simplificar la toma de decisiones de cultivo mediante una aplicación móvil que analiza el entorno del usuario y genera recomendaciones personalizadas.

El funcionamiento base esperado es:

1. Detectar la ubicación geográfica del usuario.
2. Interpretar condiciones ambientales relevantes como clima, altitud y temporada.
3. Comparar esas condiciones con una base de conocimiento agrícola.
4. Presentar cultivos recomendados segun compatibilidad con el entorno.
5. Mostrar fichas informativas con requisitos practicos de cada cultivo.

Las fichas de cultivo deben explicar necesidades de luz, agua, tipo de suelo, duración aproximada del cultivo y recomendaciones básicas para mejorar la probabilidad de éxito.

## Propuesta De Valor

El valor principal de Damkina es convertir información ambiental compleja en recomendaciones claras, personalizadas y accionables.

A diferencia de guías agrícolas generales, Damkina busca interpretar el entorno específico del usuario para responder preguntas prácticas como:

- Qué puedo cultivar en mi ubicación actual.
- Qué cultivos son adecuados para esta temporada.
- Qué condiciones debo considerar antes de sembrar.
- Qué acciones o productos podrían ayudar a mejorar el resultado.

La aplicación no solo informa qué cultivar, sino que también orienta al usuario sobre cómo cuidar mejor sus cultivos mediante recomendaciones de prácticas, recursos y productos relacionados.

## Público Objetivo

Damkina está orientada a personas interesadas en cultivar plantas a pequeña o mediana escala, especialmente en contextos urbanos, domésticos o de experimentación agrícola.

Usuarios potenciales:

- Personas que desean iniciar huertos domesticos.
- Aficionados a la jardineria urbana.
- Propietarios de patios, terrazas o balcones con espacio para cultivo.
- Pequeños productores interesados en experimentar con nuevos cultivos.
- Usuarios principiantes que necesitan orientación simple.
- Usuarios con experiencia que desean una referencia rápida de compatibilidad ambiental.

La experiencia debe ser accesible para principiantes sin impedir que usuarios con más experiencia encuentren información útil.

## Producto

Damkina se desarrollará como una aplicación móvil que integra datos ambientales con una base de conocimiento agrícola.

Funcionalidades esenciales previstas:

- Detección automática de ubicación geográfica.
- Interpretación de condiciones ambientales relevantes.
- Generación de listas de cultivos recomendados por compatibilidad.
- Acceso a fichas informativas detalladas por cultivo.
- Presentacion clara de requisitos de luz, agua, suelo y duracion aproximada.

Funcionalidades posibles para etapas avanzadas:

- Calendarios de siembra.
- Seguimiento de cultivos.
- Recomendaciones más detalladas según condiciones específicas del espacio.
- Sugerencias de productos agrícolas como fertilizantes, abonos, correctores de pH, controladores de plagas o tratamientos preventivos.

El frontend previsto es Flutter. El backend, la base de datos, las fuentes de datos climáticos/geográficos y la arquitectura de recomendación siguen sin definirse y deben decidirse mediante ADRs.

## Modelo De Negocio

Damkina adoptará un modelo freemium.

El plan gratuito debe permitir:

- Acceso a recomendaciones básicas de cultivos.
- Acceso a fichas informativas necesarias para comenzar a cultivar.
- Uso suficiente para validar valor sin obligar a una suscripción temprana.

El plan premium podría incluir:

- Calendarios de siembra.
- Seguimiento de cultivos.
- Recomendaciones avanzadas.
- Personalización basada en condiciones específicas del espacio de cultivo.

Fuentes adicionales de ingreso:

- Publicidad integrada.
- Patrocinios de fabricantes o distribuidores de productos agrícolas.
- Productos destacados dentro de recomendaciones relacionadas con el cuidado de cultivos.

La política de publicidad y patrocinio debe preservar la confianza del usuario. Las recomendaciones patrocinadas deben ser útiles, relevantes y claramente distinguibles cuando corresponda.

## Mision

Facilitar el cultivo accesible y exitoso para cualquier persona mediante tecnología móvil que interpreta el entorno del usuario y lo transforma en recomendaciones claras y prácticas sobre qué cultivar y cómo hacerlo.

Damkina busca reducir las barreras de conocimiento para quienes desean cultivar, usando datos ambientales reales para apoyar decisiones informadas y optimizar recursos disponibles.

## Vision

Convertirse en una plataforma tecnológica de referencia para la planificación y orientación de cultivos a pequeña y mediana escala.

A largo plazo, Damkina aspira a conectar datos ambientales, conocimiento agrícola y herramientas digitales para apoyar una nueva generación de cultivadores y promover prácticas más accesibles, sostenibles y adaptadas a cada entorno.

## Justificacion

Damkina se fundamenta en la intersección entre tecnología móvil, acceso a datos ambientales y creciente interés por prácticas de cultivo doméstico, jardinería urbana y producción local de alimentos.

Los teléfonos inteligentes permiten integrar ubicación, clima, temporalidad y otros datos contextuales para ofrecer servicios personalizados. Sin embargo, existe una brecha entre esos datos y la capacidad de usuarios no especializados para convertirlos en decisiones prácticas de cultivo.

Damkina busca cubrir esa brecha mediante una plataforma que traduzca condiciones ambientales en recomendaciones comprensibles y accionables.

## Vacíos O Decisiones Pendientes

Estos puntos necesitan definicion futura y probablemente ADRs:

- Stack backend, framework, lenguaje y plataforma de despliegue.
- Base de datos y modelo de persistencia para cultivos, condiciones y recomendaciones.
- Fuente de datos agrícolas inicial y estrategia para validar su calidad.
- APIs climáticas, geográficas y de altitud a utilizar.
- Criterios exactos del motor de recomendación y ponderación de compatibilidad.
- Política de uso de ubicación, privacidad y permisos del dispositivo.
- Alcance exacto del plan premium.
- Política de publicidad, productos patrocinados y transparencia para el usuario.
- Reglas UX para explicar recomendaciones sin sobrecargar a principiantes.

## Historial

| Fecha | Cambio | Responsable |
| --- | --- | --- |
| 2026-03-09 | Definición inicial de negocio del proyecto | Francisco Valle, David Ruano |
| 2026-04-20 | Incorporación como contexto maestro Markdown | Codex |
