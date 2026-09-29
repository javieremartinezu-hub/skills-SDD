# Revisión v2 — SDD + TDD

## Cambios incorporados

- UI/UX convertido en artefacto de primera clase mediante `sdd-ui`.
- Design System persistente mediante `sdd-design-system`.
- Auditoría estática mediante `sdd-ui-review`.
- Verificación de la aplicación real mediante navegador con `sdd-browser-review`.
- Browser verification integrada en implement, orchestrator, validate, bug, change y refactor.
- Evidencia por URL, viewport, pasos, esperado, observado y screenshots.
- Responsive, accessibility, estados y motion incluidos en el contrato UI.
- OpenJEV decision gate mantiene clasificación estructurada sin reemplazar SDD ni el modelo principal.

## Flujo actualizado

`spec → clarify → ui-spec(if UI) → design-system(if needed) → plan → trace → tasks → implement/TDD → browser-review(if UI) → self-review → validate`

## Principio clave

La implementación debe cumplir tanto el comportamiento especificado como la experiencia visible especificada. El código no es evidencia suficiente para declarar una UI terminada.
