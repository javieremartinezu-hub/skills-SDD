---
name: sdd-review
description: Auditoría de solo lectura con modos quality, implementation, security y dependencies. Entrega hallazgos priorizados con evidencia y enruta a bug/change/refactor/migration/implement según corresponda. No modifica código.
---

# sdd-review — Auditorías de código e implementación

## Modos

- `sdd-review quality`: correctitud, mantenibilidad, complejidad, duplicación, tests, deuda.
- `sdd-review implementation`: spec/plan/tasks frente a código/tests.
- `sdd-review security`: auth, permisos, inputs, secretos, exposición de datos, inyecciones, configuraciones y superficies relevantes.
- `sdd-review dependencies`: necesidad, duplicación funcional, mantenimiento, versión, vulnerabilidades conocidas por herramientas disponibles, licencia/configuración y riesgo de actualización.

## Principios

- SOLO audita; no corrige.
- Cada hallazgo requiere evidencia concreta.
- Prioriza defectos reales sobre preferencias estilísticas.
- Máximo 10 hallazgos; por defecto muestra `CRÍTICO`, `ALTO`, `MEDIO`.
- Usa checks reales existentes; nunca inventes comandos ni vulnerabilidades.

## Quality

Revisa correctitud, errores, complejidad, duplicación, cohesión, acoplamiento, código muerto, tests, rendimiento con riesgo concreto y sobreingeniería.

## Implementation

Verifica RF/RNF, arquitectura/plan, tasks/evidencia, cobertura de tests, comportamiento sin RF y drift entre artefactos/código.

## Security

Revisa solo superficies aplicables: autenticación/autorización, validación de entrada, inyección, secretos, exposición de datos, manejo de sesiones/tokens, dependencias, configuración insegura, logging sensible y privilegios. Distingue evidencia de hipótesis.

## Dependencies

Por dependencia agregada/actualizada revisa:
1. necesidad concreta;
2. si el proyecto ya tiene alternativa equivalente;
3. mantenimiento/compatibilidad;
4. auditorías de vulnerabilidades disponibles en el proyecto;
5. impacto de bundle/runtime/operación cuando aplique;
6. licencia solo si existen datos/herramientas confiables disponibles;
7. versión/pinning y estrategia de actualización.

## Clasificación de salida

- `BUG` → `sdd-bug`
- `CAMBIO FUNCIONAL` → `sdd-change`
- `MIGRACIÓN` → `sdd-migration`
- `DEUDA/REFACTOR` → `sdd-refactor`
- `TAREA INCOMPLETA` → `sdd-implement T-XXX`

## Salida

```text
REVISIÓN: QUALITY | IMPLEMENTATION | SECURITY | DEPENDENCIES
RESULTADO: PASS | WARN | FAIL
- [ALTO] evidencia — problema — impacto — siguiente paso
- [MEDIO] ...
CHECKS: <resumen>
SIGUIENTE: <skill o ninguno>
```

## Artefacto opcional

Solo si el usuario lo pide: `reviews/YYYY-MM-DD-<modo>.md`.
