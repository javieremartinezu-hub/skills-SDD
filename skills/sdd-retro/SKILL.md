---
name: sdd-retro
description: >-
  Documenta código existente sin spec como spec RETRO (comportamiento observable + tests de
  caracterización) sin tocar producción. Úsala antes de sdd-bug o sdd-change sobre ese código.
---

# sdd-retro — Spec retroactiva de código existente

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

Constitution `APROBADA` y `AGENTS.md` existente. Si una spec ya cubre el código, usa esa: `RETRO BLOQUEADA — Ya existe NNN-slug. Siguiente paso: sdd-bug | sdd-change`.

## Procedimiento

1. **Alcance mínimo:** el módulo o flujo que necesita el bug o cambio, no el repositorio entero.
2. Lee código y tests; extrae el comportamiento **observable** (entradas, salidas, estados, errores, permisos, interfaz existente) en EARS. Sin diseño interno.
3. **Anomalías** (comportamiento que parece defecto): regístralas como RF del comportamiento actual + `[NECESITA ACLARACIÓN: ¿defecto o intencional?]`, sin test de caracterización. Nunca las corrijas.
4. **Caracterización:** por cada RF/UI sin test que lo demuestre, el test mínimo que fija el comportamiento actual. Debe pasar sin tocar producción. Cita el test en el RF: `(Test: <ruta::nombre>)`.
5. Escribe `spec.md` con la plantilla, numeración y slug de `sdd-spec`, `Origen: RETRO`, `Versión: 1`; `Tamaño` y `Criterios de finalización`: `N/A (RETRO)`. La interfaz existente se documenta como UI-NNN.
6. Commit de spec + tests según `AGENTS.md`. `Siguiente paso: sdd-clarify`. DETENTE.

## Prohibido

Modificar código de producción, documentar intenciones en lugar del comportamiento real, ampliar el alcance más allá de lo necesario, resolver anomalías sin el usuario.
