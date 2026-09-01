---
name: sdd-trace
description: Verifica la trazabilidad RF → plan recorriendo TODOS los requisitos de spec.md y generando la matriz specs/NNN-slug/trace.md con estados COVERED, PARTIAL o MISSING. Detecta componentes sin justificación y contradicciones spec/plan. Emite TRAZABILIDAD: PASS (desbloquea sdd-tasks) o FAIL (bloquea). NO implementa ni corrige el plan. Úsala inmediatamente después de sdd-plan, o tras cualquier modificación de spec.md o plan.md.
---

# sdd-trace — Matriz de trazabilidad RF → plan

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Demostrar que CADA requisito de la spec tiene cobertura técnica en el plan, mediante la matriz `specs/NNN-slug/trace.md`.

## Alcance

- SOLO audita cobertura spec↔plan y emite el veredicto.
- NO implementa. NO corrige el plan ni la spec: se limita a detectar y reportar.

## Cuándo usar

- Justo después de `sdd-plan`.
- Tras cualquier cambio en `spec.md` o `plan.md` (la trazabilidad caduca con cada modificación).

## Precondiciones

- `specs/NNN-slug/spec.md` con `Estado: APROBADA` y `Aprobación: APROBADA`.
- `specs/NNN-slug/plan.md` existe.
- Si falta algo: `TRAZABILIDAD BLOQUEADA — <lo que falta>. Siguiente paso: sdd-plan` (o `sdd-orchestrator`).

## Contexto requerido

- `specs/NNN-slug/spec.md` (lista completa de RF y RNF)
- `specs/NNN-slug/plan.md`

## Entradas

- Ninguna adicional.

## Procedimiento

1. Lee spec.md y extrae la lista completa de RF (y RNF).
2. Por cada requisito, busca en plan.md su cobertura concreta (secciones, componentes, registros de decisión D-NNN, estrategia de tests). Clasifica:
   - `COVERED`: el plan muestra cómo se satisface por completo.
   - `PARTIAL`: hay cobertura pero incompleta o imprecisa (indica qué falta).
   - `MISSING`: sin cobertura.
3. Escribe la matriz en `specs/NNN-slug/trace.md` (plantilla).
4. Audita adicionalmente:
   - Componentes o decisiones del plan que NO cubren ningún RF (posible sobre-diseño: lista para justificar o eliminar).
   - Contradicciones entre spec y plan.
   - Requisitos mencionados de forma ambigua ("se maneja en general").
5. **Veredicto:**
   - Todos los requisitos `COVERED` y sin contradicciones → escribe `TRAZABILIDAD: PASS`, informa `Siguiente paso: sdd-tasks`. DETENTE.
   - Algún `PARTIAL`/`MISSING` o contradicción → escribe `TRAZABILIDAD: FAIL` con la lista de huecos, informa `TAREAS BLOQUEADAS. Siguiente paso: sdd-plan (completar el plan) o sdd-change (si el requisito está mal entendido)`. DETENTE.

### Plantilla de `specs/NNN-slug/trace.md`

```markdown
# Trazabilidad RF → plan — NNN-slug

> Fecha: YYYY-MM-DD · Basado en spec.md v<N> y plan.md v<N>

| RF | Descripción (resumen) | Cobertura en plan (secciones/D-NNN) | Estado |
|----|------------------------|--------------------------------------|--------|
| RF-001 | … | §2, D-001, §12 | COVERED |
| RNF-001 | … | §15 | COVERED |

## Componentes del plan sin RF asociado
| Componente | Justificación o propuesta de eliminación |

## Contradicciones spec ↔ plan
| ID | Descripción |

## Veredicto
TRAZABILIDAD: PASS | FAIL
```

## Artefactos de salida

- `specs/NNN-slug/trace.md`

## Validación

- La matriz contiene TODOS los RF y RNF de la spec, sin omisiones.
- Cada estado está justificado con referencias concretas a secciones del plan.
- El veredicto es coherente con la matriz.

## Condiciones de parada

- Tras emitir el veredicto: DETENTE siempre, en PASS y en FAIL.

## Acciones prohibidas

- Implementar código.
- Editar spec.md o plan.md para "arreglar" la trazabilidad.
- Marcar `COVERED` un requisito sin referencia concreta del plan.
- Emitir PASS con algún PARTIAL/MISSING o contradicción abierta.

## Siguiente fase permitida

`sdd-tasks` (solo con `TRAZABILIDAD: PASS`)
