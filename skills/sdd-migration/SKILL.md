---
name: sdd-migration
description: Evalúa compatibilidad y estrategia de transición para cambios de datos, APIs, eventos, formatos o configuración persistida. Persiste migration-review.md y enruta a plan/trace sin ejecutar migraciones ni editar código.
---

# sdd-migration — Migration & Compatibility Gate

## Cuándo usar

- Cambios de esquema, datos persistidos, APIs, eventos, protocolos, formatos o configuración consumida por varias versiones/sistemas.

## Precondiciones

- `spec.md` y `plan.md` vigentes.
- El plan declara `Migration Required: YES` cuando corresponde.

## Procedimiento

1. Lee spec, plan y contratos/código relevantes.
2. Identifica productores, consumidores, datos existentes y coexistencia de versiones.
3. Clasifica `COMPATIBLE`, `POR FASES` o `BREAKING`.
4. Verifica que el plan documente, cuando aplique: estado inicial/final, coexistencia, expand/contract, backfill, dual-read/write, versionado u offline migration, validación e idempotencia, rollback, fallo parcial y tests de compatibilidad/rollback.
5. Escribe `specs/NNN-slug/migration-review.md` con evidencia.
6. Si falta estrategia relevante: `MIGRATION PLAN: INCOMPLETE` → `sdd-plan`.
7. Si es suficiente: `MIGRATION PLAN: SUFFICIENT` → `sdd-trace`.

## Prohibido

- Ejecutar migraciones sobre datos reales.
- Modificar `plan.md`, código o requisitos.
- Ocultar breaking changes.
