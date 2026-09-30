---
name: sdd-release
description: Gate final pre-merge/pre-deploy. Revisa validación SDD, diff, checks del proyecto, migraciones, documentación, seguridad y evidencia UI/browser cuando aplique; persiste release.md y emite READY o BLOCKED sin corregir ni desplegar.
---

# sdd-release — Release Gate

## Precondiciones

- `spec.md` vigente; registrar su `Spec-Version`.
- `validation.md` vigente con `SPEC CUMPLIDA: SÍ`.
- UI gates, migration gate y doc-sync resueltos cuando apliquen.

## Procedimiento

1. Revisa estado SDD y versiones (`Spec-Version`, `Plan-Version` cuando aplique).
2. Revisa diff final y cambios fuera de scope.
3. Ejecuta checks existentes: tests, lint, typecheck, build, análisis estático, seguridad/dependencias.
4. Revisa migraciones, compatibilidad y rollback.
5. Revisa variables/configuración/flags y secretos accidentales.
6. Confirma documentación sincronizada o N/A.
7. Si hay UI, confirma `ui-review` y `browser-review FINAL` vigentes.
8. Escribe `specs/NNN-slug/release.md` con evidencia.
9. Emite `RELEASE: READY | BLOCKED`.
10. DETENTE. No hace merge/deploy.

## READY solo si

Todos los checks relevantes tienen evidencia en esta ejecución o una evidencia persistente vigente definida explícitamente por el proyecto; no existen gates SDD pendientes; no hay cambios fuera de scope no justificados; rollback y migraciones están resueltos cuando aplican.

## Prohibido

- Hacer merge, deploy o release.
- Corregir código durante el gate.
- Marcar READY con fallos relevantes.
