---
name: sdd-refactor
description: Mejora estructura interna sin cambiar comportamiento observable. Define scope mínimo, protege baseline con tests, reutiliza patrones existentes, evita nuevas abstracciones innecesarias, aplica cambios pequeños y valida regresión con self-review.
---

# sdd-refactor — Mejora interna sin cambio funcional

## Procedimiento

1. Define objetivo y scope mínimo.
2. Busca patrones/helpers existentes antes de crear nuevas abstracciones.
3. Ejecuta baseline de tests; si falta cobertura, añade characterization tests mínimos.
4. Refactoriza en pasos pequeños. No cambies comportamiento ni contratos.
5. Evita capas, patrones y dependencias nuevas salvo necesidad demostrable.
6. Ejecuta tests/checks después de cambios relevantes. Si el refactor afecta un componente visual, interfaz o flujo web, revalida en navegador real (comportamiento observable intacto, consola sin errores) con evidencia visual antes de cerrar.
7. Self-review del diff: scope, compatibilidad, simplicidad, duplicación, contratos y tests.

## Desvíos

- Cambio funcional → `sdd-change`.
- Defecto funcional → `sdd-bug`.
- Diagnóstico incierto → `sdd-debug`.
- Cambio estructural contractual/migración → `sdd-migration` o `sdd-plan`.

## Salida

```text
REFACTOR: COMPLETADO | BLOQUEADO
CAMBIOS: <breve>
TESTS/CHECKS: <resultado>
SELF-REVIEW: PASS | hallazgo
SIGUIENTE: <skill o ninguna>
```
