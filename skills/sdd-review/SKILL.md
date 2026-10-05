---
name: sdd-review
description: Auditoría de solo lectura con dos modos. quality revisa corrección, mantenibilidad, seguridad, tests y deuda técnica; implementation compara spec, plan y tareas contra código y tests para detectar faltantes, comportamiento sin requisito y drift. Entrega hallazgos priorizados con evidencia y la skill que debe resolverlos. Úsala cuando el usuario pida revisar, auditar o evaluar código o una implementación.
---

# sdd-review — Auditoría

Aplica las Convenciones SDD de `AGENTS.md`.

## Modos

- `quality`: corrección y manejo de errores · complejidad y duplicación · acoplamiento y arquitectura accidental · seguridad del código inspeccionado · tests (utilidad, fragilidad, mocks, casos límite) · dependencias y código muerto · rendimiento solo con riesgo concreto.
- `implementation`: RF/RNF/UI implementados vs spec · interfaz distinta de lo declarado · plan vs implementación real · tareas `- [x]` sin evidencia suficiente · requisitos sin test o con tests que no prueban comportamiento · comportamiento sin requisito · drift entre spec, plan, tasks, código y tests.

## Reglas

- Solo audita; no corrige.
- Cada hallazgo cita evidencia concreta (archivo:línea, símbolo, test o comando). Ejecuta los checks de `AGENTS.md` cuando aporten evidencia.
- Por defecto, solo `CRÍTICO`, `ALTO` y `MEDIO`, máximo 10. `BAJO` solo si se pide revisión exhaustiva.
- Preferencias de estilo no son defectos.

## Clasificación → siguiente paso

`BUG` → `sdd-bug` · `CAMBIO FUNCIONAL` → `sdd-change` · `DEUDA` → `sdd-refactor` · `TAREA INCOMPLETA` → `sdd-implement T-XXX`

## Salida

```text
REVISIÓN: QUALITY | IMPLEMENTATION · RESULTADO: PASS | WARN | FAIL
- [ALTO] archivo:línea — problema — impacto — siguiente paso
CHECKS: <resumen>
```

Persistir en `reviews/YYYY-MM-DD-<modo>.md` solo si el usuario lo pide.
