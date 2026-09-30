---
name: sdd-validate
description: >-
  Validación final de una spec. Recorre TODOS los RF y demuestra la cadena completa RF → PLAN → TASK → CODE → TEST con evidencia real (comandos ejecutados y su salida), comprueba RNF, casos límite, criterios de finalización, constitution y verificaciones del proyecto (suite completa, lint, type check, build, seguridad), y emite SPEC CUMPLIDA: SÍ o NO en specs/NNN-slug/validation.md. Nunca acepta "parece funcionar" como evidencia. Úsala cuando todas las tareas de tasks.md estén completadas con evidencia.
---

# sdd-validate — Validación final de la spec

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `specs/NNN-slug/{spec,clarify,ui-design-brief,ui-spec,plan,migration-review,trace,tasks,ui-review,browser-review,validation,doc-sync,release}.md` según corresponda.
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`.
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta sus filas `Estado`, `Aprobación` y `Versión`; nunca busques `Estado: …` como texto libre.
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Probar, con evidencia, que la implementación cumple TODOS los requisitos de la spec.

## Alcance

- SOLO audita y emite veredicto.
- NO implementa, NO repara código. Ante huecos, enruta (`sdd-implement` para tareas; `sdd-change` para requisitos nuevos o mal especificados).

## Cuándo usar

- Cuando `tasks.md` muestra TODAS las tareas `- [x]` con evidencia y el usuario pide validar o cerrar la spec.

## Precondiciones

1. `specs/NNN-slug/spec.md` con la fila `Estado` en `APROBADA`.
2. `plan.md`, `trace.md` y `tasks.md` están vinculados a la `Versión` actual de spec.md; trace.md tiene `TRAZABILIDAD: PASS`.
3. `tasks.md` existe y todas sus tareas están `- [x]` con bloque `Evidencia:`.
Si falta: `VALIDACIÓN BLOQUEADA — <lo que falta>. Siguiente paso: sdd-implement T-XXX`.

## Contexto requerido

- `spec.md`, `plan.md`, `trace.md`, `tasks.md`, `docs/constitution.md`.
- El repositorio completo (código, tests, manifiestos para descubrir verificaciones reales).

## Entradas

- Ninguna adicional.

## Procedimiento

1. Verifica precondiciones.
2. **Cadena de evidencia por requisito.** Para CADA RF y RNF construye la fila: plan · tarea · archivos · test ejecutado · resultado. Para cada UI-XXX añade UI Spec → plan → task → componente → browser evidence. Un requisito sin eslabón completo = sin evidencia.
3. Ejecuta la suite completa de tests y las verificaciones aplicables del proyecto (descúbrelas inspeccionando el repositorio: tests, lint, formateo, type checking, build, análisis estático, auditoría de dependencias, seguridad). Registra comandos y resultados. No inventes comandos; las categorías inexistentes se marcan "no disponible en el proyecto".
4. Verifica contra los artefactos: casos límite · criterios de finalización · constitution. Si hay UI, requiere `ui-review.md = PASS` y `browser-review.md Scope: FINAL = PASS` cuando corresponda. Si el plan indicó migración, requiere `migration-review.md` suficiente.
5. Escribe `specs/NNN-slug/validation.md` (plantilla).
6. **Veredicto:**
   - Todos los requisitos y gates aplicables con evidencia completa → `SPEC CUMPLIDA: SÍ`. Escribe el veredicto y DETENTE. El siguiente gate será `sdd-doc-sync` si hay docs afectadas; después `sdd-release`.
   - Cualquier hueco → `SPEC CUMPLIDA: NO`, con el siguiente paso concreto. DETENTE.
7. Nunca aceptes como evidencia: "parece funcionar", "debería funcionar", "estaba hecho antes", o salidas no producidas en esta ejecución.

### Plantilla de `specs/NNN-slug/validation.md`

```markdown
# Validación — NNN-slug

> Fecha: YYYY-MM-DD · Spec-Version: N · Plan-Version: M

## Cadena de evidencia
| RF | Descripción | Plan (sección) | Tarea | Implementación (archivos) | Test (comando → resultado) | Resultado |
|----|-------------|----------------|-------|---------------------------|----------------------------|-----------|
| RF-001 | … | §5 | T-003 | src/… | <suite>::test_x → PASS | OK |

## Requisitos no funcionales
| RNF | Evidencia | Resultado |

## Casos límite
| Caso | Test que lo demuestra | Resultado |

## Criterios de finalización
- [x]/[ ] <criterio> — <evidencia o motivo del incumplimiento>

## Verificaciones del proyecto
| Categoría | Comando | Resultado |

## Conformidad con docs/constitution.md
| Principio | Cumplimiento y cómo se comprobó |

## Veredicto
SPEC CUMPLIDA: SÍ | NO
```

## Artefactos de salida

- `specs/NNN-slug/validation.md`

## Validación

- La matriz contiene TODOS los RF y RNF, sin excepciones.
- Cada celda `Test` cita un comando ejecutado en esta validación y su resultado.
- El veredicto es coherente con la evidencia.

## Condiciones de parada

- Tras emitir el veredicto: DETENTE, sea SÍ o NO. No "arregles" los huecos tú misma.

## Acciones prohibidas

- Modificar código o tests para hacer pasar la validación.
- Emitir `SPEC CUMPLIDA: SÍ` con cualquier requisito sin evidencia.
- Aceptar verificaciones no ejecutadas como pasadas.
- Dar por válida la palabra de la conversación frente al contenido de los artefactos.

## Siguiente fase permitida

`SPEC CUMPLIDA: SÍ` → `sdd-doc-sync` si aplica → `sdd-release`.
`SPEC CUMPLIDA: NO` → `sdd-implement T-XXX` o `sdd-change` según la causa.

## Browser validation

Cuando la feature tenga UI, la matriz de validación debe incluir evidencia de `sdd-browser-review`. Un requisito visual/interactional sin evidencia browser no se considera cubierto salvo que la UI Spec haya declarado explícitamente que no requiere browser verification y exista una justificación documentada.
