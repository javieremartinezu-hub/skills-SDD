---
name: sdd-agents
description: >-
  Genera AGENTS.md en la raíz del repositorio: el manual operativo breve y permanente para agentes de IA que trabajen en el proyecto (leer constitution y spec activa, trabajar una sola tarea, tests como evidencia, no avanzar con errores, reportar qué cambió y detenerse). No contiene requisitos de producto ni decisiones técnicas. Úsala tras aprobar docs/constitution.md, o cuando el usuario pida crear o actualizar las reglas para agentes del repositorio.
---

# sdd-agents — Manual operativo para agentes de IA

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Crear `AGENTS.md` en la raíz del repositorio: el manual operativo permanente que obliga a cualquier agente de IA a trabajar dentro del flujo SDD.

## Alcance

- SOLO redacta reglas de comportamiento para agentes.
- NO contiene requisitos del producto (eso es `spec.md`), ni decisiones técnicas (eso es `plan.md`), ni duplica íntegramente `docs/constitution.md` (la referencia; no la copies).

## Cuándo usar

- Tras aprobar `docs/constitution.md`, antes de crear la primera spec.
- Cuando el usuario pida crear o actualizar las reglas para agentes.

## Precondiciones

- `docs/constitution.md` con `Estado: APROBADA`. Si no: `AGENTS.MD BLOQUEADA — Falta constitution aprobada. Siguiente paso: sdd-constitution`.

## Contexto requerido

- `docs/constitution.md`.

## Entradas

- Preferencias del usuario sobre el manual (opcional).

## Procedimiento

1. Lee `docs/constitution.md`.
2. Si ya existe `AGENTS.md`, léelo: conserva su contenido específico del proyecto que no contradiga el flujo SDD y intégralo; el flujo SDD prevalece ante conflictos. Informa al usuario de cualquier conflicto encontrado.
3. Redacta `AGENTS.md` con la plantilla. Debe ser **breve** (máx. ~60 líneas) y obligar al agente a:
   1. Leer `docs/constitution.md` antes de trabajar.
   2. Identificar y leer la spec activa en `specs/`.
   3. Leer `plan.md` y `tasks.md` de la spec activa cuando corresponda a la tarea.
   4. No implementar requisitos que no existan en la spec.
   5. Trabajar solamente sobre la tarea indicada (una ejecución = una tarea).
   6. Escribir tests antes o junto con la implementación.
   7. Ejecutar las verificaciones aplicables del proyecto.
   8. No avanzar si existen errores.
   9. No añadir dependencias sin justificación explícita.
   10. No introducir cambios arquitectónicos silenciosos.
   11. Mantener sincronizados spec, plan, tasks, código y tests.
   12. Informar exactamente qué cambió.
   13. Informar qué tests y verificaciones ejecutó, con resultado.
   14. Detenerse al completar la tarea solicitada.
   15. Consultar `sdd-orchestrator` cuando dude de la fase correcta.
   16. Para bugs usar `sdd-bug`; para revisiones `sdd-review`; para refactors sin cambio funcional `sdd-refactor`.
   17. Mantener la comunicación concisa: estado, cambios, evidencia y siguiente paso; no explicar conceptos salvo petición.
   18. Si la tarea afecta una interfaz, leer ui-spec.md y el Design System aplicable.
   19. No declarar una UI terminada sin browser verification cuando corresponda.
   20. Mantener evidencia de navegador para los criterios UI verificados.
4. Muestra el contenido y pide confirmación para escribirlo en la raíz.
5. Escribe `AGENTS.md` y recomienda: `Siguiente paso: sdd-spec` (o `sdd-orchestrator` si ya existen specs).

### Plantilla de `AGENTS.md`

```markdown
# AGENTS.md — Reglas operativas para agentes de IA

Manual permanente de este repositorio. No contiene requisitos de producto:
los requisitos viven en specs/NNN-slug/spec.md.

## Antes de tocar código
1. Lee docs/constitution.md. Sus principios son innegociables.
2. Identifica la spec activa en specs/ y léela.
3. Lee plan.md y tasks.md de esa spec cuando la tarea lo requiera.

## Mientras trabajas
4. No implementes comportamiento sin requisito en la spec.
5. Trabaja SOLO la tarea indicada (T-XXX). Una ejecución = una tarea.
6. Escribe tests antes o junto con la implementación (TDD).
7. Ejecuta las verificaciones aplicables del proyecto y no avances con errores.
8. No añadas dependencias sin justificarlas.
9. No introduzcas decisiones arquitectónicas fuera de plan.md; si crees que
   se necesita un cambio de spec o de plan, DETENTE y propón sdd-change o
   sdd-plan.
10. Mantén sincronizados spec, plan, tasks, código y tests.

## Al terminar
11. Informa exactamente qué archivos creaste o modificaste.
12. Informa qué tests y verificaciones ejecutaste y su resultado.
13. Detente. No comiences otra tarea por tu cuenta.

## Comunicación
14. Sé conciso por defecto: informa estado, cambios, tests/verificaciones y siguiente paso.
15. No repitas contexto ni expliques el proceso SDD salvo que el usuario lo pida.
16. Para bugs usa `sdd-bug`; para auditorías `sdd-review`; para refactors sin cambio funcional `sdd-refactor`.

Si dudas de qué fase procede, invoca sdd-orchestrator.
```

## Artefactos de salida

- `AGENTS.md` (raíz del repositorio)

## Validación

- Contiene las obligaciones operativas anteriores (o equivalente completo) sin requisitos de producto.
- No copia el texto íntegro de `constitution.md`; la referencia.
- El usuario confirmó el contenido antes de escribirlo.

## Condiciones de parada

- Tras escribir `AGENTS.md`: recomienda el siguiente paso y DETENTE.

## Acciones prohibidas

- Incluir requisitos funcionales, user stories o decisiones técnicas en `AGENTS.md`.
- Escribir código, tests o specs.
- Sobrescribir un `AGENTS.md` existente sin preservar su contenido no conflictivo.

## Siguiente fase permitida

`sdd-spec` (proyecto nuevo) · `sdd-orchestrator` (si ya hay specs)
