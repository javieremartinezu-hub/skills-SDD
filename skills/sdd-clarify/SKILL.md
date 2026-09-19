---
name: sdd-clarify
description: >-
  Audita specs/NNN-slug/spec.md como QA Senior (ambigüedades, contradicciones, RF no verificables, casos límite ausentes, estados y errores sin definir, suposiciones implícitas, conflictos con la constitución, mezcla spec/implementación), enumera hallazgos sin corregir nada, los resuelve con el usuario UNO POR UNO y repite la auditoría hasta emitir SPEC CLARIFICADA: SÍ. Conduce después el gate de aprobación humana de la spec. Úsala cuando exista una spec en BORRADOR o modificada por sdd-change.
---

# sdd-clarify — Auditoría de clarificación y gate de aprobación

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA`. Marcador: `[NECESITA ACLARACIÓN: ...]`.
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta y actualiza sus filas `Estado`, `Aprobación` y `Versión`; nunca uses texto libre como `Estado: …`.
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`

## Propósito

Garantizar que la spec no contiene ambigüedades, contradicciones ni huecos, y obtener la **aprobación humana explícita** que desbloquea `sdd-plan`.

## Alcance

- SOLO audita y aclara la spec.
- NO implementa. NO diseña arquitectura. NO inventa soluciones: las decisiones funcionales las toma el usuario.

## Cuándo usar

- Existe una spec cuya fila `Estado` es `BORRADOR`.
- `sdd-change` modificó una spec (re-auditar antes de re-aprobar).

## Precondiciones

- `specs/NNN-slug/spec.md` existe. Si no: `CLARIFICACIÓN BLOQUEADA — No existe spec. Siguiente paso: sdd-spec`.

## Contexto requerido

- `specs/NNN-slug/spec.md`
- `docs/constitution.md` (para detectar conflictos)

## Entradas

- Respuestas del usuario, hallazgo por hallazgo.

## Procedimiento

1. Lee `spec.md` y `docs/constitution.md`.
2. **Primera pasada — SOLO enumerar.** Audita estos 12 puntos y registra cada hallazgo numerado (H-01, H-02…) en `specs/NNN-slug/clarify.md`:
   1. Ambigüedades. 2. Contradicciones (internas y con la constitution). 3. Requisitos duplicados. 4. RF no verificables. 5. Casos límite ausentes. 6. Estados incompletos o sin transiciones. 7. Errores no definidos. 8. Suposiciones implícitas. 9. Comportamientos sin definir. 10. Huecos de seguridad funcional. 11. Conflictos con `docs/constitution.md`. 12. Mezcla accidental spec/implementación (tecnología o diseño interno colado).
   - NO corrijas nada todavía.
3. **Resolución UNO POR UNO.** Para cada hallazgo: preséntalo, propone opciones SOLO si el usuario las pide, y aplica la decisión del usuario editando `spec.md`. Si el usuario no decide, el marcador `[NECESITA ACLARACIÓN: ...]` permanece.
4. **Re-auditoría.** Repite los pasos 2-3 hasta que la auditoría no produzca hallazgos nuevos.
5. **Condición de éxito** (todas obligatorias):
   - Cero `[NECESITA ACLARACIÓN` en spec.md.
   - Cero contradicciones.
   - Todos los RF verificables.
   - `Fuera de alcance` explícito y no vacío.
6. Si se cumple: escribe en `clarify.md` el veredicto `SPEC CLARIFICADA: SÍ`, actualiza la fila `Estado` a `CLARIFICADA` en spec.md y pasa al paso 7. Si no: escribe `SPEC CLARIFICADA: NO` con los hallazgos pendientes y DETENTE (plan bloqueado).
7. **Gate de aprobación humana.** Pregunta literalmente: "¿Esta especificación representa realmente la funcionalidad que quieres construir? (sí/no)".
   - CUENTA como aprobación: sí inequívoco referido a esta spec ("sí, apruebo esta spec", "aprobada", "es exactamente lo que quiero").
   - NO CUENTA: "vale", "ok", "sigue", "suena bien", "parece bien", silencio, o aprobar otra cosa. Ante ambigüedad, repite la pregunta exacta.
   - Si aprueba: actualiza en la tabla de cabecera las filas `Estado` a `APROBADA` y `Aprobación` a `APROBADA (YYYY-MM-DD, por el usuario)`. Informa: `Siguiente paso: sdd-plan`. DETENTE.
   - Si rechaza o no aprueba explícitamente: deja la fila `Aprobación` en `PENDIENTE`, informa `PLAN BLOQUEADO — Spec sin aprobación humana explícita. Siguiente paso: repetir sdd-clarify o ajustar la spec con sdd-change`. DETENTE.

### Plantilla de `specs/NNN-slug/clarify.md`

```markdown
# Informe de clarificación — NNN-slug

> Fecha: YYYY-MM-DD · Ronda: N

## Hallazgos (primera pasada: solo enumerar)
| ID | Punto auditado | Hallazgo | RF/Sección afectada | Resolución decidida por el usuario |
| H-01 | | | | |

## Auditorías de repetición
<Rondas adicionales: hallazgos nuevos o "sin hallazgos nuevos">

## Veredicto
SPEC CLARIFICADA: SÍ | NO
```

## Artefactos de salida

- `specs/NNN-slug/clarify.md`
- `specs/NNN-slug/spec.md` actualizada solo con decisiones aclaradas + veredicto de estado.

## Validación

- Cada edición de spec.md corresponde a un hallazgo con decisión explícita del usuario.
- El veredicto final refleja la condición de éxito completa.
- La fila `Estado` solo toma `APROBADA` tras el gate del paso 7.

## Condiciones de parada

- Tras `SPEC CLARIFICADA: NO`: DETENTE.
- Tras el gate de aprobación (aprobada o rechazada): DETENTE. Nunca avances a planificación sin aprobación registrada.

## Acciones prohibidas

- Corregir hallazgos automáticamente sin decisión del usuario.
- Inventar requisitos, estados o comportamientos para "cerrar" un hallazgo.
- Diseñar arquitectura o implementar.
- Interpretar respuestas ambiguas como aprobación.

## Siguiente fase permitida

`sdd-plan` (solo con spec `APROBADA` y veredicto `SPEC CLARIFICADA: SÍ`)
