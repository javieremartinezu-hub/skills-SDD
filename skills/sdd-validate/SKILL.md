---
name: sdd-validate
description: "Validación final de una spec. Ejecuta una vez la suite completa y las verificaciones del proyecto registrando el commit, demuestra la cadena requisito → tarea → código → test para cada RF/RNF/UI, revisa casos límite, criterios de finalización y constitución, y emite SPEC CUMPLIDA: SÍ o NO en validation.md. Nunca acepta \"parece funcionar\". Úsala cuando todas las tareas estén completadas con evidencia."
---

# sdd-validate — Validación final

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

spec `APROBADA`; `tasks.md` vigente con `TRAZABILIDAD: PASS` y todas las tareas `- [x]` con evidencia. Si no: `VALIDACIÓN BLOQUEADA — … Siguiente paso: sdd-implement T-XXX`.

## Procedimiento

1. Registra el commit actual (SHA) y si el árbol de trabajo está limpio.
2. **Corrida completa única:** suite de tests completa y todas las verificaciones de `AGENTS.md` (lint, typecheck, build, seguridad/dependencias). Las categorías inexistentes se marcan "no disponible".
3. **Cadena por requisito.** Para cada RF/RNF/UI: tarea(s) → archivos → test que lo demuestra → resultado en esta corrida. Toma tareas y archivos de la evidencia de `tasks.md`; el resultado debe salir de esta ejecución. Los UI confirmados por el usuario citan la confirmación registrada en la tarea.
4. **Modo incremental** (tras un cambio LOCAL con validación previa): rehaz solo las filas de requisitos afectados y re-sella el resto. La corrida completa del paso 2 es siempre obligatoria.
5. Comprueba casos límite, criterios de finalización y principios de la constitution (una línea cada uno).
6. Escribe `validation.md` y emite el veredicto:
   - Todo con evidencia → `SPEC CUMPLIDA: SÍ` → `Siguiente paso: sdd-release`.
   - Cualquier hueco → `SPEC CUMPLIDA: NO` con el siguiente paso concreto (`sdd-implement T-XXX` o `sdd-change`).
7. DETENTE. No arregles nada.

## Plantilla de `validation.md`

```markdown
# Validación — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Plan-Version | M |
| Commit | <sha> (árbol limpio: sí/no) |

## Cadena de evidencia
| Req | Tarea | Archivos | Test | Resultado |
|---|---|---|---|---|

## Verificaciones del proyecto
| Categoría | Comando | Resultado |
|---|---|---|

## Casos límite y criterios de finalización
## Constitution
| Principio | Cumple / evidencia |

SPEC CUMPLIDA: SÍ | NO
```

## Prohibido

Modificar código o tests, emitir SÍ con algún requisito sin evidencia, aceptar resultados no producidos en esta ejecución o la palabra de la conversación.
