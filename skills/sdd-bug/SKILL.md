---
name: sdd-bug
description: Diagnostica y corrige defectos donde la implementación no cumple una spec aprobada. Si la causa es incierta, primero diagnostica por evidencia e hipótesis; luego reproduce, crea un test de regresión que falle, corrige la causa raíz con el cambio mínimo y verifica. Si el problema resulta ser un cambio de requisito, configuración o entorno, detiene y enruta. Úsala ante cualquier bug, error, fallo o comportamiento inesperado.
---

# sdd-bug — Diagnóstico y corrección de defectos

Aplica las Convenciones SDD de `AGENTS.md`.

## Regla clave

- Código ≠ spec → **bug**: se corrige código y tests.
- El usuario quiere otro comportamiento, o la spec es ambigua → `sdd-change`.

## Fase 0 — Diagnóstico (solo si la causa es incierta)

1. Define síntoma, esperado y alcance conocido; recoge la evidencia mínima (logs, errores, entradas, entorno).
2. Formula pocas hipótesis ordenadas por evidencia y diseña el experimento más barato que las discrimine. Sin cambios especulativos en producción.
3. Con causa confirmada, enruta:
   - Defecto → sigue a la Fase 1.
   - Cambio de requisito → `sdd-change`.
   - Configuración, datos o entorno externo → informa la acción concreta y DETENTE.
   - Sin evidencia suficiente → `DEBUG: INCONCLUSO` con los próximos experimentos, y DETENTE.

## Fase 1 — Corrección

1. Identifica spec y RF/UI afectados. Si no se puede saber qué se esperaba: `BUG BLOQUEADO — Siguiente paso: sdd-change`.
2. **RED:** test de regresión mínimo que falla por el bug (en fallos visuales, con la herramienta de tests de UI que tenga el proyecto; si no hay, registra los pasos de reproducción y pide confirmación al usuario).
3. Corrige la causa raíz, no el síntoma, con el cambio mínimo (**GREEN**). **REFACTOR** solo si mejora claramente.
4. Verifica con los comandos de `AGENTS.md`, proporcional al riesgo; amplía si el cambio toca código compartido.
5. Registra `specs/NNN-slug/bugs/BUG-NNN.md` y ciérralo solo con evidencia.
6. Si el proyecto usa git, haz un commit con el fix, el test y `BUG-NNN.md`, según la sección Commits de `AGENTS.md`.

```markdown
# BUG-NNN — <título>
- Spec/Req: RF-XXX | UI-XXX
- Síntoma / reproducción:
- Causa raíz:
- Test de regresión:
- Fix (archivos):
- Verificación (comando → resultado):
- Estado: ABIERTO | CERRADO
```

## Salida (máx. 6 líneas)

```text
BUG: BUG-NNN — <título> · REQ: RF-XXX
CAUSA: … · FIX: …
TESTS: <comando> → PASS/FAIL
RESULTADO: CORREGIDO | BLOQUEADO — siguiente paso: <skill>
```

## Prohibido

Cambiar la spec para que coincida con código defectuoso, añadir comportamiento, refactorizar áreas ajenas, cerrar sin test de regresión cuando sea razonablemente posible.
