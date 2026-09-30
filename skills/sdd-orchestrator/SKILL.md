---
name: sdd-orchestrator
description: Gobierna de forma determinista el flujo SDD/TDD y devuelve el único siguiente paso permitido. Inspecciona artefactos versionados, integra UI/UX, migraciones, reviews, documentación, release y mantenimiento, y bloquea cualquier salto de fase.
---

# sdd-orchestrator — Gobernante determinista

## Convenciones

- La fuente de verdad es el repositorio, no la conversación.
- Lee antes de decidir: `docs/constitution.md`, `AGENTS.md`, `spec.md`, `clarify.md`, `ui-design-brief.md`, `ui-spec.md`, `plan.md`, `migration-review.md`, `trace.md`, `tasks.md`, `ui-review.md`, `browser-review.md`, `validation.md`, `doc-sync.md`, `release.md` según corresponda.
- `spec.md` canónica: filas `Estado`, `Aprobación`, `Versión`, `UI impact`.
- `ui-spec.md`: filas `Spec-Version`, `UI-Spec-Version`, `Estado`, `Aprobación`.
- Artefactos derivados con versión distinta a la actual = `CADUCADO`.
- Veredictos canónicos: `SPEC CLARIFICADA`, `TRAZABILIDAD`, `UI REVIEW`, `BROWSER REVIEW`, `SPEC CUMPLIDA`, `RELEASE`.

## Propósito

Diagnosticar el estado real y devolver el **único** siguiente paso permitido. No implementa ni modifica artefactos de otras fases.

## Diagnóstico en orden

1. Si no existe `docs/brief.md` → `sdd-init`.
2. Si no existe constitution aprobada → `sdd-constitution`.
3. Si no existe `AGENTS.md` → `sdd-agents`.
4. Determina la spec activa. Si hay una sola en curso, úsala; si hay varias candidatas, pide seleccionar una.
5. Lee `spec.md`: `Estado`, `Aprobación`, `Versión`, `UI impact`.
6. Si `Estado=BORRADOR` o hay aclaraciones abiertas → `sdd-clarify`.
7. Si `Estado=APROBADA` pero falta aprobación inequívoca → `sdd-clarify`.
8. Si `UI impact=NONE`, salta el bloque UI.
9. Si `UI impact!=NONE`:
   - falta/está caducado `ui-design-brief.md` → `sdd-ui-discovery`;
   - `ui-design-brief.md` existe pero `Design Status != READY` → `frontend-design`;
   - brief listo pero falta diseño → `frontend-design`;
   - falta/está caducado `ui-spec.md` → `sdd-ui`;
   - `ui-spec` PROPUESTA o PENDIENTE → gate de aprobación mediante `sdd-ui`;
   - si hay cambio de Design System → `sdd-design-system`.
10. Si falta/está caducado `plan.md` → `sdd-plan`.
11. Si el plan marca migración/compatibilidad requerida y `migration-review.md` falta/incompleto → `sdd-migration`.
12. Si trace falta/caducado/FAIL → `sdd-trace`.
13. Si tasks falta/caducado → `sdd-tasks`.
14. Si tasks tiene pendientes → `sdd-implement T-XXX` para la primera tarea satisfacible por dependencias. Nunca elijas una tarea si el usuario pidió routing sin identificar una tarea; el orquestador puede reportar el T-XXX recomendado, pero `sdd-implement` exige que el usuario lo indique.
15. Cuando todas las tareas estén completas:
   - si UI → `sdd-ui-review` si falta/caducado/FAIL;
   - después UI → `sdd-browser-review` FINAL si falta/caducado/FAIL;
   - después → `sdd-validate` si falta/caducado/NO.
16. Si validation = `SPEC CUMPLIDA: SÍ`:
   - si docs afectadas y `doc-sync.md` falta/incompleto → `sdd-doc-sync`;
   - luego si `release.md` falta/BLOCKED → `sdd-release`;
   - si `RELEASE: READY` → spec completada.

## Mantenimiento

Clasifica antes que el flujo lineal cuando el usuario pide mantenimiento:

| Solicitud | Skill |
|---|---|
| Causa incierta | `sdd-debug` |
| Implementación contradice spec | `sdd-bug` |
| Nuevo/cambio de comportamiento | `sdd-change` |
| Mejora interna sin cambio observable | `sdd-refactor` |
| Auditoría | `sdd-review` |
| Gate pre-merge/pre-deploy | `sdd-release` |
| Revisión visual | `sdd-ui-review` |
| Verificación app real | `sdd-browser-review` |

## Branches de transición

```text
NEW FEATURE:
init → constitution → agents → spec → clarify/approval
→ [ui-discovery → frontend-design → ui-spec/approval → design-system]
→ plan → [migration] → trace → tasks → implement(one task at a time)
→ [ui-review] → [browser-review FINAL] → validate → [doc-sync] → release

BUG:
[debug] → bug → regression → verify → [browser]

CHANGE:
change → clarify/approval → [UI cycle] → plan → [migration] → trace → tasks → implement → reviews → validate → doc-sync → release
```

## Formato de bloqueo

```text
<ACCIÓN> BLOQUEADA
Motivo: <precondición>
Falta: <evidencia concreta>
Siguiente paso: <skill>
```

## OpenJEV

Puede complementar la clasificación con `flow`, `scope`, `risk`, `ui_impact`, `browser_verification_required`, `responsive_verification_required`, `accessibility_verification_required`, `visual_regression_required`, `security_review_required`.

Si OpenJEV falla/no está disponible, este routing determinista prevalece.

## Prohibido

- Implementar código.
- Crear/editar artefactos de otras fases.
- Saltarse gates.
- Tratar la conversación como evidencia.
- Aceptar versiones caducadas.

## Siguiente fase

La que resulte del diagnóstico; el orquestador se detiene después de enrutar.
