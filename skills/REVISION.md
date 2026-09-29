# SDD + TDD — Integración para programación con IA

## Flujos

- Nuevo desarrollo: `spec → clarify → plan → trace → tasks → implement → validate`
- Bug confirmado: `sdd-bug → reproduce → RED → root cause → fix → regression → self-review`
- Diagnóstico incierto: `sdd-debug → evidence → hypotheses → root cause → route`
- Cambio funcional: `sdd-change → spec delta → plan delta/completo → trace → tasks → implement → validate`
- Migración: `sdd-migration → compatibility/rollback → plan/tasks → implement → validate`
- Refactor: `baseline → small refactor → regression → self-review`
- Auditoría: `sdd-review quality|implementation|security|dependencies`
- Documentación: `sdd-doc-sync`
- Pre-release: `sdd-release`

## Guardrails añadidos

- **Scope guard:** limitar archivos/componentes afectados y justificar expansión.
- **Reuse-first:** buscar helpers/patrones/componentes existentes antes de crear nuevos.
- **Simplicidad:** solución mínima; sin capas/abstracciones “por si acaso”.
- **Dependency guard:** justificar necesidad, alternativa, mantenimiento y seguridad.
- **Self-review:** revisar diff, scope, contratos, duplicación, casos borde y calidad de tests antes de cerrar.
- **Verificación en navegador (UI):** todo componente visual, interfaz o flujo web creado, modificado o corregido se valida en navegador real antes de cerrar: inspección con herramienta de automatización, prueba funcional E2E sin errores de renderizado/consola (JavaScript), evidencia visual (captura o DOM) e iteración hasta pasar.
- **Compatibilidad:** preservar contratos y datos salvo cambio explícito.
- **Docs sync:** actualizar solo documentación afectada.
- **Concisión:** respuestas de estado breves; los artefactos pueden contener el detalle técnico.
