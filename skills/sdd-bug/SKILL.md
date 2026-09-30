---
name: sdd-bug
description: Corrige defectos donde la implementación no cumple una spec aprobada. Reproduce el fallo, crea un test de regresión que falle, identifica causa raíz, aplica el fix mínimo y verifica regresión. NO cambia la spec salvo que descubra que el comportamiento esperado es ambiguo o realmente debe cambiar; en ese caso detiene y enruta a sdd-change.
---

# sdd-bug — Corrección de defectos

## Propósito
Corregir un bug sin reiniciar innecesariamente el ciclo SDD cuando la intención ya está definida por una spec aprobada.

## Regla clave
- Si **código != spec** → es BUG: corrige código/tests.
- Si el usuario quiere **cambiar lo que dice la spec** → no es bug: `sdd-change`.
- Si la spec es ambigua o insuficiente → DETENTE: `sdd-change`.

## Precondiciones
- Existe una spec aprobada relacionada con el comportamiento.
- Hay implementación existente que pueda inspeccionarse.
- Si no se puede identificar la intención esperada desde la spec, no inventes: `BUG BLOQUEADO — Siguiente paso: sdd-change`.

## Procedimiento
1. Identifica spec y RF afectados.
2. Reproduce el fallo con el caso mínimo posible.
3. **RED:** crea un test de regresión que falle por el bug.
4. Localiza la causa raíz; evita parches de síntomas si la causa puede corregirse razonablemente.
5. **GREEN:** aplica el cambio mínimo para pasar el test.
6. **REFACTOR:** solo si mejora claramente el código sin cambiar comportamiento.
7. Ejecuta tests afectados y verificaciones proporcionales al riesgo; amplía regresión si el cambio toca límites compartidos.
8. Registra el bug en `specs/NNN-slug/bugs/BUG-NNN.md` y ciérralo solo con evidencia.

## Plantilla de bug
```markdown
# BUG-NNN — <título>
- Spec/RF: RF-XXX
- Síntoma: <observable>
- Reproducción: <mínima>
- Causa raíz: <concreta>
- Test de regresión: <archivo/comando>
- Fix: <archivos modificados>
- Verificación: <comandos → resultado>
- Estado: ABIERTO | CERRADO
```

## Salida en conversación
Máximo 6 líneas salvo bloqueo complejo:
```text
BUG: BUG-NNN — <título>
RF: RF-XXX
CAUSA: <resumen>
FIX: <resumen>
TESTS: <comando> → PASS/FAIL
RESULTADO: CORREGIDO | BLOQUEADO — siguiente paso: <skill>
```

## Acciones prohibidas
- Cambiar spec para hacerla coincidir con código defectuoso.
- Añadir comportamiento nuevo durante el fix.
- Refactorizar áreas no relacionadas.
- Cerrar sin test de regresión cuando sea razonablemente posible.

## Siguiente fase permitida
Bug corregido: fin del flujo · intención funcional distinta/ambigua: `sdd-change` · problema puramente estructural sin cambio funcional: `sdd-refactor`.

## Bugs UI

Reproducir primero en navegador cuando el bug sea visible. Registrar URL, viewport, pasos, esperado, observado y evidencia en `specs/NNN-slug/browser-review.md` con `Scope: TASK` cuando el bug sea visible. Añadir regression test y, si la feature queda afectada, actualizar/revalidar `ui-review` y `browser-review FINAL`.
