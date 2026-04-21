# Guía Para Agentes

Este repositorio es la memoria documental de Damkina. Antes de responder, diseñar o proponer cambios, usa los documentos existentes como fuente de verdad.

## Orden De Lectura

1. `README.md`
2. `00-indices/README.md`
3. `00-indices/contextos.md`
4. `00-indices/decisiones.md`
5. `00-indices/tags.md`
6. Documentos relacionados por carpeta, tag o frontmatter.

## Reglas Operativas

- No inventes contexto si no existe en el repositorio.
- Cita los archivos consultados cuando respondas sobre decisiones o contexto del proyecto.
- Antes de proponer arquitectura, revisa `01-contextos/decisiones/`.
- Antes de proponer backend, revisa `01-contextos/backend/` y ADRs relacionados.
- Antes de proponer cambios Flutter, revisa `01-contextos/frontend-flutter/`.
- Para frontend Flutter, revisa el ADR aceptado `01-contextos/decisiones/0001-arquitectura-frontend-flutter-mvvm-riverpod.md` antes de modificar arquitectura, navegación, estado o estructura.
- Al trabajar con IA en frontend, usa cambios pequeños, revisa cada diff con criterio humano y evita vibecoding.
- Si una propuesta contradice una decisión aceptada, adviértelo antes de continuar.
- Si una decisión aceptada debe cambiar, sugiere crear un nuevo ADR y marcar el anterior como `superseded`.
- Si falta información importante, sugiere crear un documento desde `99-plantillas/`.

## Criterio De Respuesta

Una respuesta técnica debe distinguir claramente entre:

- Contexto documentado.
- Inferencias razonables basadas en documentos.
- Vacíos de información.
- Recomendaciones nuevas que todavía necesitan ADR o validación.

## Mantenimiento

Cuando se agregue o cambie un documento:

1. Validar que tenga frontmatter completo.
2. Validar que use tags conocidos o actualizar `00-indices/tags.md`.
3. Actualizar `00-indices/contextos.md`.
4. Si es una decisión, actualizar `00-indices/decisiones.md`.
5. Si reemplaza otro documento, actualizar `reemplaza`, `reemplazado_por` y estados.
