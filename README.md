# SDD Framework — v6 · Spec-Driven Development + TDD

Framework portable y agnóstico de stack para agentes de IA. SDD es el flujo de trabajo; TDD es el mecanismo de evidencia del código. Las decisiones de interfaz las declara el usuario en la spec (UI-NNN) y el agente las implementa sin inventar diseño.

## Modelo

```text
SPEC   = QUÉ + POR QUÉ (+ interfaz declarada por el usuario)
PLAN   = CÓMO
TASKS  = TRAZABILIDAD + EJECUCIÓN
TESTS  = EVIDENCIA
VALIDATE / RELEASE = GATES FINALES
```

Cadena: `REQUISITO (RF/RNF/UI) → PLAN → TAREA → CÓDIGO → TEST → VALIDACIÓN`

## Fuente de verdad

Los artefactos versionados del repositorio, nunca la conversación. Las convenciones comunes (cabeceras, versiones, veredictos, formato de bloqueo) y los comandos de verificación del proyecto viven en un único lugar: `AGENTS.md`, generado por `sdd-agents`.

## Skills (17)

| Skill | Responsabilidad |
|---|---|
| `sdd-orchestrator` | Router determinista: único siguiente paso |
| `sdd-init` | Brief del proyecto (nuevo o existente) |
| `sdd-constitution` | Principios innegociables + aprobación |
| `sdd-agents` | AGENTS.md: reglas, convenciones y comandos |
| `sdd-spec` | QUÉ + POR QUÉ, tamaño e interfaz declarada |
| `sdd-clarify` | Auditoría de la spec + aprobación humana |
| `sdd-plan` | Diseño técnico (completo / breve / delta) |
| `sdd-migration` | Gate de compatibilidad (si aplica) |
| `sdd-tasks` | Trazabilidad + descomposición en tareas |
| `tdd` | Método RED → GREEN → REFACTOR → VERIFICAR |
| `sdd-implement` | Implementa las tareas en cadena (o una sola) |
| `sdd-validate` | Validación final con corrida completa |
| `sdd-release` | Gate READY/BLOCKED + sincronización de docs |
| `sdd-change` | Cambios de comportamiento o interfaz desde la spec |
| `sdd-bug` | Diagnóstico + fix con regresión |
| `sdd-refactor` | Cambio interno sin cambio observable |
| `sdd-review` | Auditoría quality / implementation |

## Flujo de una feature nueva

```text
sdd-init → sdd-constitution (aprobación) → sdd-agents      ← una vez por proyecto
sdd-spec → sdd-clarify (aprobación) → sdd-plan → [sdd-migration]
→ sdd-tasks → sdd-implement (encadena T-001 … T-NNN, una a la vez)
→ sdd-validate → sdd-release
```

Tamaño `S` (≤3 RF, sin migración, sin cambios de permisos/seguridad ni dependencias nuevas): plan en modo breve y 1-3 tareas; los gates se mantienen.

## Mantenimiento

| Situación | Flujo |
|---|---|
| Bug o fallo de causa incierta | `sdd-bug` (diagnóstico → regresión → fix) |
| Cambio de comportamiento o interfaz | `sdd-change → sdd-clarify → plan (delta si LOCAL) → tasks (reabre solo lo afectado) → implement → validate (incremental si LOCAL) → release` |
| Mejora interna | `sdd-refactor` |
| Auditoría | `sdd-review` |

## Gates

| Gate | Condición |
|---|---|
| Constitution | `docs/constitution.md` APROBADA |
| Spec | `SPEC CLARIFICADA: SÍ` + spec APROBADA por el usuario |
| Plan | `plan.md` vigente respecto de la spec |
| Migración | Si aplica: `MIGRACIÓN: SUFICIENTE` |
| Trazabilidad | `tasks.md` con `TRAZABILIDAD: PASS` |
| Tarea | `- [x]` solo con evidencia de esa ejecución |
| Validación | `SPEC CUMPLIDA: SÍ` |
| Release | `RELEASE: READY` |

## Artefactos

```text
docs/brief.md
docs/constitution.md
AGENTS.md                  # reglas + convenciones + comandos
specs/NNN-slug/
├── spec.md
├── clarify.md
├── plan.md
├── migration-review.md    # si aplica
├── tasks.md               # incluye la matriz de trazabilidad
├── validation.md
├── release.md
└── bugs/BUG-NNN.md
```

Las plantillas están dentro de cada skill. Un derivado con `Spec-Version`/`Plan-Version` distinta de la actual está CADUCADO; si un cambio LOCAL no lo afecta, se re-sella.

## Reglas críticas

1. Sin spec aprobada no hay funcionalidad nueva.
2. Ninguna ambigüedad se resuelve en silencio.
3. `sdd-implement` encadena las tareas sin pedir permiso, pero cierra cada una con evidencia y un commit propio antes de abrir la siguiente y se detiene ante cualquier fallo o desvío. `sdd-implement T-XXX` ejecuta solo una.
4. Sin arquitectura ni dependencias fuera del plan.
5. Tests son evidencia; nada se da por pasado sin ejecutarse.
6. El agente no inventa decisiones de interfaz: implementa las declaradas.
7. Los cambios funcionales pasan por `sdd-change`, los bugs por `sdd-bug` y los refactors por `sdd-refactor`.

## Costo de verificación

- `sdd-implement`: tests de la tarea y del módulo + lint/typecheck de lo tocado.
- `sdd-validate`: una corrida completa, registrando el commit.
- `sdd-release`: reutiliza esa evidencia si el commit no cambió.

## Instalación

Copia `skills/*` a `~/.agents/skills/`, `~/.pi/agent/skills/` o al directorio de skills de tu harness compatible con Agent Skills (o a `.agents/skills/` dentro del repo).

## Inicio

Ejecuta `sdd-orchestrator` ante cualquier duda sobre el estado o el siguiente paso.
