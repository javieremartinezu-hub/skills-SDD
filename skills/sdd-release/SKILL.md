---
name: sdd-release
description: Gate final pre-merge/pre-deploy. Reutiliza la evidencia de sdd-validate si el commit no cambió, revisa diff y alcance, secretos, configuración, migración y rollback, sincroniza solo la documentación afectada y emite RELEASE: READY o BLOCKED en release.md. No hace merge ni deploy ni toca código. Úsala tras SPEC CUMPLIDA: SÍ o cuando el usuario pida un gate pre-merge.
---

# sdd-release — Gate de release

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

`validation.md` vigente con `SPEC CUMPLIDA: SÍ`; si el plan exige migración, `MIGRACIÓN: SUFICIENTE`.

## Procedimiento

1. **Reutilización.** Si el commit actual es el de `validation.md` y el árbol está limpio, reutiliza sus resultados. Si cambió algo, vuelve a ejecutar la suite completa y los checks de `AGENTS.md`.
2. **Diff y alcance.** Revisa el diff de la spec y marca los cambios fuera de alcance no justificados.
3. **Seguridad y configuración.** Secretos accidentales, variables, flags y configuración nueva documentada.
4. **Migración y rollback.** Confirma que lo previsto en el plan está implementado y es reversible.
5. **Documentación.** Localiza la documentación directamente afectada (README, docs de API, ejemplos, runbooks, configuración) y actualiza solo lo desactualizado. Si existen validadores de docs, ejecútalos. Es la única edición permitida en esta skill.
6. Escribe `release.md` y emite el veredicto. DETENTE.

**READY** solo si: checks con evidencia vigente, sin gates SDD pendientes, sin cambios fuera de alcance injustificados, migración y rollback resueltos cuando aplican, y documentación sincronizada o N/A.

## Plantilla de `release.md`

```markdown
# Release — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Plan-Version | M |
| Commit | <sha> |
| Evidencia | reutilizada de validation / re-ejecutada |

| Control | Evidencia | Resultado |
|---|---|---|
| Tests y checks | | |
| Diff / alcance | | |
| Secretos / configuración | | |
| Migración / rollback | | |
| Documentación | <archivos actualizados o N/A> | |

## Riesgos

RELEASE: READY | BLOCKED
```

## Prohibido

Merge, deploy o release; corregir código o tests; marcar READY con fallos relevantes; reescribir documentación ajena al cambio.
