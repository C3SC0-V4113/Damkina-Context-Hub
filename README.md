# Damkina Context Hub

Repositorio Markdown-first para conservar contexto técnico, de producto y de arquitectura de Damkina. Está inspirado en Architecture Decision Records (ADR) y en la idea de usar documentación estructurada como memoria persistente para humanos y agentes de IA.

Este repositorio no contiene código de la aplicación. Su objetivo es mantener decisiones, contexto, flujos, diagramas, resúmenes y referencias de una aplicación con frontend Flutter y backend aún por definir.

## Cómo Debe Usarse Este Repositorio

Antes de responder preguntas técnicas, proponer arquitectura o sugerir cambios relevantes:

1. Lee este `README.md`.
2. Revisa los índices en `00-indices/`.
3. Consulta decisiones en `01-contextos/decisiones/`.
4. Busca documentos relacionados por `tags`, `relacionados`, `reemplaza` y `reemplazado_por`.
5. Si falta contexto, dilo explícitamente y sugiere crear o actualizar un documento.

No asumas decisiones que no estén documentadas. Si una decisión nueva afecta arquitectura, backend, frontend, dominio, integraciones, seguridad, datos o experiencia de usuario, crea o propone crear una decisión ADR.

## Mapa Del Repositorio

```text
README.md                         Entrada principal para humanos y agentes.
AGENTS.md                         Reglas operativas resumidas para agentes.
00-indices/                       Índices manuales para ubicar información.
01-contextos/                     Contextos y decisiones por área.
02-flujos/                        Flujos de negocio, usuario, sistema o proceso.
03-diagramas/                     Diagramas en Markdown, Mermaid o referencias.
04-resumenes/                     Síntesis ejecutivas o snapshots de contexto.
05-referencias/                   Links, notas externas y material fuente.
99-plantillas/                    Templates para crear nuevos documentos.
```

## Dónde Buscar

| Necesidad | Ruta inicial |
| --- | --- |
| Ver todos los tags conocidos | `00-indices/tags.md` |
| Ver todos los contextos registrados | `00-indices/contextos.md` |
| Ver decisiones ADR y su estado | `00-indices/decisiones.md` |
| Entender decisiones arquitectónicas | `01-contextos/decisiones/` |
| Entender frontend Flutter | `01-contextos/frontend-flutter/` |
| Entender backend pendiente o elegido | `01-contextos/backend/` |
| Entender conceptos de negocio | `01-contextos/dominio/` |
| Entender flujos de usuario o sistema | `02-flujos/` |
| Ver diagramas | `03-diagramas/` |
| Leer síntesis rápidas | `04-resumenes/` |
| Consultar fuentes externas | `05-referencias/` |
| Crear documentos nuevos | `99-plantillas/` |

## Convenciones De Nombres

- Contextos generales: `YYYY-MM-DD-slug-descriptivo.md`
- Decisiones tipo ADR: `NNNN-slug-decision.md`
- Flujos: `flujo-slug-descriptivo.md`
- Diagramas: `diagrama-slug-descriptivo.md`
- Resúmenes: `resumen-slug-descriptivo.md`
- Referencias: `referencia-slug-descriptivo.md`

Usa slugs en minúsculas, sin espacios, con guiones medios.

## Frontmatter Obligatorio

Todos los documentos de contexto deben iniciar con frontmatter YAML:

```yaml
---
id: CTX-YYYYMMDD-slug
tipo: contexto | decision | flujo | diagrama | resumen | referencia
estado: borrador | propuesto | aceptado | vigente | deprecated | superseded
fecha_creacion: YYYY-MM-DD
fecha_actualizacion: YYYY-MM-DD
responsables: []
tags: []
relacionados: []
reemplaza: []
reemplazado_por: []
---
```

## Tipos

| Tipo | Uso |
| --- | --- |
| `contexto` | Información estable sobre producto, dominio, frontend, backend, integraciones u operación. |
| `decision` | Decisión significativa con contexto, alternativas y consecuencias. |
| `flujo` | Secuencia de pasos de usuario, negocio, sistema o proceso. |
| `diagrama` | Representación visual o textual de arquitectura, datos o procesos. |
| `resumen` | Síntesis de varios documentos o estado actual de un área. |
| `referencia` | Fuente externa, artículo, enlace, documentación o nota de investigación. |

## Estados

| Estado | Significado |
| --- | --- |
| `borrador` | Documento incompleto o pendiente de ordenar. |
| `propuesto` | Pendiente de revisión o aprobación. |
| `aceptado` | Decisión aprobada. Se usa principalmente en ADRs. |
| `vigente` | Contexto activo que describe el estado actual. |
| `deprecated` | Ya no se recomienda usar, pero se conserva por historia. |
| `superseded` | Reemplazado por otro documento. Debe completar `reemplazado_por`. |

## Tags Iniciales

Usa tags consistentes para que agentes y humanos puedan indexar información sin leer todo el repositorio:

`frontend`, `flutter`, `backend`, `api`, `auth`, `database`, `arquitectura`, `dominio`, `producto`, `ux`, `integracion`, `operaciones`, `seguridad`, `performance`, `ia-contexto`

El catálogo vivo está en `00-indices/tags.md`.

## Relaciones Entre Documentos

Usa estos campos del frontmatter para conectar contexto:

- `relacionados`: documentos que ayudan a entender este archivo.
- `reemplaza`: documentos anteriores que este archivo deja obsoletos.
- `reemplazado_por`: documentos nuevos que sustituyen este archivo.

Si un ADR queda reemplazado, cambia su estado a `superseded` y agrega el archivo nuevo en `reemplazado_por`.

## Cómo Crear Un Documento Nuevo

1. Elige la plantilla en `99-plantillas/`.
2. Copia la estructura al directorio correspondiente.
3. Define `id`, `tipo`, `estado`, fechas, responsables, tags y relaciones.
4. Escribe un resumen corto y una explicación ordenada.
5. Actualiza el índice correspondiente en `00-indices/`.
6. Si el documento cambia una decisión previa, actualiza también el documento reemplazado.

## Cuándo Crear Una Decisión ADR

Crea una decisión ADR cuando se defina o cambie algo que afecte:

- Arquitectura general.
- Stack de frontend o backend.
- Persistencia de datos.
- APIs y contratos entre sistemas.
- Seguridad, autenticación o autorización.
- Integraciones externas.
- Estrategia de despliegue, observabilidad u operación.
- Patrones de estado, navegación o arquitectura Flutter.

Las decisiones ADR viven en `01-contextos/decisiones/` y usan `99-plantillas/decision-adr.md`.

## Flujo Recomendado Para Agentes

Cuando un agente reciba una pregunta sobre Damkina:

1. Leer `README.md` y `AGENTS.md`.
2. Revisar `00-indices/contextos.md`, `00-indices/decisiones.md` y `00-indices/tags.md`.
3. Identificar tags relevantes para la pregunta.
4. Leer los documentos vinculados por esos tags.
5. Revisar si hay ADRs `aceptado`, `deprecated` o `superseded` que afecten la respuesta.
6. Responder citando los archivos consultados.
7. Indicar vacíos de contexto si no existe información suficiente.

## Referencias Base

- https://adr.github.io/
- https://medium.com/all-you-need-is-clean-code/adr-la-memoria-arquitect%C3%B3nica-que-tu-ia-necesita-ed1d350bf73d
