---
name: sdd-spec
description: Genera la especificación funcional specs/NNN-slug/spec.md (QUÉ y POR QUÉ, nunca CÓMO) entrevistando al usuario una pregunta cada vez. Requisitos con identificadores estables RF-001..., formato EARS, marcador [NECESITA ACLARACIÓN] para lo no resuelto, sin decisiones técnicas. Determina el siguiente número libre sin sobrescribir specs. Úsala tras sdd-agents para una funcionalidad nueva, o cuando sdd-orchestrator indique que falta una spec.
---

# sdd-spec — Especificación funcional

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA`. Marcador: `[NECESITA ACLARACIÓN: ...]`.
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta y actualiza el valor de las filas `Estado`, `Aprobación` y `Versión`; nunca busques ni escribas `Estado: …` como texto libre.
- Requisitos: `RF-001`, `RF-002`… Requisitos no funcionales: `RNF-001`…
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Definir QUÉ debe hacer el sistema y POR QUÉ, en `specs/NNN-slug/spec.md`, de forma observable, verificable e inequívoca.

## Alcance

- SOLO especifica comportamiento.
- NO diseña arquitectura ni elige tecnologías. NO escribe código ni tests.

## Cuándo usar

- Tras `sdd-agents` en un proyecto nuevo.
- Para una funcionalidad nueva sin spec propia cuando `sdd-orchestrator` lo indique.

## Precondiciones

- `docs/constitution.md` con `Estado: APROBADA`. Si no: `SPEC BLOQUEADA — Falta constitution aprobada. Siguiente paso: sdd-constitution`.
- `AGENTS.md` existe. Si no: `Siguiente paso: sdd-agents`.
- No existe otra spec en curso sin completar (si existe, avisa y confirma con el usuario si esta nueva spec es lo que quiere).

## Contexto requerido

Lee ANTES de preguntar nada:

1. `docs/constitution.md`
2. `AGENTS.md`
3. `docs/brief.md` (si existe)
4. Specs existentes relevantes (para coherencia y para no duplicar requisitos).

## Entradas

- La idea o necesidad funcional que motiva la spec.

## Procedimiento

1. **Numeración.** Lista `specs/`. El siguiente número = (máximo existente `NNN-`) + 1, a 3 dígitos (`001`, `002`…). Nunca reutilices ni sobrescribas números existentes. Slug: kebab-case corto y descriptivo (ej. `001-autenticacion`).
2. **Entrevista con UNA pregunta cada vez** sobre: problema y contexto · actores y usuarios · comportamiento esperado · permisos y roles · eventos · estados · datos involucrados (semántica, no modelo físico) · límites y volúmenes · errores esperados · seguridad funcional · casos límite · fuera de alcance · criterios de aceptación.
   - NO preguntes por tecnología salvo que afecte al comportamiento observable.
   - Si el usuario no sabe o no decide algo: escribe `[NECESITA ACLARACIÓN: <duda concreta>]` en el punto afectado. Nunca lo resuelvas tú.
3. Redacta `specs/NNN-slug/spec.md` con la plantilla.
4. Aplica a cada RF el **test de verificabilidad**: "¿Puedo escribir al menos un test que demuestre este requisito?" Si no, reescríbelo o márcalo `[NECESITA ACLARACIÓN: ...]`.
5. Muestra la spec completa. Recomienda: `Siguiente paso: sdd-clarify`.

### Formato EARS (úsalo cuando aplique)

```text
EL SISTEMA <respuesta>.
CUANDO <evento>, EL SISTEMA <respuesta>.
MIENTRAS <estado>, EL SISTEMA <respuesta>.
DONDE <condición>, EL SISTEMA <respuesta>.
SI <error>, ENTONCES EL SISTEMA <respuesta>.
```

### Plantilla de `specs/NNN-slug/spec.md`

```markdown
# <Título de la funcionalidad>

| Campo | Valor |
|---|---|
| Spec-ID | NNN-slug |
| Versión | 1 |
| Estado | BORRADOR |
| Aprobación | PENDIENTE |

## Contexto
## Problema
## Objetivo
## Actores
## Historias de usuario
Como <actor>, quiero <capacidad>, para <beneficio>.
## Requisitos funcionales
- RF-001: <enunciado verificable, EARS si aplica>
- RF-002: …
## Requisitos no funcionales
- RNF-001: <restricción medible de rendimiento, seguridad, usabilidad, etc.>
## Estados
## Permisos
| Rol | Acción | Permitido |
## Errores
| Condición | Comportamiento observable del sistema |
## Casos límite
## Seguridad funcional
## Fuera de alcance
## Criterios de finalización
- [ ] <condición objetiva y comprobable>
## Decisiones pendientes
- [NECESITA ACLARACIÓN: …] (si las hay)

## Registro de cambios
| Versión | Fecha | Cambio |
| 1 | YYYY-MM-DD | Creación inicial |
```

## Artefactos de salida

- `specs/NNN-slug/spec.md` con la fila `Estado` en `BORRADOR` y la fila `Aprobación` en `PENDIENTE`.

## Validación

- Los RF tienen IDs estables y únicos en todo el repositorio.
- Cada RF es observable y verificable (pasa el test del paso 4) o lleva `[NECESITA ACLARACIÓN]`.
- No hay framework, ORM, librerías, estructura de clases, patrón interno ni tecnología de persistencia en la spec, salvo restricción de producto explícita del usuario (y entonces consta como restricción, no como diseño).
- Existen secciones `Fuera de alcance` y `Criterios de finalización`.
- Ninguna ambigüedad fue resuelta en silencio.

## Condiciones de parada

- Tras escribir y mostrar la spec: DETENTE. La aprobación llega más tarde, a través de `sdd-clarify`.

## Acciones prohibidas

- Diseñar arquitectura, componentes o modelos de datos físicos.
- Elegir tecnologías.
- Escribir código o tests.
- Resolver ambigüedades sin decidirlo el usuario.
- Aprobar la spec (esa gate no es de esta skill).

## Siguiente fase permitida

`sdd-clarify`
