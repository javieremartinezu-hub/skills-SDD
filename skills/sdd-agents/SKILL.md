---
name: sdd-agents
description: >-
  Genera AGENTS.md en la raíz del repositorio: reglas operativas breves y permanentes para agentes de IA. Obliga a trabajar desde artefactos SDD, limitar alcance, reutilizar antes de crear, evitar sobreingeniería, aplicar TDD cuando aporta valor, verificar cambios, hacer self-review y comunicar de forma concisa. Úsala tras aprobar docs/constitution.md o para actualizar las reglas del repositorio.
---

# sdd-agents — Manual operativo para agentes de IA

## Convenciones

- La fuente de verdad son los artefactos del repositorio, nunca la conversación.
- Artefactos principales: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`.
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`.

## Propósito

Crear o actualizar `AGENTS.md` como manual breve para cualquier agente que programe en el repositorio.

## Precondiciones

- `docs/constitution.md` con `Estado: APROBADA`. Si no: `AGENTS.MD BLOQUEADA — Siguiente paso: sdd-constitution`.

## Procedimiento

1. Lee `docs/constitution.md` y el `AGENTS.md` existente si lo hay.
2. Conserva reglas específicas del proyecto que no contradigan SDD.
3. Genera un `AGENTS.md` breve (objetivo: ≤70 líneas) usando la plantilla.
4. Muestra el contenido y pide confirmación antes de escribirlo.
5. Escribe el archivo y DETENTE.

### Plantilla de `AGENTS.md`

```markdown
# AGENTS.md — Reglas operativas para agentes de IA

Los requisitos viven en `specs/`. La conversación no sustituye los artefactos.

## Antes de modificar
1. Lee `docs/constitution.md` y la spec activa.
2. Lee `plan.md` y `tasks.md` cuando correspondan.
3. Entiende el código afectado antes de editarlo.
4. Busca implementaciones, helpers, patrones y componentes existentes antes de crear otros nuevos.
5. Define el scope probable: archivos, módulos, contratos y tests afectados.

## Durante el trabajo
6. No implementes comportamiento sin requisito que lo respalde.
7. Trabaja solo sobre la tarea o problema solicitado.
8. Prefiere cambios mínimos y localizados.
9. No modifiques fuera del scope salvo necesidad comprobada; si aparece, reevalúa el impacto antes de seguir.
10. Usa la solución más simple que cumpla completamente el requisito.
11. No introduzcas capas, servicios, abstracciones, patrones o dependencias sin necesidad concreta.
12. No dupliques funcionalidad existente.
13. Aplica TDD cuando aporte valor; para bugs reproducibles crea test de regresión antes del fix.
14. No cambies una spec para justificar un bug existente.
15. No cambies contratos públicos, persistencia, permisos, seguridad o arquitectura silenciosamente.
16. No añadas dependencias sin justificar necesidad, mantenimiento, seguridad y ausencia de alternativa existente.
17. Mantén compatibilidad hacia atrás salvo que la spec indique lo contrario.
18. Actualiza solo la documentación afectada.

## Verificación y self-review
19. Ejecuta las verificaciones aplicables del proyecto; nunca afirmes que pasó algo que no ejecutaste.
20. Antes de cerrar, revisa el diff y comprueba:
   - cumplimiento de spec/tarea;
   - ausencia de cambios ajenos al scope;
   - ausencia de duplicación o abstracciones innecesarias;
   - casos borde evidentes;
   - contratos preservados;
   - tests que realmente prueban el comportamiento.
21. No avances con errores relevantes.

## Enrutamiento
22. Bug → `sdd-bug`; diagnóstico incierto → `sdd-debug`.
23. Cambio funcional → `sdd-change`; migración sensible → `sdd-migration`.
24. Refactor → `sdd-refactor`.
25. Auditoría → `sdd-review quality|implementation|security|dependencies`.
26. Sincronizar documentación → `sdd-doc-sync`; pre-release → `sdd-release`.
27. Si dudas de la fase, usa `sdd-orchestrator`.

## Comunicación
28. Sé conciso por defecto: resultado, cambios, evidencia y siguiente paso.
29. No repitas contexto ni expliques conceptos/proceso salvo petición del usuario.
30. Al completar una tarea, detente; no empieces otra por tu cuenta.
```

## Validación

- No contiene requisitos de producto ni duplica la constitution.
- Incluye scope guard, reuse-first, simplicidad, dependency guard, TDD pragmático, self-review y comunicación concisa.

## Acciones prohibidas

- Escribir código, specs o planes.
- Sobrescribir reglas válidas específicas del proyecto sin necesidad.

## Siguiente fase permitida

`sdd-spec` o `sdd-orchestrator`.
