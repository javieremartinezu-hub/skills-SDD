---
name: sdd-orchestrator
description: Gobierna el flujo Spec-Driven Development (SDD). Diagnostica el estado del repositorio (constitution, AGENTS.md, specs, plan, trazabilidad, tasks, validación) y determina el único siguiente paso permitido. Bloquea cualquier acción fuera de fase e indica la skill correcta. Úsala cuando el usuario pregunte "qué sigue" o "en qué estado vamos", pida un diagnóstico del workflow SDD, o solicite especificar, planificar, implementar, validar o cambiar algo sin constancia de que las fases previas están completas.
---

# sdd-orchestrator — Gobernante del flujo SDD

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/spec.md` · `specs/NNN-slug/clarify.md` · `specs/NNN-slug/plan.md` · `specs/NNN-slug/trace.md` · `specs/NNN-slug/tasks.md` · `specs/NNN-slug/validation.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA` (solo por aprobación humana explícita).
- Marcador de incertidumbre funcional: `[NECESITA ACLARACIÓN: ...]`
- Veredictos canónicos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`
- Requisitos: `RF-001`, `RF-002`… Tareas: `T-001`, `T-002`…

## Propósito

Gobernar y explicar el workflow SDD: diagnosticar el estado real del repositorio y determinar el **único** siguiente paso permitido. Es el guardián de los gates: ninguna fase se ejecuta si sus precondiciones no están cumplidas.

## Alcance

- SOLO diagnostica, explica y enruta.
- NO implementa código. NO crea ni edita artefactos de otras fases (no escribe specs, planes, tasks ni código).
- Puede leer, listar y buscar en el repositorio sin restricción.

## Cuándo usar

- "¿Qué sigue?" / "¿En qué estado está el proyecto?"
- El usuario pide una acción (implementar, planificar, validar, cambiar) y no consta que las fases previas estén completas.
- Antes de invocar cualquier otra skill SDD, si hay duda de que sea la fase correcta.

## Precondiciones

Ninguna. Funciona en cualquier repositorio, esté o no inicializado.

## Contexto requerido

- Acceso de lectura al repositorio actual.

## Entradas

- La petición del usuario (acción deseada o pregunta de estado).

## Procedimiento

1. **Diagnóstico.** Comprueba, en este orden, leyendo el disco (no la memoria):
   1. `docs/constitution.md` existe y contiene `Estado: APROBADA`.
   2. `AGENTS.md` existe en la raíz.
   3. `specs/` contiene carpetas `NNN-slug/`. Identifica la **spec activa**: la de `Estado: APROBADA` sin `validation.md` con veredicto SÍ, o la modificada más recientemente. Si hay más de una candidata, pregunta al usuario cuál es la activa.
   4. Spec activa: contiene `[NECESITA ACLARACIÓN` → cuenta cuántos.
   5. `clarify.md` de la spec activa existe y termina con `SPEC CLARIFICADA: SÍ`.
   6. Campo `Aprobación` de spec.md: `APROBADA` o `PENDIENTE`.
   7. `plan.md` existe.
   8. `trace.md` existe y contiene `TRAZABILIDAD: PASS`.
   9. `tasks.md` existe; cuenta tareas totales y marcadas `- [x]`.
   10. `validation.md` existe y contiene `SPEC CUMPLIDA: SÍ`.
2. **Determina la fase** con la tabla de transiciones y elige el único siguiente paso válido.
3. **Resuelve la petición:**
   - Si la acción pedida coincide con el paso permitido → confirma que es la fase correcta y nombra la skill a invocar (no la ejecutes tú).
   - Si no coincide → **bloquea** con el formato de bloqueo y no intentes sortear el gate por ninguna vía.

### Tabla de transiciones

| Estado observado | Siguiente paso permitido |
|---|---|
| Sin `docs/constitution.md` aprobada y sin `docs/brief.md` | `sdd-init` |
| Sin constitution aprobada, con brief | `sdd-constitution` |
| Constitution aprobada, sin `AGENTS.md` | `sdd-agents` |
| Con AGENTS.md, sin spec en curso | `sdd-spec` |
| Spec `BORRADOR` | `sdd-clarify` |
| Spec `CLARIFICADA` con `Aprobación: PENDIENTE` | Gate de aprobación (lo conduce `sdd-clarify`) |
| Spec `APROBADA` sin `plan.md` | `sdd-plan` |
| `plan.md` sin `trace.md: PASS` | `sdd-trace` |
| `trace.md: PASS` sin `tasks.md` | `sdd-tasks` |
| `tasks.md` con tareas pendientes | `sdd-implement T-XXX` (la siguiente por dependencias) |
| Todas las tareas `- [x]` sin `SPEC CUMPLIDA: SÍ` | `sdd-validate` |
| `SPEC CUMPLIDA: SÍ` | Spec completada. Nuevo requisito → `sdd-change` |
| Cambio funcional solicitado (en cualquier fase posterior a existir una spec) | `sdd-change` |

### Formato de bloqueo

```
<ACCIÓN> BLOQUEADA

Motivo: <qué precondición no se cumple>
Falta: <lista concreta de lo que falta>
Siguiente paso: <skill>
```

Ejemplo:

```
IMPLEMENTACIÓN BLOQUEADA

Motivo: No existe una especificación aprobada.
Falta: specs/001-…/spec.md con Estado: APROBADA.
Siguiente paso: sdd-spec
```

## Artefactos de salida

Ninguno (solo la respuesta de diagnóstico en la conversación).

## Validación

- Cada afirmación del diagnóstico proviene de un archivo leído en esta ejecución.
- El siguiente paso propuesto aparece en la tabla de transiciones.

## Condiciones de parada

- Tras entregar el diagnóstico + veredicto o bloqueo: DETENTE. No ejecutes la fase siguiente.

## Acciones prohibidas

- Implementar o modificar código.
- Crear, editar o "rellenar huecos" de spec, plan, tasks, constitution o AGENTS.md.
- Saltarte, suavizar o reinterpretar cualquier gate.
- Aceptar "ya lo hicimos antes en la conversación" como sustituto de leer los artefactos.

## Siguiente fase permitida

Ninguna: esta skill enruta hacia la skill correcta pero no la ejecuta.
