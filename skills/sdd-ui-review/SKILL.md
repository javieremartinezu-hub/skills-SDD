---
name: sdd-ui-review
description: Audita estáticamente una implementación UI contra la UI Spec y el Design System vigentes. Persiste ui-review.md con evidencia de código y emite PASS o FAIL; nunca sustituye Browser Review.
---

# sdd-ui-review — Static UI Review

## Precondiciones

- `spec.md`, `ui-spec.md` y, si aplica, documentación de Design System vigentes.
- Tareas completas para la feature, salvo que el usuario solicite una revisión parcial explícita.
- Versiones de `ui-spec.md` alineadas con `spec.md`.

## Procedimiento

1. Lee `ui-spec.md` y extrae todos los `UI-XXX`.
2. Inspecciona componentes, estilos, tokens, assets, layouts y estados reales.
3. Verifica: layout, jerarquía, componentes, tokens, tipografía, color, spacing, estados, responsive, accessibility, motion, contenido/idioma, reutilización y hardcoded visual values.
4. Para cada criterio aporta evidencia concreta de archivo/símbolo/selector.
5. Escribe `specs/NNN-slug/ui-review.md` con `Spec-Version` y `UI-Spec-Version`.
6. Emite `UI REVIEW: PASS` solo si todos los criterios aplicables pasan. Un hallazgo que contradice UI Spec = FAIL.
7. DETENTE. No corrijas hallazgos.

## Importante

`UI Review` es estática. Nunca reemplaza `sdd-browser-review`, porque el código puede ser correcto y la interfaz renderizada seguir siendo incorrecta.

## Salida

```text
UI REVIEW: PASS | FAIL
HALLAZGOS: <resumen>
ARTEFACTO: specs/NNN-slug/ui-review.md
SIGUIENTE: sdd-browser-review | sdd-implement T-XXX | sdd-change
```
