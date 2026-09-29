---
name: sdd-implement
description: Implementa EXACTAMENTE UNA tarea T-XXX con scope limitado, reuse-first, solución mínima, TDD pragmático, verificaciones reales y self-review final. Marca la tarea completada solo con evidencia y se detiene.
---

# sdd-implement — Implementación de UNA tarea

## Precondiciones

1. Constitution aprobada y `AGENTS.md` presente.
2. Spec aprobada, sin `[NECESITA ACLARACIÓN]`.
3. Plan/trace/tasks vigentes; `TRAZABILIDAD: PASS`.
4. `T-XXX` existe y sus dependencias están completadas.
5. El usuario indicó `T-XXX`; nunca elijas otra por tu cuenta.

Si falla algo: bloquea y enruta a la skill correspondiente.

## Procedimiento

1. Lee constitution, AGENTS, spec, plan, tasks y el código relacionado.
2. **Scope guard:** identifica archivos/módulos/contratos/tests probablemente afectados. Busca primero helpers, patrones o componentes existentes reutilizables.
3. Descubre comandos reales del proyecto para tests/lint/typecheck/build/seguridad; no inventes comandos.
4. **RED cuando aporte valor:** escribe el test mínimo del comportamiento requerido y confirma el fallo correcto. Para cambios triviales ya cubiertos puede bastar ampliar/verificar tests existentes.
5. **GREEN:** implementa el cambio mínimo correcto. No agregues capacidades futuras, capas, abstracciones ni dependencias innecesarias.
6. **REFACTOR opcional:** solo mejoras claras y dentro del scope, manteniendo verde.
7. Ejecuta verificaciones proporcionales al riesgo; amplía a suite completa cuando el impacto lo justifique.
8. **Verificación en navegador (obligatoria para UI):** si la tarea crea, modifica o corrige un componente visual, interfaz o flujo web, antes de darla por terminada: (1) abre la aplicación en ejecución con la herramienta de automatización de navegador del proyecto (p. ej. `playwright-cli`); (2) valida el flujo funcional E2E en navegador real: sin errores de renderizado ni de consola (JavaScript) y con la interacción operativa; (3) registra evidencia visual (captura de pantalla o DOM) que confirme el resultado requerido; (4) si falla, corrige el código y repite la validación web hasta que pase.
9. **Self-review obligatorio del diff:**
   - ¿cumple RF/tarea sin extras?;
   - ¿tocó algo fuera de scope?;
   - ¿duplicó funcionalidad existente?;
   - ¿introdujo abstracción/dependencia innecesaria?;
   - ¿preservó contratos y compatibilidad?;
   - ¿cubrió casos borde obvios?;
   - ¿los tests prueban comportamiento y no detalles accidentales?;
   - ¿los cambios de UI se validaron en navegador con evidencia?
10. Si todo pasa, marca `T-XXX` como `- [x]` y registra evidencia. Si no, déjala pendiente.
11. DETENTE.

## Dependency guard

Antes de añadir una dependencia demuestra: necesidad concreta, alternativa existente insuficiente, mantenimiento razonable, impacto de seguridad/licencia y reflejo en `plan.md` cuando corresponda. Si no puede justificarse, no se añade.

## Desvíos

- Requisito faltante/cambio funcional → `sdd-change`.
- Migración/compatibilidad relevante → `sdd-migration`.
- Decisión técnica no prevista → `sdd-plan`.
- Tarea mal descompuesta → `sdd-tasks`.
- Bug ajeno detectado → no lo arregles silenciosamente; `sdd-bug`.

## Salida

```text
TAREA: T-XXX — COMPLETADA | INCOMPLETA
CAMBIOS: <resumen breve>
TESTS: <resumen → resultado>
CHECKS: <lint/typecheck/build/browser-UI (si aplica)/...>
SELF-REVIEW: PASS | hallazgo
SIGUIENTE: <skill o ninguna>
```

## Prohibido

- Trabajar más de una tarea.
- Cambiar spec/plan/trace.
- Reescribir módulos completos si un cambio localizado resuelve la tarea.
- Afirmar verificaciones no ejecutadas.
- Marcar completada una tarea de UI sin la verificación en navegador con evidencia.
