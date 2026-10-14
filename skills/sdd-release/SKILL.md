---
name: sdd-release
description: >-
  Gate READY/BLOCKED antes de merge o deploy: evidencia vigente, alcance del diff, secretos,
  migración, docs y resumen de PR. No hace merge ni deploy.
---

# sdd-release — Gate de release

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

`validation.md` vigente con `SPEC CUMPLIDA: SÍ`; si el plan exige migración, `MIGRACIÓN: SUFICIENTE`.

## Procedimiento

1. **Reutilización.** Un resultado de CI verde sobre el mismo commit cuenta como evidencia. Si el commit actual es el de `validation.md` y el árbol está limpio, reutiliza sus resultados. Si cambió algo, vuelve a ejecutar la suite completa y los checks de `AGENTS.md`.
2. **Diff y alcance.** Revisa el diff de la spec y marca los cambios fuera de alcance no justificados.
3. **Seguridad y configuración.** Secretos accidentales, variables, flags y configuración nueva documentada. Check de seguridad según el umbral de `AGENTS.md`: fallo → BLOCKED, salvo alta/crítica sin parche: pregunta si aceptas el riesgo y, con un sí inequívoco, añádela a "Excepciones aceptadas" de `AGENTS.md`. Excepciones con parche ya disponible → BLOCKED. `no disponible` o hallazgos no bloqueantes → Riesgos.
4. **Migración y rollback.** Confirma que lo previsto en el plan está implementado y es reversible.
5. **Documentación.** Localiza la documentación directamente afectada (README, docs de API, ejemplos, runbooks, configuración) y actualiza solo lo desactualizado. Si existen validadores de docs, ejecútalos. Junto con "Excepciones aceptadas", es la única edición permitida en esta skill.
6. Escribe `release.md`, emite el veredicto y commitea los artefactos SDD pendientes (`docs(NNN-slug): artefactos SDD`). DETENTE.

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

## Resumen para PR (con ramas por spec)
Qué cambia · RF cubiertos · riesgos · cómo probar.

RELEASE: READY | BLOCKED
```

## Prohibido

Merge, deploy o release; corregir código o tests; marcar READY con fallos relevantes; reescribir documentación ajena al cambio.
