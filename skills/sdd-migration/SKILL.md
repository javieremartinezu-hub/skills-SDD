---
name: sdd-migration
description: Gate de migración y compatibilidad. Revisa que plan.md tenga una estrategia de transición suficiente para cambios de esquema, datos persistidos, APIs, eventos, formatos o configuración consumida por varias versiones. Persiste migration-review.md con veredicto SUFICIENTE o INCOMPLETA. No ejecuta migraciones ni edita código o plan. Úsala cuando el plan declare Migración requerida: SÍ.
---

# sdd-migration — Gate de migración y compatibilidad

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

`spec.md` y `plan.md` vigentes, con `Migración requerida: SÍ` en el plan.

## Procedimiento

1. Lee la spec, la §10 del plan y los contratos o código afectados.
2. Identifica productores, consumidores, datos existentes y coexistencia de versiones. Confirma la clasificación: `COMPATIBLE`, `POR FASES` o `BREAKING`.
3. Comprueba que el plan cubra lo que aplique: estado inicial y final · coexistencia (expand/contract, dual-read/write, versionado) · backfill · validación e idempotencia · rollback y fallo parcial · tests de compatibilidad y rollback.
4. Escribe `migration-review.md`.
5. `MIGRACIÓN: INCOMPLETA` → `Siguiente paso: sdd-plan`. `MIGRACIÓN: SUFICIENTE` → `Siguiente paso: sdd-tasks`. DETENTE.

## Plantilla

```markdown
# Revisión de migración — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Plan-Version | M |
| Clasificación | COMPATIBLE / POR FASES / BREAKING |

| Aspecto | Cubierto en el plan | Falta |
|---|---|---|

MIGRACIÓN: SUFICIENTE | INCOMPLETA
```

## Prohibido

Ejecutar migraciones sobre datos reales, modificar plan, código o requisitos, ocultar breaking changes.
