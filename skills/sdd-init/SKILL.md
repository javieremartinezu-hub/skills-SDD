---
name: sdd-init
description: Inicializa un proyecto bajo Spec-Driven Development. Entrevista al usuario (UNA pregunta cada vez) para comprender problema, usuarios, objetivos, resultado esperado, restricciones y funcionalidades imaginadas, y persiste el contexto en docs/brief.md. No elige stack, no escribe código y no crea la spec. Úsala al arrancar un proyecto nuevo o cuando el repositorio no tenga docs/constitution.md ni contexto SDD previo.
---

# sdd-init — Inicialización SDD del proyecto

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Estado de una spec: `BORRADOR` → `CLARIFICADA` → `APROBADA`. Marcador: `[NECESITA ACLARACIÓN: ...]`
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Comprender qué proyecto se quiere construir ANTES de crear cualquier otro artefacto SDD, y dejar ese entendimiento persistido en `docs/brief.md`.

## Alcance

- SOLO entiende, pregunta y registra contexto.
- NO crea specs, ni constitution, ni código. NO elige lenguaje, framework, base de datos, ORM, librerías, arquitectura, cloud ni infraestructura — salvo que el usuario los presente explícitamente como **restricción**.

## Cuándo usar

- Proyecto nuevo sin inicializar SDD (no existe `docs/constitution.md`).
- `sdd-orchestrator` enruta aquí.

## Precondiciones

- No debe existir una spec en curso. Si ya existen artefactos SDD, enruta a `sdd-orchestrator` en lugar de reinicializar.

## Contexto requerido

- El repositorio (puede estar vacío) y la idea inicial del usuario.

## Entradas

- Descripción libre de la idea, por incompleta que sea.

## Procedimiento

1. Comprueba que no existan `docs/brief.md`, `docs/constitution.md` ni `specs/`. Si existe cualquiera, detente sin sobrescribir nada: si solo existe `docs/brief.md`, `INICIALIZACIÓN BLOQUEADA — ya existe un brief. Siguiente paso: sdd-constitution`; en cualquier otro caso, `INICIALIZACIÓN BLOQUEADA — el proyecto ya está inicializado. Siguiente paso: sdd-orchestrator`.
2. **Entrevista con UNA pregunta cada vez.** No hagas listas de preguntas. Cubre, como mínimo:
   - ¿Qué problema se quiere resolver?
   - ¿Quiénes son los usuarios y actores?
   - ¿Qué objetivos tiene el proyecto y cómo se sabrá que funcionó (resultado esperado)?
   - ¿Qué funcionalidades se imaginan inicialmente?
   - ¿Qué restricciones existen (regulatorias, de integración, de plataforma, técnicas impuestas)?
   - ¿Qué queda explícitamente fuera?
3. En cada respuesta clasifica y anota en dos columnas mentales:
   - **REQUISITO DEL PRODUCTO**: qué debe hacer el sistema y por qué.
   - **DECISIÓN DE IMPLEMENTACIÓN**: cómo se construirá. Si aparece una y NO fue impuesta como restricción, no la registres como requisito; anótala en "Decisiones aún abiertas".
4. Cuando tengas suficiente comprensión (puedes explicar problema, usuarios, objetivo y restricciones sin inventar nada), escribe `docs/brief.md` con la plantilla y muéstraselo al usuario para confirmación.
5. Recomienda: `Siguiente paso: sdd-constitution`.

### Plantilla de `docs/brief.md`

```markdown
# Brief del proyecto

> Estado: BORRADOR (contexto de partida; NO es una especificación)

## Problema
## Usuarios y actores
## Objetivos y resultado esperado
## Funcionalidades imaginadas inicialmente (tentativas, sin diseño)
## Restricciones impuestas (incluye restricciones técnicas si el usuario las impone)
## Requisitos del producto vs decisiones de implementación
| Tema | ¿Es requisito de producto o decisión de implementación? |
## Decisiones aún abiertas
## Fuera de alcance (preliminar)
```

## Artefactos de salida

- `docs/brief.md`

## Validación

- `docs/brief.md` no contiene elección de stack salvo restricción impuesta explícita.
- Ningún enunciado del brief fue inventado por el agente: todo procede de respuestas del usuario o está marcado `Decisión aún abierta`.
- El usuario confirmó el contenido del brief.

## Condiciones de parada

- Si el usuario no puede responder algo esencial, regístralo en "Decisiones aún abiertas" y detente tras escribir el brief. No rellenes huecos con suposiciones.
- Tras escribir el brief y recomendar `sdd-constitution`: DETENTE.

## Acciones prohibidas

- Elegir o sugerir lenguaje, framework, BD, ORM, librerías, arquitectura o infraestructura no impuestos como restricción.
- Escribir código o tests.
- Crear `specs/`, `docs/constitution.md` o `AGENTS.md`.
- Inventar requisitos o usuarios.

## Siguiente fase permitida

`sdd-constitution`
