---
name: sdd-review
description: Auditoría de solo lectura con dos modos: quality revisa calidad, mantenibilidad, seguridad, tests y deuda técnica; implementation compara spec/plan/tareas contra código y tests para detectar desviaciones, faltantes y drift. No modifica código. Entrega hallazgos priorizados y breves con evidencia.
---

# sdd-review — Revisión de código o implementación

## Modos
- `sdd-review quality`: calidad interna del código.
- `sdd-review implementation`: cumplimiento de spec/plan por la implementación.

## Principios
- SOLO audita; no corrige.
- Cada hallazgo debe citar evidencia concreta: archivo, símbolo, test o comando.
- Reporta primero problemas reales; no llenes la salida con preferencias estilísticas.
- Por defecto muestra solo hallazgos `CRÍTICO`, `ALTO` y `MEDIO`. `BAJO` solo si el usuario pide revisión exhaustiva.

## Quality — revisar
1. Correctitud evidente y manejo de errores.
2. Complejidad, duplicación y responsabilidades.
3. Legibilidad y mantenibilidad.
4. Acoplamiento y arquitectura accidental.
5. Seguridad relevante al código inspeccionado.
6. Tests: cobertura útil, fragilidad, exceso de mocks, casos límite.
7. Dependencias y código muerto.
8. Rendimiento solo donde exista riesgo concreto.

## Implementation — revisar
1. RF/RNF implementados vs spec.
2. Código incompatible con requisitos.
3. Plan/arquitectura vs implementación real.
4. Tareas marcadas completas sin evidencia suficiente.
5. RF sin tests o tests que no prueban comportamiento observable.
6. Comportamiento implementado sin RF asociado.
7. Drift entre spec, plan, tasks, código y tests.

## Verificaciones
Ejecuta checks existentes cuando aporten evidencia: tests, lint, typecheck, build, análisis estático o auditorías. No inventes comandos.

## Resultado
Clasifica cada hallazgo:
- `BUG` → `sdd-bug`
- `CAMBIO FUNCIONAL` → `sdd-change`
- `DEUDA/REFACTOR` → `sdd-refactor`
- `TAREA INCOMPLETA` → `sdd-implement T-XXX`

## Salida en conversación
Sé conciso. Máximo 10 hallazgos por defecto.
```text
REVISIÓN: QUALITY | IMPLEMENTATION
RESULTADO: PASS | WARN | FAIL
- [ALTO] archivo:línea — problema — impacto — siguiente paso
- [MEDIO] ...
CHECKS: <resumen>
SIGUIENTE PASO: <skill o ninguno>
```

## Artefacto opcional
Solo si el usuario pide persistir la auditoría: `reviews/YYYY-MM-DD-<modo>.md`.

## Acciones prohibidas
- Modificar código, spec, plan, tasks o tests.
- Crear hallazgos sin evidencia.
- Convertir preferencias personales en defectos.

## Revisión UI y browser

- `ui`: revisión estática contra UI Spec/Design System.
- `browser`: revisión de la aplicación renderizada mediante navegador.
- Si ambos aplican, ejecutar ambos; uno no sustituye al otro.
