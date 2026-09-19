---
name: sdd-release
description: Gate final de pre-merge/pre-deploy. Verifica estado SDD, diff, tests, lint, typecheck, build, migraciones, dependencias, documentación y riesgos de release usando solo comandos existentes. No corrige problemas: emite READY o BLOCKED con evidencia.
---

# sdd-release — Check previo a merge/deploy

## Propósito

Evitar que una implementación técnicamente “terminada” llegue a release con checks, migraciones, docs o riesgos pendientes.

## Procedimiento

1. Confirma que las specs afectadas están validadas o identifica explícitamente por qué no aplica.
2. Revisa diff final y cambios fuera de scope.
3. Ejecuta checks reales aplicables: tests, lint, format check, typecheck, build/compile, análisis estático, auditoría de dependencias/seguridad.
4. Revisa migraciones pendientes y estrategia de rollback cuando existan.
5. Revisa variables/configuración/feature flags y secretos accidentales.
6. Confirma documentación afectada sincronizada (`sdd-doc-sync` si falta).
7. Revisa breaking changes/dependencias y notas operativas relevantes.
8. Emite veredicto; no arregla hallazgos dentro de esta skill.

## Salida

```text
RELEASE: READY | BLOCKED
CHECKS: <resumen>
MIGRACIONES: OK | N/A | BLOQUEO
DOCS: OK | N/A | BLOQUEO
RIESGOS: <ninguno o breve>
SIGUIENTE: <skill o release>
```

## Prohibido

- Hacer merge, deploy o release por iniciativa propia.
- Corregir código durante el gate.
- Marcar READY con checks relevantes fallando o sin evidencia.
