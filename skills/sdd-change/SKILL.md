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

- SOLO modifica `spec.md` (y prepara el reinicio del ciclo).
- NO implementa código. NO actualiza plan.md ni tasks.md (les toca tras el ciclo: `sdd-plan`, `sdd-tasks`).

## Cuándo usar

- "Añade MFA", "cambia el comportamiento de X", "agrega una nueva regla", o cualquier cambio de comportamiento de un sistema que ya tiene spec — esté la spec completada o en curso.

## Precondiciones

- Existe al menos una spec en `specs/`. Si no: `CAMBIO BLOQUEADO — No existe ninguna spec. Siguiente paso: sdd-spec`.

## Contexto requerido

- `specs/` completa (para localizar la spec afectada) y `docs/constitution.md`.

## Entradas

- La petición de cambio, en lenguaje natural.

## Procedimiento

1. **Identifica la spec afectada** buscando por RF, secciones y tema. Si el cambio no corresponde a ninguna spec existente (funcionalidad genuinamente nueva), créala siguiendo las convenciones de `sdd-spec` (nueva carpeta `NNN-slug/` con el siguiente número libre) y continúa.
2. **Analiza el impacto**: RF afectados (añadidos/modificados/eliminados), tareas y tests existentes que quedan obsoletos, secciones de la spec colaterales. Preséntalo antes de editar.
3. **Modifica spec.md PRIMERO**: aplica los cambios de requisitos con IDs estables (los RF modificados conservan su ID; los nuevos continúan la numeración; los eliminados se marcan como `RF-XXX: (eliminado en v<N> — motivo)` para conservar la trazabilidad histórica).
4. Revisa por coherencia, actualizando lo afectado: errores · estados · permisos · seguridad funcional · casos límite · fuera de alcance · criterios de finalización.
5. **Versiona e invalida veredictos**: incrementa `Versión`, resetea `Estado: BORRADOR` y `Aprobación: PENDIENTE`, y añade fila a `Registro de cambios` (qué cambió y por qué petición). Además, ANULA los veredictos previos para re-bloquear el ciclo: en `clarify.md` escribe `SPEC CLARIFICADA: NO (caducado por cambio v<N>)` y en `trace.md` escribe `TRAZABILIDAD: FAIL (caducado por cambio v<N>)`. Sin este paso, plan y tasks quedarían desbloqueados con auditorías de una versión antigua de la spec.
6. Lo que no se pueda cerrar sin decisión del usuario queda como `[NECESITA ACLARACIÓN: ...]`.
7. **Muestra los cambios** (diff resumido: qué se añadió, modificó o eliminó, sección por sección).
8. **Gate de aprobación.** Pregunta literalmente: "¿Apruebas estos cambios en la especificación? (sí/no)".
   - CUENTA: sí inequívoco referido a estos cambios. NO CUENTA: "vale", "ok", "sigue", silencio, ambigüedad (repite la pregunta).
   - Si aprueba: registra `Aprobación: APROBADA (YYYY-MM-DD, por el usuario)` solo si además NO queda ningún `[NECESITA ACLARACIÓN`; si quedan marcadores, permanece PENDIENTE.
9. **DETENTE** e informa el reinicio del ciclo:

```
CAMBIO REGISTRADO EN LA SPEC (v<N>)
Siguiente paso: sdd-clarify
Después: aprobación → sdd-plan → sdd-trace → sdd-tasks → sdd-implement → sdd-validate
```

### Principio innegociable

NINGÚN CAMBIO FUNCIONAL COMIENZA MODIFICANDO CÓDIGO. Si el usuario insiste en tocar código primero, bloquea: `CAMBIO BLOQUEADO — La spec debe cambiar y aprobarse antes que el código. Siguiente paso: completar este flujo`.

## Artefactos de salida

- `specs/NNN-slug/spec.md` modificada (nueva versión, `Estado: BORRADOR`, `Aprobación: PENDIENTE` o registrada).
- `specs/NNN-slug/clarify.md` y `specs/NNN-slug/trace.md` con veredictos anulados (`NO (caducado…)` / `FAIL (caducado…)`): `sdd-clarify` y `sdd-trace` los regenerarán.

## Validación

- Ningún archivo de código, `plan.md` o `tasks.md` fue modificado en esta skill.
- El `Registro de cambios` documenta la versión nueva.
- Los RF modificados conservan IDs; los nuevos continúan la numeración.
- Se revisaron las 8 secciones del paso 4 (aunque sea para concluir "sin cambios").
- Los veredictos de `clarify.md` y `trace.md` quedaron anulados; nadie puede avanzar a plan o tasks con auditorías de una versión anterior de la spec.

## Condiciones de parada

- Tras mostrar los cambios y resolver el gate: DETENTE. El ciclo continúa con `sdd-clarify`, no contigo.

## Acciones prohibidas

- Modificar, crear o eliminar código, tests, plan.md o tasks.md.
- Aprobar tus propios cambios.
- Saltarte la re-auditoría (`sdd-clarify`) o la re-trazabilidad (`sdd-trace`) posteriores.
- Resolver ambigüedades del cambio sin el usuario.

## Siguiente fase permitida

`sdd-clarify` (reinicio del ciclo)
