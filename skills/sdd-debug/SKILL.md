---
name: sdd-debug
description: Diagnostica fallos cuando todavía no está clara la causa, el alcance o incluso si se trata de un bug. Trabaja por evidencia e hipótesis, reproduce de forma controlada y termina con causa raíz confirmada o próximos experimentos. No aplica fixes de producción.
---

# sdd-debug — Diagnóstico basado en evidencia

## Cuándo usar

- Fallo intermitente o difícil de reproducir.
- No está claro si es bug, configuración, datos, dependencia, infraestructura o requisito.
- Se necesita causa raíz antes de tocar producción.

## Procedimiento

1. Define síntoma observable, esperado y alcance conocido.
2. Recoge evidencia mínima: logs, errores, tests, inputs, entorno y camino de ejecución relevante.
3. Formula pocas hipótesis ordenadas por evidencia, no por intuición.
4. Diseña el experimento más barato que discrimine entre hipótesis.
5. Reproduce o descarta hipótesis una a una; no modifiques producción para “probar suerte”.
6. Identifica causa raíz solo cuando exista evidencia suficiente.
7. Enruta:
   - defecto confirmado → `sdd-bug`;
   - cambio de requisito → `sdd-change`;
   - migración/compatibilidad → `sdd-migration`;
   - problema externo/configuración → informa acción concreta correspondiente.

## Salida

```text
DEBUG: CONFIRMADO | INCONCLUSO
SÍNTOMA: <breve>
CAUSA: <evidencia> | pendiente
DESCARTADO: <hipótesis principales>
SIGUIENTE: <sdd-bug/sdd-change/sdd-migration/experimento>
```

## Prohibido

- Hacer múltiples cambios especulativos a la vez.
- Declarar causa raíz sin evidencia.
- Aplicar un fix de producción; esa responsabilidad pasa a `sdd-bug` u otro flujo.
