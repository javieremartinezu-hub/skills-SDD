---
name: sdd-refactor
description: Mejora la calidad interna sin cambiar el comportamiento observable (simplificación, duplicación, nombres, cohesión, separación de responsabilidades, deuda técnica). Protege el comportamiento con tests existentes o de caracterización, aplica cambios pequeños y verifica regresión. Si requiere cambiar comportamiento o contratos, detiene y enruta. Úsala para refactors y limpieza de código.
---

# sdd-refactor — Mejora interna sin cambio funcional

Aplica las Convenciones SDD de `AGENTS.md`.

## Regla clave

Un refactor cambia **cómo** está construido el código, no **qué** hace, ni cómo se ve la interfaz.

## Procedimiento

1. Define el objetivo y el alcance mínimo.
2. Ejecuta los tests actuales: baseline en verde.
3. Si falta protección relevante, añade primero tests de caracterización mínimos.
4. Aplica cambios pequeños sin capacidades nuevas; ejecuta tests tras cada cambio relevante.
5. Verifica con los comandos de `AGENTS.md`, proporcional al riesgo.
6. Revisa el diff para confirmar que no hay cambios funcionales ni visuales accidentales.
7. Si el proyecto usa git, haz un commit según la sección Commits de `AGENTS.md`.

## Desvíos

Cambia comportamiento o interfaz → `sdd-change` · descubre un defecto → `sdd-bug` · requiere decisión arquitectónica o contrato nuevo → `sdd-change`.

## Salida

```text
REFACTOR: <objetivo> · ARCHIVOS: <resumen>
TESTS: <comando> → PASS/FAIL · CHECKS: <resumen>
RESULTADO: COMPLETADO | BLOQUEADO — <siguiente paso>
```
