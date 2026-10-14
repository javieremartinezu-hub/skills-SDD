---
name: sdd-orchestrator
description: >-
  Router del flujo SDD: diagnostica el estado por cabeceras y veredictos, ejecuta el siguiente paso
  y encadena fases hasta el próximo gate humano; clasifica bugs, cambios, refactors, auditorías y
  releases. Úsala para avanzar una spec o ver su estado ("sdd-orchestrator estado" solo
  diagnostica).
---

# sdd-orchestrator — Router determinista

Aplica las Convenciones SDD de `AGENTS.md` (si aún no existe, sigue el diagnóstico igualmente).

## Propósito

Determinar el **único** siguiente paso permitido y ejecutarlo invocando su skill. No implementa ni edita artefactos por sí mismo.

## Modo continuo (por defecto)

Tras cada skill, vuelve a diagnosticar y ejecuta el siguiente paso sin preguntar. Detente solo ante: gate de aprobación o pregunta pendiente al usuario · `BLOQUEADA` · veredicto negativo (`NO`, `FAIL`, `INCOMPLETA`, `BLOCKED`), sin reintentar · condición de parada de `sdd-implement` · varias specs activas · `RELEASE: READY` · petición del usuario. Con `sdd-orchestrator estado` solo diagnostica y devuelve la Salida. Si el harness lo permite, ejecuta cada skill en una sesión o subagente nuevo: el estado vive en los artefactos y git.

## Lectura mínima

Para diagnosticar, lee de cada artefacto **solo la tabla de cabecera y la línea de veredicto**, nunca el cuerpo. Excepción: `tasks.md`, del que solo necesitas las líneas de estado `- [ ]` / `- [x]` y las dependencias de la primera pendiente; y en specs RETRO completadas, solo la búsqueda de la marca `(defecto)`.

## Mantenimiento (se clasifica antes que el flujo lineal)

| Pedido | Skill |
|---|---|
| Bug o cambio en código que ninguna spec cubre | `sdd-retro` (después, `sdd-bug` / `sdd-change`) |
| Defecto o fallo de causa incierta | `sdd-bug` |
| Comportamiento nuevo o distinto en una spec existente | `sdd-change` |
| Mejora interna sin cambio observable | `sdd-refactor` |
| Auditoría de calidad o de cumplimiento | `sdd-review` |
| Gate pre-merge/pre-deploy | `sdd-release` |

Funcionalidad nueva sin spec → flujo lineal desde `sdd-spec`.

## Diagnóstico en orden

1. Falta `docs/brief.md` → `sdd-init`.
2. Constitution ausente o no `APROBADA` → `sdd-constitution`.
3. Falta `AGENTS.md` → `sdd-agents`.
4. Spec activa: con ramas por spec, la de la rama actual; si no, la única no completada. Completada = `RELEASE: READY`, o `Origen: RETRO` v1 `APROBADA`. Si hay varias, pide elegir. RETRO completada con RF `(defecto)` → `sdd-bug`. Sin spec activa: funcionalidad nueva descrita por el usuario → `sdd-spec`; si no, pregunta qué hacer y DETENTE.
5. spec `Estado` ≠ `APROBADA`, `Aprobación` pendiente, o `clarify.md` ausente/caducado/`NO` → `sdd-clarify`.
6. `plan.md` ausente, caducado o con `Aprobación: PENDIENTE` → `sdd-plan`.
7. Plan con `Migración requerida: SÍ` y `migration-review.md` ausente/caducado/`INCOMPLETA` → `sdd-migration`.
8. `tasks.md` ausente o caducado → `sdd-tasks`. Con `TRAZABILIDAD: FAIL` → `sdd-plan`.
9. Tareas pendientes → `sdd-implement` (modo continuo: encadena todas las pendientes hasta terminar o hasta una condición de parada).
10. Todas completas y `validation.md` ausente/caducado/`NO` → `sdd-validate`.
11. `SPEC CUMPLIDA: SÍ` y `release.md` ausente/caducado/`BLOCKED` → `sdd-release`.
12. `RELEASE: READY` → spec completada.

## Tamaño de la spec (fila `Tamaño`)

- `S`: ≤3 RF, sin migración, sin cambios de permisos/seguridad ni dependencias nuevas. Plan en modo breve, 1-3 tareas. El resto de gates se mantiene.
- `M` / `L`: flujo completo.

## Salida

```text
ESTADO: <spec> v<N> · fase actual
SIGUIENTE PASO: <skill> [T-XXX]
MOTIVO: <precondición que lo determina>
```

## Prohibido

Implementar o editar artefactos por sí mismo, encadenar tras una parada, saltarse gates, aceptar derivados caducados o la conversación como evidencia.
