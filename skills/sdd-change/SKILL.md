---
name: sdd-change
description: Gestiona cambios funcionales mediante Spec-Anchored Development. Ante "añade MFA", "cambia el comportamiento de X" o "nueva regla", identifica la spec afectada, analiza el impacto y modifica PRIMERO spec.md (RF, errores, estados, permisos, seguridad, casos límite, fuera de alcance, criterios, registro de cambios), muestra el diff y pide aprobación explícita. PROHIBIDO modificar código antes de que la spec cambie y se re-apruebe. Después el ciclo se reinicia (clarify → plan → trace → tasks → implement → validate).
---

# sdd-change — Cambio funcional desde la spec

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA`. Marcador: `[NECESITA ACLARACIÓN: ...]`
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Incorporar un requisito nuevo o modificado **empezando siempre por la spec**: ningún cambio funcional comienza modificando código.

## Alcance

- SOLO modifica `spec.md`. Los artefactos derivados existentes (`clarify.md`, `plan.md`, `trace.md`, `tasks.md`, `validation.md`) no se editan: pasan a estar caducados al quedar vinculados a una versión anterior.
- NO implementa código ni registra aprobaciones. El ciclo posterior regenerará los artefactos derivados.

## Cuándo usar

- "Añade MFA", "cambia el comportamiento de X", "agrega una nueva regla", o cualquier cambio de comportamiento de un sistema que ya tiene spec — esté la spec completada o en curso.

## Precondiciones

- Existe al menos una spec en `specs/`. Si no: `CAMBIO BLOQUEADO — No existe ninguna spec. Siguiente paso: sdd-spec`.

## Contexto requerido

- `specs/` completa (para localizar la spec afectada) y `docs/constitution.md`.

## Entradas

- La petición de cambio, en lenguaje natural.

## Procedimiento

1. **Identifica la spec afectada** buscando por RF, secciones y tema. Si el cambio no corresponde a ninguna spec existente (funcionalidad genuinamente nueva), no la crees ni la entrevistes aquí: `CAMBIO BLOQUEADO — La funcionalidad requiere una spec nueva. Siguiente paso: sdd-spec`.
2. **Analiza y clasifica el impacto**:
   - `LOCAL`: cambia comportamiento dentro del diseño existente; no altera contratos/APIs públicas, persistencia, permisos/seguridad, RNF, arquitectura ni otras specs.
   - `ESTRUCTURAL`: afecta cualquiera de esas áreas o cruza varios componentes/specs.
   Identifica RF, tareas, tests y secciones afectadas. Presenta solo un resumen breve antes de editar.
3. **Modifica spec.md PRIMERO**: aplica los cambios de requisitos con IDs estables (los RF modificados conservan su ID; los nuevos continúan la numeración; los eliminados se marcan como `RF-XXX: (eliminado en v<N> — motivo)` para conservar la trazabilidad histórica).
4. Revisa por coherencia, actualizando lo afectado: errores · estados · permisos · seguridad funcional · casos límite · fuera de alcance · criterios de finalización.
5. **Versiona e invalida derivaciones**: incrementa la fila `Versión`, restablece las filas `Estado` a `BORRADOR` y `Aprobación` a `PENDIENTE`, y añade una fila al `Registro de cambios` incluyendo la clasificación persistente `[LOCAL]` o `[ESTRUCTURAL]`, qué cambió y por qué. No modifiques los artefactos derivados: sus cabeceras conservan la versión anterior y el orquestador los tratará como caducados.
6. Lo que no se pueda cerrar sin decisión del usuario queda como `[NECESITA ACLARACIÓN: ...]`.
7. **Muestra los cambios** con un diff resumido y la clasificación `LOCAL|ESTRUCTURAL`. No solicites ni registres una aprobación en esta skill: la aprobación de la nueva versión ocurre tras `sdd-clarify`.
8. **DETENTE** e informa el reinicio del ciclo:

```
CAMBIO REGISTRADO EN LA SPEC (v<N>)
Siguiente paso: sdd-clarify
Después: aprobación → [UI discovery/design/spec si aplica] → sdd-plan → [sdd-migration] → sdd-trace → sdd-tasks → sdd-implement → [UI/browser reviews] → sdd-validate → [sdd-doc-sync] → sdd-release
Nota: si el cambio es LOCAL, `sdd-plan` debe usar revisión de compatibilidad/delta y `sdd-tasks` reabrir solo tareas afectadas.
```

### Principio innegociable

NINGÚN CAMBIO FUNCIONAL COMIENZA MODIFICANDO CÓDIGO. Si el usuario insiste en tocar código primero, bloquea: `CAMBIO BLOQUEADO — La spec debe cambiar y aprobarse antes que el código. Siguiente paso: completar este flujo`.

## Artefactos de salida

- `specs/NNN-slug/spec.md` modificada: nueva versión, fila `Estado` en `BORRADOR` y fila `Aprobación` en `PENDIENTE`.
- Los artefactos derivados no se modifican; quedan CADUCADOS al cambiar `Spec-Version`, incluidos `ui-design-brief.md`, `ui-spec.md`, `plan.md`, `migration-review.md`, `trace.md`, `tasks.md`, `ui-review.md`, `browser-review.md`, `validation.md`, `doc-sync.md` y `release.md` cuando existan.

## Validación

- Ningún archivo distinto de `spec.md` fue modificado en esta skill.
- El `Registro de cambios` documenta la versión nueva y conserva la clasificación `[LOCAL]` o `[ESTRUCTURAL]` para que las siguientes skills no dependan de la conversación.
- Los RF modificados conservan IDs; los nuevos continúan la numeración.
- Se revisaron las 8 secciones del paso 4 (aunque sea para concluir "sin cambios").
- La nueva versión deja sin vigencia plan, trazabilidad, tareas y validación anteriores; el orquestador no permite usarlos hasta regenerarlos.

## Condiciones de parada

- Tras mostrar los cambios y resolver el gate: DETENTE. El ciclo continúa con `sdd-clarify`, no contigo.

## Acciones prohibidas

- Modificar, crear o eliminar código, tests, plan.md o tasks.md.
- Registrar o inferir aprobación: el gate pertenece exclusivamente a `sdd-clarify`.
- Saltarte la re-auditoría (`sdd-clarify`) o la re-trazabilidad (`sdd-trace`) posteriores.
- Resolver ambigüedades del cambio sin el usuario.

## Siguiente fase permitida

`sdd-clarify` (reinicio del ciclo)

## UI delta

Si el cambio es visible, actualizar la UI Spec antes de implementar:
- pantalla/flujo;
- layout;
- componentes;
- estados;
- tokens;
- responsive;
- accessibility;
- motion;
- browser acceptance criteria.
