---
name: sdd-browser-review
description: Verifica la aplicación web renderizada en un navegador real contra la UI Spec y los flujos de aceptación. Soporta revisiones TASK y FINAL, persiste browser-review.md y nunca se sustituye por inspección de código.
---

# sdd-browser-review — Browser Verification

## Dos alcances

- `Scope: TASK`: comprueba solo el alcance visible de una tarea T-XXX. Puede ejecutarse antes de completar una tarea UI.
- `Scope: FINAL`: comprueba la feature completa después de que todas las tareas estén completas. Es el gate obligatorio para declarar terminada una feature UI, salvo que el plan/UI Spec haya justificado explícitamente que no aplica.

## Precondiciones

- Para FINAL: todas las tareas de la feature completadas.
- `ui-spec.md` vigente, con `Spec-Version` igual a `spec.md` y `UI-Spec-Version` registrada.
- URL/entrypoint disponible y entorno levantable.
- Para TASK: tarea T-XXX indicada y su alcance visible identificable.

## Procedimiento

1. Determina scope TASK o FINAL.
2. Obtén URL/entrypoint, commit/version y browser.
3. Ejecuta los flujos definidos por `ui-spec.md` y, en TASK, únicamente los pasos que pertenezcan al alcance de T-XXX.
4. Verifica contenido, layout, componentes, interacciones y estados.
5. Verifica viewports definidos; si faltan y el producto es responsive, documenta los elegidos.
6. Comprueba accessibility aplicable: keyboard, focus, names/labels, roles, errores, contraste cuando sea posible y reduced motion.
7. Inspecciona console/network cuando aporten evidencia útil.
8. Captura screenshots/evidencia.
9. Compara cada `UI-XXX` y RF visible aplicable con observado.
10. Escribe/actualiza `specs/NNN-slug/browser-review.md`, conservando la evidencia anterior y agregando una entrada por versión/revisión cuando sea necesario.
11. Emite veredicto.

## Regla contra deadlock

Una revisión TASK **no** verifica funcionalidades todavía pertenecientes a tareas posteriores. La revisión FINAL sí ejecuta el flujo end-to-end completo.

## Matriz mínima

| ID | RF/UI | URL | Viewport | Pasos | Esperado | Observado | Resultado | Evidencia |
|---|---|---|---|---|---|---|---|---|

## Estados

Probar estados definidos: loading, empty, success, error, permission denied, validation, offline/stale, disabled y otros aplicables.

## Criterio

Una discrepancia contra un requisito aprobado = FAIL aunque el código sea técnicamente válido.

## Salida

```text
BROWSER REVIEW: PASS | FAIL
SCOPE: TASK | FINAL
ARTEFACTO: specs/NNN-slug/browser-review.md
HALLAZGOS: <resumen>
SIGUIENTE: sdd-implement T-XXX | sdd-ui-review | sdd-validate | sdd-change
```
