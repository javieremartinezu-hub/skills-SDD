---
name: sdd-bug
description: Corrige un defecto confirmado contra comportamiento esperado. Limita el scope, reproduce, crea test de regresión, encuentra causa raíz, aplica fix mínimo, ejecuta regresión y self-review. No cambia la spec para justificar el bug.
---

# sdd-bug — Corrección de defectos

## Regla

- `código != spec` → BUG.
- Si debe cambiar el comportamiento esperado → `sdd-change`.
- Si aún no sabes si es bug, causa o alcance → `sdd-debug`.

## Procedimiento

1. Identifica spec/RF y comportamiento esperado.
2. Define scope inicial y revisa implementaciones existentes relacionadas.
3. Reproduce el fallo con el caso mínimo.
4. **RED:** crea test de regresión que falle por el defecto, cuando sea razonablemente posible.
5. Determina la causa raíz con evidencia; evita parchear síntomas.
6. **GREEN:** aplica el fix mínimo. No agregues funcionalidades ni refactors ajenos.
7. Ejecuta tests afectados y regresión proporcional al riesgo.
8. **Self-review:** confirma scope, ausencia de extras, compatibilidad, test útil y ausencia de duplicación/abstracciones innecesarias.
9. Registra `specs/NNN-slug/bugs/BUG-NNN.md` y cierra solo con evidencia.

## Plantilla

```markdown
# BUG-NNN — <título>
- Spec/RF: RF-XXX
- Síntoma: <observable>
- Reproducción: <mínima>
- Causa raíz: <evidencia>
- Scope: <áreas afectadas>
- Test de regresión: <archivo/comando>
- Fix: <archivos>
- Verificación: <comandos → resultado>
- Estado: ABIERTO | CERRADO
```

## Salida

```text
BUG: BUG-NNN — CORREGIDO | BLOQUEADO
CAUSA: <breve>
FIX: <breve>
TESTS: <resultado>
SELF-REVIEW: PASS | hallazgo
SIGUIENTE: <skill o ninguna>
```

## Prohibido

- Cambiar spec para adaptarla al código defectuoso.
- Añadir comportamiento nuevo.
- Refactorizar fuera del scope.
