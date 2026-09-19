---
name: sdd-migration
description: Revisa cambios de esquema de datos, APIs/contratos o formatos persistidos donde importan compatibilidad, datos existentes y rollback. Analiza productores/consumidores, estrategia de transición y riesgo; no implementa ni modifica plan, y enruta a sdd-plan si falta documentar la estrategia.
---

# sdd-migration — Gate de compatibilidad y transición

## Cuándo usar

- Cambios de DB, datos persistidos, APIs públicas, eventos, protocolos, formatos o configuración consumida por otras versiones/sistemas.

## Regla

- Si cambia comportamiento/requisito: primero `sdd-change` y aprobación.
- La estrategia final debe quedar registrada en `plan.md`; esta skill la revisa, no edita el plan.

## Procedimiento

1. Lee spec y plan vigentes, más código/contratos relevantes.
2. Identifica productores, consumidores, datos existentes y versiones coexistentes.
3. Clasifica: `COMPATIBLE`, `POR FASES` o `BREAKING`.
4. Revisa que el plan defina, cuando aplique:
   - estado inicial/final;
   - ventana de coexistencia;
   - expand/contract, backfill, dual-read/write, versionado o migración offline;
   - validación de datos e idempotencia;
   - rollback y fallo parcial;
   - tests de compatibilidad, históricos y rollback.
5. Si falta algo relevante, devuelve exactamente qué debe añadirse y enruta a `sdd-plan`.
6. Si la estrategia es suficiente, permite continuar a `sdd-trace`.

## Salida

```text
MIGRACIÓN: COMPATIBLE | POR FASES | BREAKING
AFECTA: <contratos/datos>
PLAN: SUFICIENTE | INCOMPLETO
RIESGOS: <breve>
SIGUIENTE: sdd-plan | sdd-trace
```

## Prohibido

- Modificar código, datos o `plan.md`.
- Ejecutar migraciones.
- Ocultar breaking changes o irreversibilidad.
