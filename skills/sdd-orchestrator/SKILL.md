---
name: sdd-orchestrator
description: Gobierna el flujo Spec-Driven Development (SDD). Diagnostica el estado del repositorio (constitution, AGENTS.md, specs, plan, trazabilidad, tasks, validación) y determina el único siguiente paso permitido. Bloquea cualquier acción fuera de fase e indica la skill correcta. Úsala cuando el usuario pregunte "qué sigue" o "en qué estado vamos", pida un diagnóstico del workflow SDD, o solicite especificar, planificar, implementar, validar o cambiar algo sin constancia de que las fases previas están completas.
---

# sdd-orchestrator — Gobernante del flujo SDD

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/spec.md` · `specs/NNN-slug/clarify.md` · `specs/NNN-slug/ui-spec.md` (si hay UI) · `design/` (si existe) · `specs/NNN-slug/plan.md` · `specs/NNN-slug/trace.md` · `specs/NNN-slug/tasks.md` · `specs/NNN-slug/browser-review.md` (si hay UI) · `specs/NNN-slug/validation.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA` (solo por aprobación humana explícita).
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta las filas `Estado`, `Aprobación` y `Versión`; nunca busques `Estado: …` como texto libre.
- Cada artefacto derivado debe declarar su vínculo de versión: `plan.md` incluye `Spec-Version` y `Plan-Version`; `trace.md`, `tasks.md` y `validation.md` incluyen ambos. Un vínculo distinto al valor actual es **caducado**, aunque el archivo o su veredicto exista.
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
   3. `specs/` contiene carpetas `NNN-slug/`. Identifica la **spec activa**: la que tenga la fila `Estado` en `APROBADA` sin `validation.md` vigente con veredicto `SPEC CUMPLIDA: SÍ`, o la modificada más recientemente. Si hay más de una candidata, pregunta al usuario cuál es la activa.
   4. Lee las filas `Estado`, `Aprobación` y `Versión` de la spec activa; cuenta sus marcadores `[NECESITA ACLARACIÓN`. Determina además si la spec tiene impacto UI directo o indirecto.
   5. `clarify.md` de la spec activa existe y contiene el veredicto final `SPEC CLARIFICADA: SÍ`.
6. Si `ui_impact != none`, `ui-spec.md` debe existir y estar alineada con la versión de spec; si no existe, el siguiente paso es `sdd-ui`.
7. Si hay Design System y la UI lo afecta, verificar coherencia con `design/`.
   8. `plan.md` existe y su `Spec-Version` coincide con la fila `Versión`; registra su `Plan-Version`.
   9. `trace.md` contiene `TRAZABILIDAD: PASS` y sus `Spec-Version` y `Plan-Version` coinciden con los actuales.
   10. `tasks.md` existe, sus versiones coinciden con las actuales y cuenta tareas totales y marcadas `- [x]`.
   11. `validation.md` contiene `SPEC CUMPLIDA: SÍ` y sus versiones coinciden con las actuales.
   12. Marca expresamente como `CADUCADO` todo artefacto que no tenga cabecera de versión o no coincida; su contenido no supera ningún gate.
2. **Clasifica primero solicitudes de mantenimiento** cuando ya exista implementación:
   - Error respecto de comportamiento esperado → `sdd-bug`.
   - Cambio de comportamiento esperado → `sdd-change`.
   - Revisión de calidad/deuda → `sdd-review quality`.
   - Revisión de cumplimiento spec/plan → `sdd-review implementation`.
   - Mejora interna sin cambio observable → `sdd-refactor`.
   Estas ramas no deben obligar a reiniciar el ciclo lineal si su propia skill no lo requiere.
3. Para desarrollo normal, determina la fase con la tabla de transiciones y elige el único siguiente paso válido.
4. **Resuelve la petición:**
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
| Spec `CLARIFICADA` con fila `Aprobación` en `PENDIENTE` | Gate de aprobación (lo conduce `sdd-clarify`) |
| Spec `APROBADA` con impacto UI y sin `ui-spec.md` | `sdd-ui` |
| Spec `APROBADA` sin plan vigente para su `Versión` | `sdd-plan` |
| Plan vigente sin trazabilidad `PASS` vigente | `sdd-trace` |
| Trazabilidad `PASS` vigente sin tareas vigentes | `sdd-tasks` |
| `tasks.md` vigente con tareas pendientes | `sdd-implement T-XXX` (la siguiente por dependencias) |
| Todas las tareas vigentes `- [x]`, con UI pendiente de browser verification | `sdd-browser-review` |
| Todas las tareas vigentes `- [x]` sin validación vigente `SPEC CUMPLIDA: SÍ` | `sdd-validate` |
| `SPEC CUMPLIDA: SÍ` vigente | Spec completada. Nuevo requisito → `sdd-change` |
| Bug: implementación contradice comportamiento definido | `sdd-bug` |
| Cambio funcional solicitado | `sdd-change` |
| Revisar calidad del código | `sdd-review quality` |
| Revisar implementación contra spec/plan | `sdd-review implementation` |
| Refactor sin cambio funcional | `sdd-refactor` |
| Revisión visual/UI | `sdd-ui-review` |
| Verificación de interfaz ejecutándose | `sdd-browser-review` |

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
Falta: specs/001-…/spec.md con la fila Estado en APROBADA.
Siguiente paso: sdd-spec
```

## Artefactos de salida

Ninguno (solo la respuesta de diagnóstico en la conversación).

## Validación

- Cada afirmación del diagnóstico proviene de un archivo leído en esta ejecución.
- El siguiente paso propuesto aparece en la tabla de transiciones.

## Condiciones de parada

- Tras entregar el diagnóstico + veredicto o bloqueo: DETENTE. No ejecutes la fase siguiente.
- Respuesta breve por defecto: estado, motivo y siguiente skill. No expliques el framework salvo que se solicite.

## Acciones prohibidas

- Implementar o modificar código.
- Crear, editar o "rellenar huecos" de spec, plan, tasks, constitution o AGENTS.md.
- Saltarte, suavizar o reinterpretar cualquier gate.
- Aceptar "ya lo hicimos antes en la conversación" como sustituto de leer los artefactos.

## Siguiente fase permitida

Ninguna: esta skill enruta hacia la skill correcta pero no la ejecuta.
