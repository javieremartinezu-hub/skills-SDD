---
name: sdd-tasks
description: >-
  Verifica la trazabilidad spec → plan (cada RF/RNF/UI cubierto, sin contradicciones ni componentes
  injustificados) y, si pasa, descompone el plan en tareas pequeñas y verificables en
  specs/NNN-slug/tasks.md (T-NNN con requisitos cubiertos, dependencias, tests y criterio "Hecho
  cuando"). Emite TRAZABILIDAD: PASS o FAIL. No implementa. Úsala tras sdd-plan (o sdd-migration), o
  cuando tasks.md esté caducado.
---

# sdd-tasks — Trazabilidad + descomposición

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

spec `APROBADA`; `plan.md` con `Spec-Version` actual; si el plan exige migración, `MIGRACIÓN: SUFICIENTE`. Si falta algo, bloquea hacia `sdd-plan` o `sdd-migration`.

## Procedimiento

1. **Trazabilidad.** Recorre todos los RF, RNF y UI de la spec contra la tabla de cobertura del plan, comprobando que la referencia citada realmente lo resuelve. Detecta además contradicciones spec↔plan y componentes del plan sin requisito (sobre-diseño).
   - Algún requisito sin cobertura real o alguna contradicción → escribe `tasks.md` solo con la cabecera, la sección de trazabilidad y `TRAZABILIDAD: FAIL`. `Siguiente paso: sdd-plan` (o `sdd-change` si el requisito está mal planteado). DETENTE.
2. **Descomposición.** Una tarea = un cambio pequeño + sus tests + un resultado verificable en una sola ejecución. Divide las tareas que agrupen varios comportamientos ("implementar backend", "todo el login").
3. Numera `T-001…`; una tarea solo depende de tareas anteriores.
4. Comprueba que cada RF/RNF/UI aparece en al menos una tarea.
5. **Re-edición tras `sdd-change`.** Actualiza la cabecera, reabre (`- [ ]`, borrando su evidencia) solo las tareas afectadas, añade las nuevas al final sin renumerar y re-sella el resto.
6. Escribe `tasks.md` con `TRAZABILIDAD: PASS`. `Siguiente paso: sdd-implement` (encadena todas las tareas). DETENTE.

## Plantilla de `tasks.md`

```markdown
# Tareas — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Plan-Version | M |

## Trazabilidad
| Requisito | Plan | Tareas |
|---|---|---|
| RF-001 | §4, D-002 | T-001, T-003 |
Sobre-diseño / contradicciones: ninguno | <lista>

## Tareas
- [ ] T-001 — <objetivo indivisible>
  - Cubre: RF-003, UI-002
  - Depende de: — | T-00X
  - Archivos esperados: <áreas según el plan>
  - Tests requeridos: <qué tests demuestran lo cubierto> | N/A — <motivo, solo config/docs>
  - Hecho cuando: <condiciones objetivas; para UI sin test automatizable: "confirmado por el usuario: <pasos>">
  - Evidencia: (la añade sdd-implement)

TRAZABILIDAD: PASS | FAIL
```

## Prohibido

Escribir código o tests, modificar spec o plan, crear tareas sin requisito o sin criterio objetivo, empezar la implementación.
