# Revisión v5 — SDD + TDD + UI/UX integrado

## Estado
Versión final consolidada y autocontenida del framework.

## Cambios incorporados

- V4 adoptado como base sin el V2 anidado de V3.
- `frontend-design` y `sdd-ui-discovery` integrados formalmente en el flujo SDD.
- Todas las skills tienen frontmatter `name` + `description`.
- `sdd-orchestrator` incorpora rutas para debug, migration, doc-sync, release y UI.
- `ui-spec.md` tiene versionado y gate de aprobación cuando corresponde.
- `ui-design-brief.md` persiste el descubrimiento UI antes de formalizar `ui-spec.md`.
- `ui-review.md`, `browser-review.md`, `migration-review.md`, `doc-sync.md` y `release.md` son artefactos persistentes de evidencia.
- Browser Review final obligatorio para UI, separado del control opcional por tarea.
- Se elimina el posible deadlock del Browser Review: una revisión por tarea verifica solo el alcance implementado; la revisión final verifica la feature completa.
- `sdd-plan` incluye estrategia de migración y estrategia de verificación UI/browser.
- `sdd-trace` contempla `UI-XXX → plan → task → componente → browser evidence`.
- `sdd-validate` integra UI Review, Browser Review, documentación y migración cuando aplican.
- `sdd-release` queda conectado después de la validación final.

## Flujo principal

`init → constitution → agents → spec → clarify+approval → [ui-discovery → frontend-design → ui-spec+approval → design-system] → plan → migration(if needed) → trace → tasks → implement/TDD → [task browser check if UI] → ui-review → browser-review-final → validate → doc-sync(if needed) → release`

## Principio clave

La spec funcional y, cuando existe interfaz, la UI Spec son contratos persistentes. El plan, las tareas, el código, los tests y la evidencia visual deben permanecer vinculados por versión y trazabilidad.
