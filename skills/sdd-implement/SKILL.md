---
name: sdd-implement
description: Implementa EXACTAMENTE UNA tarea (T-XXX) de tasks.md aplicando TDD estricto (RED → GREEN → REFACTOR), tras verificar spec aprobada, cero [NECESITA ACLARACIÓN], trazabilidad PASS y dependencias completadas. Descubre las verificaciones reales del proyecto inspeccionándolo (tests, lint, type check, build, auditoría) sin inventar comandos. Registra evidencia, marca la tarea solo si todo pasa, informa y SE DETIENE. Es la skill para implementar tareas planificadas; `sdd-bug` puede escribir fixes y `sdd-refactor` puede modificar estructura sin cambiar comportamiento.
---

# sdd-implement — Implementación de UNA tarea con TDD

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `specs/NNN-slug/{spec,clarify,ui-design-brief,ui-spec,plan,migration-review,trace,tasks,ui-review,browser-review,validation,doc-sync,release}.md` según corresponda.
- Si `ui-spec.md` aplica, lee `Spec-Version` y `UI-Spec-Version`; una versión incompatible está CADUCADA.
- Estados de tarea: `- [ ]` pendiente · `- [x]` completada (solo con evidencia registrada).
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta las filas `Estado`, `Aprobación` y `Versión`; nunca busques `Estado: …` como texto libre.
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Convertir UNA tarea en código + tests que la demuestren, con evidencia verificable.

## Alcance

- Implementa código de producto asociado a tareas planificadas.
- `sdd-bug` queda autorizado exclusivamente para fixes de defectos y `sdd-refactor` para cambios internos sin cambio funcional.
- Trabaja sobre UNA sola tarea por ejecución. Jamás sobre dos.

## Cuándo usar

- Cuando el usuario lo solicita indicando explícitamente la tarea: `sdd-implement T-XXX`.

## Precondiciones (verifica TODAS leyendo los archivos, antes de tocar código)

1. `docs/constitution.md` con `Estado: APROBADA`.
2. `AGENTS.md` existe.
3. `specs/NNN-slug/spec.md` con las filas `Estado` y `Aprobación` en `APROBADA`.
4. Cero `[NECESITA ACLARACIÓN` en spec.md.
5. `plan.md` y `trace.md` están vinculados a la `Versión` actual de spec.md; `trace.md` tiene `TRAZABILIDAD: PASS`.
6. `tasks.md` está vinculado al `Spec-Version` y `Plan-Version` actuales y contiene la tarea `T-XXX`.
7. Todas las dependencias de la tarea están `- [x]` con evidencia.

Si falta cualquiera: `IMPLEMENTACIÓN BLOQUEADA — <precondición ausente>. Siguiente paso: <skill correcta>`.
Si no se indica `T-XXX`: pide la tarea. Nunca elijas una por tu cuenta.

## Contexto requerido

Lee antes de modificar código: `docs/constitution.md` · `AGENTS.md` · `spec.md` · `plan.md` · `tasks.md` (y el código existente que toque la tarea).

## Entradas

- `T-XXX` (obligatorio).

## Procedimiento

1. Verifica las precondiciones.
2. **Inspecciona el proyecto** para descubrir stack, gestor de paquetes, runner de tests, linter, formateador, type checker, build y auditorías (manifiestos: `package.json`, `pyproject.toml`/`requirements*.txt`, `Cargo.toml`, `go.mod`, `pom.xml`/`build.gradle*`, `*.csproj`, `Makefile`, `justfile`, configuración de CI…). Usa SOLO comandos que existan en el proyecto. Si una categoría no existe, regístralo; no inventes comandos.
3. **RED.** Escribe los tests que demuestren los RF de esta tarea según `Tests requeridos`. Ejecútalos y confirma que fallan por la razón correcta (no por errores de compilación/importación evitables).
4. **GREEN.** Implementa el mínimo código necesario para que los tests pasen. Sin comportamiento extra: cualquier capacidad no pedida por los RF de esta tarea no se escribe.
5. **REFACTOR.** Mejora estructura y claridad SIN cambiar comportamiento. Re-ejecuta los tests tras cada cambio.
6. **Verificación.** Ejecuta todas las categorías aplicables descubiertas en el paso 2: suite de tests completa · lint · formateo · type checking · build · análisis estático · auditoría de dependencias · verificaciones de seguridad. Todo debe pasar.
7. **Cierre.** Solo si TODO pasa: marca `- [x]` la tarea en tasks.md y añade bajo `Evidencia:` los comandos ejecutados con su resultado y los archivos creados/modificados. Si algo falla y no puedes resolverlo dentro del alcance de la tarea: deja `- [ ]`, informa el fallo y DETENTE.
8. **Informe final** (formato obligatorio):

```
TAREA: T-XXX — <objetivo>
RF IMPLEMENTADOS: RF-00X, RF-00Y
ARCHIVOS CREADOS: …
ARCHIVOS MODIFICADOS: …
TESTS CREADOS: …
TESTS EJECUTADOS: <suite> → resultado
VERIFICACIONES: lint → …, typecheck → …, build → … (las no aplicables: motivo)
RESULTADO: COMPLETADA | INCOMPLETA (motivo)
```

9. **DETENTE.**

### Desvíos obligatorios durante la ejecución

- Detectas que falta un requisito o la spec está mal → DETENTE, no "lo remiendas": `Siguiente paso: sdd-change`.
- Necesitas una decisión técnica no prevista en plan.md → DETENTE: `Siguiente paso: sdd-plan` (y re-trazar).
- La tarea está mal descompuesta o su "Hecho cuando" es inalcanzable → DETENTE: `Siguiente paso: sdd-tasks`.
- Necesitas una dependencia nueva → detente y justifícala al usuario antes de instalarla; si el plan no la contempla → `sdd-plan`.

## Artefactos de salida

- Código y tests de la tarea.
- `tasks.md` actualizado: `- [x]` + bloque `Evidencia:` (solo si todo pasó).

## Validación

- Los tests nuevos demostraban los RF y fallaban antes de la implementación.
- Todas las verificaciones del paso 6 pasaron en esta ejecución (no "las pasaron la vez anterior").
- tasks.md refleja el estado real; nunca `- [x]` sin evidencia.
- No modificaste tareas distintas de T-XXX.

## Condiciones de parada

- Tras el informe final, en éxito o en fallo: DETENTE.
- Está EXPLÍCITAMENTE PROHIBIDO comenzar automáticamente la siguiente tarea.

## Acciones prohibidas

- Implementar más de una tarea por ejecución.
- Implementar comportamiento sin RF que lo respalde.
- Marcar una tarea completada con verificaciones fallando o sin ejecutar.
- Inventar comandos de verificación inexistentes.
- Modificar spec.md, plan.md o trace.md directamente.
- Introducir cambios arquitectónicos silenciosos.
- Añadir dependencias sin justificación explícita y sin reflejo en plan.md.

## Siguiente fase permitida

`sdd-implement T-YYY` (siguiente tarea, SOLO si el usuario la solicita) · `sdd-validate` (cuando todas las tareas estén completadas y el usuario lo pida).

## Gate de UI

Si la tarea afecta UI:
1. Lee `ui-spec.md` vigente.
2. Respeta `design/` y tokens existentes.
3. Implementa estados, responsive, accessibility y motion definidos.
4. Ejecuta una **verificación browser de tarea** antes de marcar T-XXX completa; esa verificación debe limitarse al alcance implementado por T-XXX y registrarse en `browser-review.md` con `Scope: TASK`.
5. Si el flujo completo todavía no puede probarse porque depende de tareas posteriores, no lo uses como motivo para bloquear T-XXX; el `browser-review` FINAL se ejecutará cuando todas las tareas estén completas.

El browser check no se sustituye por tests unitarios.
