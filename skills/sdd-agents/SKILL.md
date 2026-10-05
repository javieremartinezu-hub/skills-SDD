---
name: sdd-agents
description: Genera o actualiza AGENTS.md en la raíz del repositorio, el manual operativo permanente para agentes de IA en un proyecto SDD. Contiene las reglas de trabajo, las Convenciones SDD compartidas por todas las skills (cabeceras, versiones, veredictos, formato de bloqueo) y los comandos de verificación reales del proyecto. Úsala tras aprobar docs/constitution.md, o cuando el usuario pida crear o actualizar las reglas para agentes.
---

# sdd-agents — Manual operativo para agentes

## Propósito

Crear `AGENTS.md`: reglas de trabajo + **Convenciones SDD** + **Commits** + **Comandos de verificación**. Es la única copia de las convenciones; el resto de skills la referencian en lugar de repetirla, y los comandos se descubren una vez en lugar de en cada ejecución.

## Precondiciones

`docs/constitution.md` con `Estado: APROBADA`. Si no: `AGENTS.MD BLOQUEADA — Falta constitution aprobada. Siguiente paso: sdd-constitution`.

## Procedimiento

1. Lee `docs/constitution.md` y, si existe, el `AGENTS.md` actual. Conserva su contenido propio del proyecto que no contradiga SDD; ante conflicto prevalece SDD e informa al usuario.
2. Inspecciona los manifiestos del repositorio (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`/`build.gradle*`, `*.csproj`, `Makefile`, `justfile`, CI…) y rellena la tabla de comandos solo con comandos que existan. Si el proyecto aún no tiene código, deja la tabla vacía: la primera ejecución de `sdd-implement` la completará.
3. Redacta `AGENTS.md` con la plantilla (máx. ~100 líneas). Si el proyecto ya tiene convención de commits (commitlint, CONTRIBUTING), adapta el formato a ella. No copies la constitution ni incluyas requisitos de producto o decisiones técnicas.
4. Muestra el contenido, pide confirmación y escríbelo.
5. `Siguiente paso: sdd-spec` (o `sdd-orchestrator` si ya existen specs). DETENTE.

## Plantilla de `AGENTS.md`

```markdown
# AGENTS.md — Reglas operativas para agentes de IA

Los requisitos viven en specs/NNN-slug/spec.md; las reglas innegociables en docs/constitution.md.

## Antes de tocar código
1. Lee docs/constitution.md y la spec activa.
2. Lee de plan.md y tasks.md solo lo que la tarea necesita.

## Mientras trabajas
3. No implementes comportamiento sin requisito (RF/RNF/UI) en la spec.
4. Una tarea a la vez: ciérrala con tests y evidencia en tasks.md antes de pasar a la siguiente. Cada tarea cerrada lleva su propio commit. Las tareas de una spec se encadenan sin pedir permiso; detente solo ante fallo, desvío, decisión pendiente o si el usuario pidió una única tarea.
5. Tests antes o junto con la implementación.
6. No avances con verificaciones fallando.
7. Sin dependencias nuevas ni decisiones de arquitectura fuera de plan.md. Si hacen falta, DETENTE y propone sdd-plan o sdd-change.
8. Interfaz: las decisiones visuales y de UX las declara el usuario en la sección Interfaz de la spec (UI-NNN). Implementa exactamente lo declarado; no inventes ni "mejores" diseño. Si falta una decisión, pregunta.

## Al terminar
9. Informa archivos creados/modificados, tests y verificaciones ejecutadas con su resultado, y el siguiente paso. Sé conciso; no expliques el proceso salvo petición.

## Rutas
Fase dudosa → sdd-orchestrator · bug o fallo de causa incierta → sdd-bug · cambio de comportamiento → sdd-change · mejora interna → sdd-refactor · auditoría → sdd-review.

## Convenciones SDD
- Fuente de verdad: los artefactos del repositorio, nunca la conversación.
- Artefactos: docs/brief.md · docs/constitution.md · AGENTS.md · specs/NNN-slug/{spec, clarify, plan, migration-review, tasks, validation, release}.md · specs/NNN-slug/bugs/BUG-NNN.md
- Cabecera: tabla `| Campo | Valor |` al inicio. spec.md: Spec-ID, Versión, Estado (BORRADOR → CLARIFICADA → APROBADA), Aprobación, Tamaño (S|M|L), Interfaz (SÍ|NO). Derivados: Spec-Version y, si aplica, Plan-Version.
- Caducidad: un derivado cuya Spec-Version o Plan-Version no coincide con la actual está CADUCADO y no supera ningún gate.
- Re-sellado: si un cambio LOCAL no afecta a un derivado, la skill dueña solo actualiza su cabecera y añade al registro "Re-sellado vN: sin cambios".
- Veredicto: una línea al final del artefacto. clarify: SPEC CLARIFICADA: SÍ|NO · migration-review: MIGRACIÓN: SUFICIENTE|INCOMPLETA · tasks: TRAZABILIDAD: PASS|FAIL · validation: SPEC CUMPLIDA: SÍ|NO · release: RELEASE: READY|BLOCKED
- IDs: RF-NNN, RNF-NNN, UI-NNN, D-NNN, T-NNN. Nunca se reutilizan.
- Marcador de duda: [NECESITA ACLARACIÓN: …]
- Aprobación humana: solo un sí inequívoco referido al artefacto mostrado. "ok", "vale", "sigue" o el silencio no cuentan.
- Bloqueo: `<ACCIÓN> BLOQUEADA — Motivo: … — Falta: … — Siguiente paso: <skill>`

## Commits
- Un commit por tarea cerrada (y por bug o refactor completado), solo con los archivos de ese trabajo + tasks.md / BUG-NNN.md.
- Formato: `feat(NNN-slug): T-XXX <objetivo>` · `fix(NNN-slug): BUG-NNN <título>` · `refactor: <objetivo>`. Cuerpo: `Cubre: RF-…, UI-…`.
- Nunca push, cambio de rama, `--no-verify` ni reescritura de historia sin indicación del usuario.

## Comandos de verificación
| Categoría | Comando | Alcance (archivo/módulo/todo) |
|---|---|---|
| Tests | | |
| Lint | | |
| Typecheck | | |
| Build | | |
| Seguridad/dependencias | | |
(Solo comandos existentes. Si uno deja de funcionar, se corrige aquí, no se improvisa.)
```

## Validación

- Contiene reglas, Convenciones SDD y tabla de comandos (vacía solo si no hay código).
- Ningún comando inventado; sin requisitos de producto.
- El usuario confirmó antes de escribir.

## Prohibido

- Incluir requisitos, user stories o decisiones técnicas.
- Sobrescribir un `AGENTS.md` sin preservar su contenido no conflictivo.
- Escribir código, tests o specs.
