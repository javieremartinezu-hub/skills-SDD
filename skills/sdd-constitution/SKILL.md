---
name: sdd-constitution
description: Crea docs/constitution.md, la constitución del proyecto con 8-12 principios innegociables, cortos y verificables (SDD, simplicidad, calidad, seguridad por diseño, tests, dependencias, errores, observabilidad, cambios controlados). Propone, muestra y exige aprobación humana explícita antes de considerarlas vigentes. Úsala tras sdd-init, cuando exista docs/brief.md y no haya constitution aprobada, o cuando el usuario pida crear o revisar la constitución del proyecto.
---

# sdd-constitution — Constitución del proyecto

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Estado de la constitution: `PROPUESTA` → `APROBADA` (solo por aprobación humana explícita).
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Definir las reglas innegociables del proyecto en `docs/constitution.md`. Todo el trabajo posterior (specs, planes, código, tests) se validará contra estas reglas.

## Alcance

- SOLO redacta principios de gobierno del proyecto.
- NO escribe código ni tests. NO crea specs. NO define arquitectura concreta (los principios son tecnológicamente neutros).

## Cuándo usar

- Tras `sdd-init`, cuando exista `docs/brief.md` y no haya constitution aprobada.
- Cuando el usuario pida crear, revisar o enmendar la constitución.

## Precondiciones

- `docs/brief.md` existe (si no: `CONSTITUCIÓN BLOQUEADA — Falta docs/brief.md. Siguiente paso: sdd-init`).
- Si `docs/constitution.md` ya tiene `Estado: APROBADA`, este flujo es una **enmienda**: conserva todos los principios no afectados, numera la revisión, cambia temporalmente el estado a `PROPUESTA` y exige la misma aprobación explícita antes de que la nueva versión entre en vigor. No bloquees ni sobrescribas silenciosamente la constitution existente.

## Contexto requerido

- `docs/brief.md`
- Respuestas del usuario durante la propuesta.

## Entradas

- Restricciones y valores del proyecto que el usuario quiera elevar a principio.

## Procedimiento

1. Lee `docs/brief.md` y, si existe, `docs/constitution.md`. Si no existe el brief, detente (ver Precondiciones). Si hay una constitution aprobada, identifica primero sus principios y registro de enmiendas: conserva el contenido no afectado y prepara únicamente la revisión solicitada; no redactes una constitution nueva desde cero.
2. Para una constitution nueva, redacta **entre 8 y 12 principios**. Para una enmienda, conserva entre 8 y 12 principios tras aplicar solo los cambios solicitados. Cada principio: enunciado en una frase + "Cómo se verifica" en una frase. Deben poder comprobarse en una revisión de código o de artefactos.
3. Cubre como mínimo estos temas (pueden agruparse): Spec-Driven Development · simplicidad · mantenibilidad · calidad de código · seguridad por diseño · tests · dependencias · separación de responsabilidades · errores · observabilidad · compatibilidad · documentación · cambios controlados.
4. Incluye, con esta esencia, estas reglas innegociables:
   - La spec manda sobre el código.
   - Ningún comportamiento existe sin requisito.
   - No se integra trabajo con tests fallando.
   - Las dependencias deben ser mínimas y justificadas.
   - La seguridad se aplica por defecto, no se añade después.
   - Los errores deben ser explícitos y observables, nunca tragados en silencio.
   - Los cambios deben ser pequeños y verificables.
   - Spec, plan, código y tests deben permanecer sincronizados.
5. Escribe `docs/constitution.md` con `Estado: PROPUESTA`. Para una enmienda, incrementa una revisión o versión visible y añade al registro una fila marcada `PENDIENTE`; conserva el historial previo.
6. Muestra la constitución completa y el diff de la enmienda al usuario.
7. **Gate de aprobación** (ver reglas abajo): pide aprobación explícita. 
   - Si aprueba: actualiza la cabecera a `Estado: APROBADA (YYYY-MM-DD, aprobada por el usuario)` y recomienda `Siguiente paso: sdd-agents`.
   - Si pide cambios: aplica solo los cambios pedidos, vuelve al paso 6.
   - Si no responde inequívocamente: deja `Estado: PROPUESTA`, `CONSTITUCIÓN NO APROBADA. Siguiente paso: repetir la aprobación aquí.`

### Reglas del gate de aprobación

- Pregunta literalmente: "¿Apruebas esta constitución como reglas innegociables del proyecto? (sí/no)".
- CUENTA como aprobación: un sí inequívoco referido a la constitución mostrada ("sí, apruebo", "aprobada", "adelante con esta constitución").
- NO CUENTA: "vale", "ok", "sigue", "suena bien", silencio, aprobar otra cosa, o responder otra pregunta. Ante ambigüedad, repite la pregunta.
- Nunca infieras aprobación. Nunca auto-apruebes.

### Plantilla de `docs/constitution.md`

```markdown
# Constitución del proyecto

> Estado: PROPUESTA

Los principios de esta constitución son innegociables. Toda spec, plan,
tarea, código y test se valida contra ellos.

## Principios

### P-01 — <Nombre corto>
<Enunciado en una frase.>
Verificación: <cómo se comprueba en la práctica>

### P-02 — …
(repetir hasta 8–12 principios)

## Registro de enmiendas
| Fecha | Cambio | Aprobada por |
```

## Artefactos de salida

- `docs/constitution.md`

## Validación

- Entre 8 y 12 principios; cada uno con enunciado + verificación.
- Todos los temas mínimos del paso 3 están cubiertos.
- Ningún principio contradice a otro ni impone una tecnología concreta salvo que el usuario la haya impuesto como restricción.
- `Estado: APROBADA` solo tras aprobación explícita registrada.
- Una enmienda conserva los principios e historial no afectados, incrementa su revisión y nunca entra en vigor mientras esté `PROPUESTA`.

## Condiciones de parada

- Tras mostrar la propuesta: DETENTE esperando aprobación. No continúes a ninguna otra fase sin ella.
- Tras registrar la aprobación: recomienda `sdd-agents` y DETENTE.

## Acciones prohibidas

- Considerar aprobada la constitución de forma implícita o por defecto.
- Escribir código, tests o specs.
- Añadir principios que el usuario no haya pedido ni validado con contenido inventado.

## Siguiente fase permitida

`sdd-agents` (solo con `Estado: APROBADA`)
