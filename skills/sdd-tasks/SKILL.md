---
name: sdd-tasks
description: >-
  Descompone plan.md en unidades pequeñas y verificables en specs/NNN-slug/tasks.md. Cada tarea define ID (T-001…), objetivo, RF relacionados, dependencias, componentes esperados, tests requeridos, verificaciones y criterio objetivo "Hecho cuando". Ordena por dependencias y comprueba que cada RF aparece en al menos una tarea. NO implementa código. Úsala solo con TRAZABILIDAD: PASS, o tras un cambio aprobado que modifique el plan.
---

# sdd-tasks — Descomposición en tareas

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Tareas: `T-001`, `T-002`… Estados en tasks.md: `- [ ]` pendiente, `- [x]` completada con evidencia.
- Veredicto previo obligatorio: `TRAZABILIDAD: PASS`.
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta la fila `Estado` y su `Versión`; nunca busques `Estado: …` como texto libre.
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Convertir el plan en una lista ordenada de tareas ejecutables, cada una con tests y un resultado verificable.

## Alcance

- SOLO descompone trabajo.
- NO implementa código ni tests. NO modifica plan ni spec.

## Cuándo usar

- Tras `TRAZABILIDAD: PASS`.
- Tras un cambio aprobado: reorganiza/añade/reabre tareas afectadas.

## Precondiciones

1. `specs/NNN-slug/spec.md` con la fila `Estado` en `APROBADA`.
2. `specs/NNN-slug/plan.md` existe y su `Spec-Version` coincide con la fila `Versión` de spec.md.
3. `specs/NNN-slug/trace.md` con `TRAZABILIDAD: PASS` y cabecera con el mismo `Spec-Version` y `Plan-Version` que los artefactos actuales.
Si falta algo: `TAREAS BLOQUEADAS — <lo que falta>. Siguiente paso: sdd-plan` (o `sdd-trace`).

## Contexto requerido

- `spec.md`, `plan.md`, `trace.md`.

## Entradas

- Ninguna adicional (si el usuario pide tareas "más grandes o más pequeñas", aplica la regla de granularidad y explica el límite).

## Procedimiento

1. Verifica precondiciones leyendo los archivos. Si existe tasks.md, toda re-edición debe reemplazar su cabecera por las versiones actuales y reabrir las tareas afectadas.
2. Descompón el plan en tareas. **Regla de granularidad: UNA TAREA = UN CAMBIO PEQUEÑO + TESTS + RESULTADO VERIFICABLE.** Si una tarea contiene varios comportamientos, divídela.
3. Rechaza (dividiendo) tareas del tipo: "Implementar backend", "Crear todo el sistema de autenticación", "Construir el MVP". Cada tarea debe poder implementarse y verificarse en una sola ejecución.
4. Numera `T-001`, `T-002`… (3 dígitos). Ordena por dependencias: una tarea solo puede depender de tareas con número menor ya definidas.
5. Comprueba **cobertura de tareas**: cada RF y RNF aparece en `RF relacionados` de al menos una tarea. Si no, corrige la descomposición (no el plan; si el hueco es del plan, detente y enruta a `sdd-plan`).
6. Escribe `specs/NNN-slug/tasks.md` con la plantilla. En `Verificaciones` lista QUÉ debe comprobarse (suite de tests del módulo, lint, type checking, build…), no comandos concretos: `sdd-implement` descubrirá los comandos reales inspeccionando el repositorio. Para tareas que afecten un componente visual, interfaz o flujo web, incluye explícitamente la verificación en navegador (render correcto, consola sin errores JavaScript, interacción E2E y evidencia visual) entre las `Verificaciones` y, si aplica, en `Hecho cuando`.
7. Si es una re-edición tras `sdd-change`: reabre (`- [ ]`, borrando evidencia obsoleta) las tareas cuyo comportamiento cambió, añade las nuevas al final manteniendo numeración estable, y no renumeres las existentes.
8. Muestra la lista y recomienda: `Siguiente paso: sdd-implement T-001` (la primera tarea sin dependencias pendientes).

### Plantilla por tarea (en `specs/NNN-slug/tasks.md`)

```markdown
# Tareas — NNN-slug

> Spec-Version: N · Plan-Version: M · Trazabilidad: PASS

- [ ] T-001
  - Objetivo: <capacidad concreta e indivisible>
  - RF relacionados: RF-003, RF-004
  - Dependencias: (ninguna | T-000…)
  - Componentes o archivos esperados: <rutas/áreas previstas por el plan>
  - Tests requeridos: <qué tests demostrarán los RF de esta tarea>
  - Verificaciones: <qué categorías de comprobación deben pasar>
  - Hecho cuando: <condiciones objetivas y comprobables>
  - Evidencia: (la añade sdd-implement: comandos ejecutados y resultado)

- [ ] T-002 …
```

## Artefactos de salida

- `specs/NNN-slug/tasks.md`

## Validación

- Cada tarea tiene los 8 campos completos (Evidencia queda para `sdd-implement`).
- Ninguna tarea agrupa varios comportamientos.
- Cobertura: todo RF/RNF aparece en ≥1 tarea.
- Las tareas que afectan UI/interfaz web incluyen la verificación en navegador entre sus verificaciones.
- Sin dependencias circulares ni referencias a tareas inexistentes.
- `Hecho cuando` es objetivo (comprobable por un tercero), nunca "queda bien" o "funciona".

## Condiciones de parada

- Tras escribir y mostrar tasks.md: DETENTE. Nunca comiences a implementar.

## Acciones prohibidas

- Escribir código o tests.
- Crear tareas sin RF relacionados o sin tests requeridos.
- Depender de la conversación en lugar de spec/plan/trace.
- Comenzar la implementación de T-001.

## Siguiente fase permitida

`sdd-implement T-XXX` (una tarea por ejecución)
