---
name: sdd-init
description: >-
  Inicializa un proyecto bajo Spec-Driven Development, nuevo o ya existente. Entrevista al usuario
  por bloques cortos para entender problema, usuarios, objetivos, funcionalidades, restricciones y
  límites; en repos con código inspecciona primero lo existente. Persiste docs/brief.md. No elige
  stack, no escribe código ni specs. Úsala al arrancar SDD en un repositorio que aún no tiene
  docs/brief.md.
---

# sdd-init — Inicialización del proyecto

Formato de bloqueo: `<ACCIÓN> BLOQUEADA — Motivo — Falta — Siguiente paso: <skill>`.

## Propósito

Dejar en `docs/brief.md` el entendimiento del proyecto antes de cualquier otro artefacto SDD.

## Precondiciones

- Si existe `docs/brief.md` → `INICIALIZACIÓN BLOQUEADA — ya existe un brief. Siguiente paso: sdd-constitution` (o `sdd-orchestrator` si también hay constitution). Nunca sobrescribas.

## Procedimiento

1. **Repo existente.** Si hay código, inspecciona manifiestos, estructura de carpetas y README antes de preguntar. Resume stack, módulos y convenciones detectadas; el stack existente se registra como *restricción existente* tras confirmarlo el usuario. No preguntes lo que el repo ya responde.
2. **Entrevista por bloques** de 3-5 preguntas relacionadas, sin listas exhaustivas. Cubre: problema · usuarios y actores · objetivos y cómo se sabrá que funcionó · funcionalidades imaginadas · restricciones (regulatorias, integración, plataforma, técnicas impuestas) · fuera de alcance.
3. Clasifica cada respuesta como **requisito de producto** o **decisión de implementación**. Una decisión técnica no impuesta como restricción va a "Decisiones abiertas", nunca a requisitos.
4. Cuando puedas explicar problema, usuarios, objetivo y restricciones sin inventar nada, escribe `docs/brief.md` y pide confirmación.
5. `Siguiente paso: sdd-constitution`. DETENTE.

## Plantilla de `docs/brief.md`

```markdown
# Brief del proyecto

> Estado: BORRADOR (contexto de partida; no es una especificación)

## Problema
## Usuarios y actores
## Objetivos y resultado esperado
## Funcionalidades imaginadas (tentativas)
## Restricciones (impuestas o existentes en el repo)
## Estado actual del repositorio (solo si ya hay código)
## Decisiones abiertas
## Fuera de alcance (preliminar)
```

## Validación

- Nada inventado: todo procede del usuario o del repositorio, o está en "Decisiones abiertas".
- Sin elección de stack salvo restricción impuesta o existente confirmada.
- El usuario confirmó el brief.

## Prohibido

Elegir o sugerir tecnologías, escribir código o tests, crear specs, constitution o `AGENTS.md`, inventar requisitos.
