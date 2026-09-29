---
name: sdd-refactor
description: Mejora calidad interna sin cambiar comportamiento observable: simplificación, duplicación, nombres, cohesión, separación de responsabilidades o deuda técnica. Protege el comportamiento con tests existentes o characterization tests, aplica cambios pequeños y ejecuta regresión. Si requiere cambiar comportamiento o arquitectura contractual, detiene y enruta a sdd-change.
---

# sdd-refactor — Mejora interna sin cambio funcional

## Regla clave
Un refactor cambia **cómo** está construido el código, no **qué** hace el sistema.

## Precondiciones
- El comportamiento esperado está definido.
- Existe cobertura suficiente para proteger el área; si no, añade primero characterization tests mínimos.

## Procedimiento
1. Define el objetivo concreto del refactor y el alcance mínimo.
2. Ejecuta tests actuales y confirma baseline verde.
3. Añade tests de caracterización solo si falta protección relevante.
4. Aplica cambios pequeños, sin nuevas capacidades.
5. Ejecuta tests después de cada cambio relevante.
6. Ejecuta verificaciones proporcionales: lint/typecheck/build/tests según proyecto.
7. Inspecciona el diff para confirmar ausencia de cambios funcionales accidentales.

## Desvíos
- Cambia comportamiento esperado → `sdd-change`.
- Descubre un defecto funcional → `sdd-bug`.
- Requiere decisión arquitectónica importante o contrato/API nuevo → `sdd-change` o `sdd-plan` según corresponda.

## Salida en conversación
```text
REFACTOR: <objetivo>
ARCHIVOS: <resumen>
TESTS: <comando> → PASS/FAIL
VERIFICACIONES: <resumen>
RESULTADO: COMPLETADO | BLOQUEADO — <siguiente paso>
```
Mantén la respuesta breve; no expliques decisiones obvias salvo que se solicite.

## Refactor UI

Si afecta UI, tomar baseline browser antes del refactor y repetir el flujo después para demostrar ausencia de cambio funcional/visual no intencionado.
