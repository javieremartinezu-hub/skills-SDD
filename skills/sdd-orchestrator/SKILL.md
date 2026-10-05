---
name: sdd-orchestrator
description: Router determinista del flujo SDD. Diagnostica el estado real leyendo solo cabeceras y veredictos de los artefactos y devuelve el único siguiente paso permitido; también clasifica pedidos de mantenimiento (bug, cambio, refactor, auditoría, release). Úsala ante cualquier duda sobre en qué fase está una spec o qué hacer a continuación.
---

# sdd-orchestrator — Router determinista

Aplica las Convenciones SDD de `AGENTS.md` (si aún no existe, sigue el diagnóstico igualmente).

## Propósito

Devolver el **único** siguiente paso permitido. No implementa ni edita artefactos.

## Lectura mínima

Para diagnosticar, lee de cada artefacto **solo la tabla de cabecera y la línea de veredicto**, nunca el cuerpo. Excepción: `tasks.md`, del que solo necesitas las líneas de estado `- [ ]` / `- [x]` y las dependencias de la primera pendiente.

## Mantenimiento (se clasifica antes que el flujo lineal)

| Pedido | Skill |
|---|---|
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
4. Determina la spec activa (la única con release no READY). Si hay varias, pide elegir.
5. spec `Estado` ≠ `APROBADA`, `Aprobación` pendiente, o `clarify.md` ausente/caducado/`NO` → `sdd-clarify`.
6. `plan.md` ausente o caducado → `sdd-plan`.
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

Implementar, editar artefactos, saltarse gates, aceptar derivados caducados o la conversación como evidencia.
