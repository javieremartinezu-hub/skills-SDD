# SDD Framework — v5 Final · Spec-Driven Development + TDD + UI/UX

Framework genérico, portable y agnóstico de stack para agentes de IA. Convierte SDD en el flujo principal de trabajo y usa TDD como mecanismo de evidencia para el código. Cuando existe interfaz, incorpora discovery UX, diseño frontend, UI Spec, Design System, UI Review y Browser Review.

## Modelo

```text
SPEC      = QUÉ + POR QUÉ
UI SPEC   = EXPERIENCIA OBSERVABLE (si hay UI)
PLAN      = CÓMO
TRACE     = COBERTURA
TASKS     = EJECUCIÓN
TESTS     = EVIDENCIA
REVIEWS   = CONTROLES
RELEASE   = GATE FINAL
```

Cadena funcional:

```text
REQUISITO → PLAN → TAREA → CÓDIGO → TEST → VALIDACIÓN
```

Cadena UI:

```text
UI-REQ → UI-SPEC → PLAN → TASK → COMPONENTE → BROWSER EVIDENCE
```

## Fuente de verdad

La conversación no es fuente permanente de verdad. Los artefactos versionados del repositorio lo son.

- La `spec.md` define comportamiento y requisitos.
- La `ui-spec.md` define experiencia visible e interacción cuando existe UI.
- El `plan.md` define decisiones técnicas.
- `trace.md` demuestra cobertura.
- `tasks.md` define ejecución.
- Tests, reviews y validation aportan evidencia ejecutada.
- `release.md` conserva el último gate de pre-merge/pre-deploy.

Los artefactos derivados quedan **CADUCADOS** cuando `Spec-Version` o `Plan-Version` no coinciden con los valores actuales.

## Skills

| Skill | Responsabilidad |
|---|---|
| `sdd-init` | Comprender proyecto y restricciones |
| `sdd-constitution` | Principios innegociables |
| `sdd-agents` | Reglas operativas para agentes |
| `sdd-spec` | QUÉ + POR QUÉ |
| `sdd-clarify` | Auditoría de ambigüedad + aprobación humana |
| `sdd-ui-discovery` | Discovery UX/UI progresivo |
| `frontend-design` | Diseño frontend y patrones UI |
| `sdd-ui` | Contrato UI persistente |
| `sdd-design-system` | Lenguaje visual y tokens |
| `sdd-plan` | Diseño técnico |
| `sdd-migration` | Compatibilidad y transición |
| `sdd-trace` | Trazabilidad |
| `sdd-tasks` | Descomposición |
| `tdd` | RED → GREEN → REFACTOR → VERIFY |
| `sdd-implement` | Implementa UNA tarea |
| `sdd-ui-review` | Auditoría UI estática |
| `sdd-browser-review` | Verificación UI en navegador |
| `sdd-validate` | Validación final de spec |
| `sdd-doc-sync` | Sincronización documental |
| `sdd-release` | READY / BLOCKED |
| `sdd-debug` | Diagnóstico por evidencia |
| `sdd-bug` | Fix con regresión |
| `sdd-change` | Cambios funcionales desde spec |
| `sdd-refactor` | Cambio interno sin comportamiento nuevo |
| `sdd-review` | Auditoría quality / implementation |
| `openjev-decision-gate` | Decisión estructurada opcional; nunca reemplaza routing determinista |

## Flujo completo de una feature nueva

```text
IDEA
 ↓
sdd-init
 ↓
sdd-constitution → aprobación
 ↓
sdd-agents
 ↓
sdd-spec
 ↓
sdd-clarify → aprobación de spec
 ↓
¿UI impact?
 ├─ NO ───────────────────────────────┐
 │                                    │
 └─ SÍ                                │
    ↓                                 │
  sdd-ui-discovery                    │
    ↓                                 │
  frontend-design                     │
    ↓                                 │
  sdd-ui → ui-spec → aprobación UI   │
    ↓                                 │
  sdd-design-system (si aplica)       │
    └─────────────────────────────────┘
                  ↓
              sdd-plan
                  ↓
        ¿Migración/compatibilidad?
          ├─ SÍ → sdd-migration
          └─ NO
                  ↓
              sdd-trace
                  ↓
              sdd-tasks
                  ↓
       sdd-implement T-001
                  ↓
       ... una tarea por ejecución
                  ↓
       Todas las tareas completas
                  ↓
        sdd-ui-review (si UI)
                  ↓
     sdd-browser-review FINAL (si UI)
                  ↓
             sdd-validate
                  ↓
        sdd-doc-sync (si afecta docs)
                  ↓
             sdd-release
                  ↓
                FIN
```

### Regla de UI durante implementación

`sdd-implement` puede invocar una **verificación browser de tarea** para comprobar únicamente el alcance visible de la tarea actual. Esa verificación no sustituye la `sdd-browser-review` final, que valida la feature completa.

## Flujos de mantenimiento

### Bug

```text
sdd-debug (si causa incierta)
 → sdd-bug
 → regression test
 → fix
 → UI/browser checks si aplica
 → fin o sdd-validate si impacta la spec vigente
```

### Cambio funcional

```text
sdd-change
 → sdd-clarify
 → aprobación
 → UI discovery/design si aplica
 → ui-spec si aplica
 → sdd-plan
 → sdd-migration si aplica
 → sdd-trace
 → sdd-tasks
 → sdd-implement
 → reviews
 → validate
 → doc-sync
 → release
```

### Refactor

```text
sdd-refactor
 → tests de protección
 → cambios internos
 → verify
 → UI/browser baseline si afecta UI
```

### Debug

```text
sdd-debug
 → causa confirmada
   ├─ bug → sdd-bug
   ├─ cambio de requisito → sdd-change
   ├─ migración → sdd-migration
   └─ problema externo/config → acción operativa correspondiente
```

## Gates

| Gate | Condición |
|---|---|
| G1 Constitution | `docs/constitution.md` aprobada |
| G2 Spec Clarified | `clarify.md` = `SPEC CLARIFICADA: SÍ` y cero `[NECESITA ACLARACIÓN]` |
| G3 Spec Approval | `spec.md` = `APROBADA` con aprobación humana inequívoca |
| G4 UI Contract | Si `UI impact != NONE`: `ui-spec.md` vigente y, cuando corresponda, aprobada |
| G5 Plan | `plan.md` vigente respecto de `Spec-Version` |
| G6 Migration | Si aplica: `migration-review.md` = suficiente/OK |
| G7 Trace | `trace.md` = `TRAZABILIDAD: PASS` y versiones vigentes |
| G8 Tasks | `tasks.md` vigente; dependencias satisfechas |
| G9 Task Closure | Una tarea completa solo con evidencia de tests/verificaciones y browser task check si aplica |
| G10 UI Review | `ui-review.md` = PASS cuando la UI lo requiere |
| G11 Browser Review | `browser-review.md` = PASS cuando la UI lo requiere |
| G12 Validation | `validation.md` = `SPEC CUMPLIDA: SÍ` |
| G13 Docs | Documentación afectada sincronizada o explícitamente N/A |
| G14 Release | `release.md` = `RELEASE: READY` |

## Artefactos canónicos

```text
docs/brief.md
docs/constitution.md
AGENTS.md
specs/NNN-slug/
├── spec.md
├── clarify.md
├── ui-design-brief.md            # si UI
├── ui-spec.md                   # si UI
├── plan.md
├── migration-review.md          # si migración/compatibilidad
├── trace.md
├── tasks.md
├── ui-review.md                 # si UI
├── browser-review.md            # si UI
├── validation.md
├── doc-sync.md                  # si docs afectadas
└── release.md
```

Cada artefacto derivado incorpora `Spec-Version` y, cuando corresponde, `Plan-Version` y/o su propia versión. `specs-template/` contiene las plantillas de estos artefactos; no es una skill ejecutable.

## Reglas críticas

1. No implementar funcionalidad nueva sin spec aprobada.
2. No resolver ambigüedades en silencio.
3. Una ejecución de `sdd-implement` = una tarea.
4. No introducir arquitectura o dependencias sin plan/justificación.
5. Tests son evidencia: no se aceptan afirmaciones no ejecutadas.
6. Cuando hay UI, el navegador real es evidencia final; código/DOM/snapshots no lo sustituyen.
7. Browser Review por tarea verifica solo el alcance de esa tarea; Browser Review FINAL verifica la feature completa.
8. `sdd-orchestrator` es el router determinista; OpenJEV solo complementa la clasificación.
9. Cambios funcionales pasan por `sdd-change`; bugs pasan por `sdd-bug`; refactors por `sdd-refactor`.
10. Un artefacto con versión incompatible queda CADUCADO.

## Portabilidad

No se asume stack. `sdd-implement`, `sdd-validate` y las reviews descubren comandos reales inspeccionando el repositorio y nunca inventan comandos.

## Instalación

Copia `skills/*` a `~/.pi/agent/skills/`, `~/.agents/skills/` o al directorio de skills del harness compatible con Agent Skills.

Para un repo concreto también puede instalarse bajo `.pi/skills/` o `.agents/skills/`.

## Inicio recomendado

Ejecuta `sdd-orchestrator` ante cualquier duda sobre el estado o el próximo paso.
