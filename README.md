# SDD Framework — Skills reutilizables de Spec-Driven Development

Framework genérico, portable y agnóstico de stack que convierte **Spec-Driven Development (SDD)** en el sistema operativo de agentes de IA para cualquier proyecto de software: frontend, backend, full-stack, CLI, APIs, móvil, sistemas distribuidos, librerías, herramientas internas o infraestructura.

**Enfoque metodológico: SPEC-ANCHORED + TDD**

```
SPEC   = QUÉ + POR QUÉ
PLAN   = CÓMO
TASKS  = EJECUCIÓN
TESTS  = EVIDENCIA
```

Cadena de trazabilidad obligatoria:

```
REQUISITO → PLAN → TAREA → CÓDIGO → TEST
```

---

## Qué es Spec-Driven Development y qué significa "Spec-Anchored"

**Spec-Driven Development** significa que ninguna línea de código de producto nace de una idea: nace de una especificación funcional aprobada. La conversación con el agente NO es fuente de verdad; los artefactos persistentes sí.

**Spec-Anchored** significa que la spec es el ancla de todo el ciclo de vida: cada cambio funcional empieza modificando la spec; cada tarea referencia requisitos; cada test demuestra un requisito; la validación final recorre requisito por requisito. Si la spec y el código divergen, gana la spec y se corrige el código.

El framework impide que un agente:

- programe antes de tener una spec;
- invente requisitos;
- mezcle requisitos con implementación;
- salte de la idea al código;
- introduzca decisiones arquitectónicas en silencio;
- implemente varias tareas de una vez;
- declare "terminado" sin evidencia;
- dé por terminado un componente visual, interfaz o flujo web sin verificarlo en navegador real (inspección, E2E, evidencia visual e iteración);
- atienda un requisito nuevo tocando código directamente.

---

## Skills

| Skill | Responsabilidad |
|-------|-----------------|
| sdd-init | Comprender el proyecto |
| sdd-constitution | Crear principios |
| sdd-agents | Crear reglas para agentes |
| sdd-spec | Definir QUÉ + POR QUÉ |
| sdd-clarify | Eliminar ambigüedad |
| sdd-plan | Definir CÓMO |
| sdd-trace | Verificar RF → plan |
| sdd-tasks | Descomponer trabajo |
| sdd-implement | Implementar una tarea con TDD |
| sdd-validate | Probar cumplimiento |
| sdd-change | Gestionar cambios desde la spec |
| sdd-orchestrator | Gobernar el flujo |
| sdd-bug | Corregir bug confirmado con test de regresión |
| sdd-debug | Diagnosticar causa raíz incierta |
| sdd-migration | Revisar migraciones y compatibilidad |
| sdd-refactor | Mejorar estructura sin cambiar comportamiento |
| sdd-review | Auditoría de solo lectura (quality / implementation / security / dependencies) |
| sdd-doc-sync | Sincronizar documentación afectada |
| sdd-release | Gate pre-merge / pre-deploy (READY / BLOCKED) |
| tdd | Workflow TDD pragmático para cambios de código |
| sdd-ui | Definir la UI/UX como contrato SDD persistente (si hay UI) |
| sdd-design-system | Definir/mantener el lenguaje visual y tokens compartidos |
| sdd-ui-review | Revisión estática de la implementación UI contra UI Spec y Design System |
| sdd-browser-review | Verificación en navegador real contra la UI Spec y flujos de aceptación |
| openjev-decision-gate | Capa de decisión estructurada para seleccionar flujo y gates SDD/TDD |

Cada `SKILL.md` es autocontenido y ejecutable sin conocer esta conversación, y define: propósito, alcance, cuándo usar, precondiciones, contexto requerido, entradas, procedimiento, artefactos, validación, condiciones de parada, acciones prohibidas y siguiente fase permitida.

---

## Flujo completo

### Proyecto nuevo

```
IDEA
 → sdd-init            (comprender: docs/brief.md)
 → sdd-constitution    (principios: docs/constitution.md)
 → sdd-agents          (reglas: AGENTS.md)
 → sdd-spec            (QUÉ+POR QUÉ: specs/NNN-slug/spec.md)
 → sdd-clarify         (auditoría QA + GATE de aprobación humana)
 → sdd-ui              (si UI: specs/NNN-slug/ui-spec.md)
 → sdd-design-system   (si aplica: lenguaje visual y tokens)
 → sdd-plan            (CÓMO: plan.md)
 → sdd-trace           (RF → plan: trace.md · PASS obligatorio)
 → sdd-tasks           (descomposición: tasks.md)
 → sdd-implement T-001 → verificación
 → sdd-implement T-002 → verificación
 → …
 → sdd-browser-review  (si UI: verificación en navegador real contra ui-spec)
 → sdd-validate        (evidencia RF→TEST: validation.md)
 → SPEC COMPLETADA
```

### Cambio funcional posterior

```
NUEVO REQUISITO
 → sdd-change          (modifica y versiona spec.md + aprobación)
 → sdd-clarify         (re-auditoría + aprobación)
 → sdd-ui              (si aplica: actualiza ui-spec.md)
 → sdd-plan            (actualiza diseño)
 → sdd-trace           (re-verifica cobertura)
 → sdd-tasks           (reabre/añade tareas)
 → sdd-implement T-XXX (una tarea por ejecución)
 → sdd-browser-review  (si UI: verificación en navegador real)
 → sdd-validate
```

Nunca existen `IDEA → CÓDIGO` ni `CHANGE → CÓDIGO`.

**Caducidad de veredictos:** `sdd-change` anula los veredictos previos (`SPEC CLARIFICADA: NO (caducado…)` y `TRAZABILIDAD: FAIL (caducado…)`), de modo que el cambio re-bloquea plan y tasks hasta pasar de nuevo por `sdd-clarify` y `sdd-trace`.

---

## Gates que bloquean el avance

| Gate | Condición para pasar | Quién lo aplica |
|------|----------------------|-----------------|
| G1 · Constitution | `docs/constitution.md` con `Estado: APROBADA` | sdd-spec, sdd-plan, sdd-implement |
| G2 · Clarificación | `SPEC CLARIFICADA: SÍ` y cero `[NECESITA ACLARACIÓN]` | sdd-plan |
| G3 · Aprobación humana | `Aprobación: APROBADA (fecha, por el usuario)` con sí inequívoco | sdd-clarify, sdd-plan, sdd-implement |
| G4 · Trazabilidad | `TRAZABILIDAD: PASS` (todo RF `COVERED`) | sdd-tasks, sdd-implement |
| G5 · Dependencias de tarea | Tarea existe y todas sus dependencias `- [x]` con evidencia | sdd-implement |
| G6 · Cierre de tarea | Todas las verificaciones del proyecto pasan; para cambios de UI, verificación en navegador real con evidencia; si no, la tarea sigue `- [ ]` | sdd-implement |
| G7 · Validación final | Evidencia completa RF→TEST para todos los requisitos (para RF de UI, E2E en navegador real con evidencia visual); si no, `SPEC CUMPLIDA: NO` | sdd-validate |

Además: `sdd-orchestrator` bloquea cualquier acción cuya fase previa no esté completa y devuelve siempre la skill correcta.

Una aprobación solo cuenta si es inequívoca y referida al artefacto mostrado ("sí, apruebo esta spec"). "Vale", "ok", "sigue", "suena bien", el silencio o cambiar de tema NO son aprobación: se vuelve a preguntar.

---

## Acciones prohibidas (resumen global)

1. Implementar código fuera de `sdd-implement`.
2. Implementar sin spec aprobada, sin clarificar o sin trazabilidad PASS.
3. Implementar más de una tarea por ejecución o encadenar tareas automáticamente.
4. Inventar o resolver en silencio requisitos; usar `[NECESITA ACLARACIÓN: ...]` ante la duda.
5. Modificar código ante un cambio funcional (`sdd-change` primero, siempre).
6. Introducir decisiones arquitectónicas o dependencias sin justificación explícita.
7. Marcar tareas o specs como completadas sin evidencia real ("parece funcionar" no es evidencia).
8. Saltarse gates o reinterpretarlos (`sdd-orchestrator` lo impide).
9. Inventar comandos de verificación inexistentes: se descubren inspeccionando cada repositorio.

---

## Artefactos y estado persistente

La conversación del agente no es fuente permanente de verdad. Los artefactos lo son, y **el estado del workflow se deriva de ellos** (no existe un archivo de estado paralelo que pueda desincronizarse):

| Artefacto | Contiene | Estado que aporta |
|-----------|----------|-------------------|
| `docs/brief.md` | Contexto del proyecto | Proyecto comprendido |
| `docs/constitution.md` | 8-12 principios innegociables | `Estado: PROPUESTA/APROBADA` |
| `AGENTS.md` | Manual operativo para agentes | SDD activo en el repo |
| `specs/NNN-slug/spec.md` | QUÉ + POR QUÉ, con RF-xxx | `Estado: BORRADOR/CLARIFICADA/APROBADA` · `Aprobación` · `Versión` · `Registro de cambios` |
| `specs/NNN-slug/clarify.md` | Auditoría QA + veredicto | `SPEC CLARIFICADA: SÍ/NO` |
| `specs/NNN-slug/plan.md` | CÓMO, decisiones D-NNN | Diseño técnico |
| `specs/NNN-slug/trace.md` | Matriz RF→plan | `TRAZABILIDAD: PASS/FAIL` |
| `specs/NNN-slug/tasks.md` | Tareas T-xxx con checklist | `- [ ]`/`- [x]` + `Evidencia:` |
| `specs/NNN-slug/validation.md` | Evidencia RF→TEST | `SPEC CUMPLIDA: SÍ/NO` |

Cada skill **vuelve a leer** los documentos relevantes antes de actuar; nunca confía en información recordada de conversaciones anteriores.

### Nota de diseño: no se creó `.sdd/state.yaml`

Se evaluó un mecanismo de estado persistente (`.sdd/state.yaml` con `active_spec`, `current_phase`, etc.) y se descartó: todos esos campos son derivables de los artefactos (`spec.md` cabeceras, veredictos en `clarify.md`/`trace.md`/`validation.md`, checkboxes de `tasks.md`). Un archivo de estado paralelo sería una segunda fuente de verdad que puede desincronizarse — justo lo que el prohíbe el principio 19. Si en el futuro se necesita, debe seguir siendo derivable y nunca contener requisitos.

---

## Portabilidad

El framework no asume ningún lenguaje, framework, base de datos, ORM, runtime, cloud ni herramienta:

- `sdd-spec` solo habla de comportamiento observable (EARS, RF-xxx), jamás de tecnología.
- `sdd-plan` es el único punto donde se elige stack, siempre justificado con registros de decisión y contra las restricciones de producto.
- `sdd-implement` y `sdd-validate` **descubren** las verificaciones reales inspeccionando el repositorio anfitrión (manifiestos, scripts, CI) y nunca inventan comandos.

Instrucciones compartidas: cada skill es autocontenida; las reglas transversales viven en `docs/constitution.md` y `AGENTS.md`, que todas las skills obligan a leer.

---

## Instalación (formato Agent Skills / pi)

Cada carpeta bajo `skills/` contiene un `SKILL.md` con frontmatter `name` + `description` (especificación Agent Skills).

- **Global (pi):** copia las carpetas a `~/.pi/agent/skills/` (o `~/.agents/skills/`).
- **Por proyecto:** copia las carpetas a `<repo>/.pi/skills/` (o `<repo>/.agents/skills/`).
- **Otros harness compatibles con Agent Skills:** copia `skills/*` a su directorio de skills y, si procede, decláralo en su configuración.

Recomendación: copia **las 25**. El framework asume que `sdd-orchestrator` está presente para gobernar los gates; las funcionalidades con interfaz web añaden `sdd-ui`, `sdd-design-system`, `sdd-ui-review` y `sdd-browser-review`, y `openjev-decision-gate` complementa la selección de flujo. Las plantillas de `specs-template/` documentan el formato de `ui-spec.md` y `browser-review.md`.

## Cómo empezar en un proyecto

1. `sdd-init` → entrevista y `docs/brief.md`.
2. `sdd-constitution` → aprueba los principios.
3. `sdd-agents` → genera `AGENTS.md`.
4. `sdd-spec` → primera funcionalidad.
5. Sigue los veredictos; ante cualquier duda: `sdd-orchestrator` te dice el único paso válido.
