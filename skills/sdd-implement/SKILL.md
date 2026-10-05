---
name: sdd-implement
description: Implementa las tareas de tasks.md con el ciclo TDD (RED → GREEN → REFACTOR). Por defecto encadena automáticamente todas las tareas pendientes de la spec en orden de dependencias, cerrando cada una con evidencia en tasks.md y un commit propio antes de pasar a la siguiente, y se detiene ante cualquier fallo, desvío o confirmación requerida. Con "sdd-implement T-XXX" implementa solo esa tarea. Implementa la interfaz exactamente como la declaró el usuario (UI-NNN). Úsala para implementar tareas planificadas o continuar una implementación en curso.
---

# sdd-implement — Implementación con TDD

Aplica las Convenciones SDD de `AGENTS.md`.

## Modos

- **Continuo** (`sdd-implement`, por defecto): implementa todas las tareas pendientes, una detrás de otra, sin pedir permiso entre ellas.
- **Tarea única** (`sdd-implement T-XXX`): implementa solo esa tarea y se detiene.

En ambos modos se trabaja **una tarea a la vez**: cada tarea se cierra por completo (tests, checks y evidencia en `tasks.md`) antes de empezar la siguiente. Nunca hay dos tareas abiertas a la vez.

## Precondiciones (lee solo cabeceras; se comprueban una vez al inicio)

1. Constitution `APROBADA`, spec `APROBADA` sin `[NECESITA ACLARACIÓN`.
2. `tasks.md` vigente (Spec-Version y Plan-Version actuales) con `TRAZABILIDAD: PASS`.
3. Si el proyecto usa git: árbol de trabajo limpio. Si hay cambios sin commitear que no pertenecen a esta implementación, no los mezcles: pregunta al usuario qué hacer antes de empezar.

Si falta algo: `IMPLEMENTACIÓN BLOQUEADA — … Siguiente paso: <skill>`.

## Bucle

1. **Selecciona** la primera tarea `- [ ]` cuyas dependencias estén en `- [x]` (en modo tarea única, la indicada; si sus dependencias no están completas, bloquea).
2. **Contexto mínimo de la tarea:** su entrada en `tasks.md`, los RF/RNF/UI que cubre, las secciones del plan citadas en la trazabilidad y el código afectado. No arrastres detalles de tareas anteriores: lo que importa de ellas ya está en el código y en su evidencia.
3. **Comandos.** Usa la tabla de `AGENTS.md`. Si está vacía o un comando no existe, descúbrelos en los manifiestos una sola vez y actualiza esa tabla (única edición de `AGENTS.md` permitida aquí). Nunca inventes comandos.
4. **RED.** Escribe los tests de `Tests requeridos` y confirma que fallan por la razón correcta.
5. **GREEN.** El mínimo código que los hace pasar. Nada fuera de lo que cubre la tarea.
6. **REFACTOR.** Solo con verde y sin cambiar comportamiento.
7. **Verificación acotada.** Tests de la tarea y del módulo afectado, más lint/typecheck sobre lo modificado. Build solo si la tarea toca configuración de build o puntos de entrada. La suite completa la ejecuta `sdd-validate`.
8. **Interfaz.** Implementa los UI-NNN tal como están declarados, reutilizando componentes y estilos existentes. Si hace falta una decisión visual no declarada, es una condición de parada.
9. **Cierre.** Solo si todo pasa: marca `- [x]` y añade en `Evidencia` los comandos con su resultado y los archivos tocados.
10. **Commit** (si el proyecto usa git). Añade solo los archivos de esta tarea más `tasks.md` (y `AGENTS.md` si actualizaste comandos), nunca `git add -A`. Mensaje según la sección Commits de `AGENTS.md`. Si un hook falla, es un check fallido: corrige dentro del alcance o detente. `tasks.md` + el commit son el punto de control: si la sesión se interrumpe, se reanuda desde ahí.
11. **Reporta** una línea: `✓ T-XXX — <objetivo> · tests PASS · <n> archivos · <sha corto>`.
12. **Continúa** con el paso 1. En modo tarea única, o cuando no queden tareas, ve al informe final.

## Condiciones de parada (en cualquier modo)

Detén el bucle, deja la tarea actual en `- [ ]` y explica el motivo cuando:

- Un test o check falla y no se resuelve dentro del alcance de la tarea.
- Falta o está mal un requisito → `sdd-change`.
- Hace falta una decisión técnica o dependencia no prevista en el plan → `sdd-plan`.
- La tarea está mal descompuesta o su "Hecho cuando" es inalcanzable → `sdd-tasks`.
- Hace falta una decisión visual no declarada → pregunta al usuario.
- El "Hecho cuando" exige confirmación del usuario → muestra los pasos y espera su confirmación explícita. Con ella, cierra la tarea y retoma el bucle.
- El commit falla y no se resuelve dentro del alcance de la tarea.
- El usuario pide parar.

## Informe final

```text
IMPLEMENTACIÓN: <n> tareas completadas en esta sesión (T-00A … T-00B)
PENDIENTES: <ninguna | T-XXX…>
DETENIDO EN: <— | T-XXX: motivo>
SIGUIENTE: sdd-validate | <skill según el motivo de parada>
```

Al terminar todas las tareas, DETENTE: la validación la lanza el usuario o el orquestador.

## Prohibido

Tener más de una tarea abierta a la vez, avanzar con una tarea sin cerrar o con checks fallando, comportamiento o diseño sin requisito, marcar `- [x]` sin evidencia de esta ejecución, inventar comandos, editar spec o plan, cambios arquitectónicos silenciosos, continuar tras una condición de parada, hacer push, crear o cambiar de rama sin indicación, usar `--no-verify`, reescribir commits ya hechos (amend, rebase, reset) o incluir en un commit archivos ajenos a la tarea.
