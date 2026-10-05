# Cambios v5 → v6

## Eliminado
- Capa UI completa: `sdd-ui-discovery`, `frontend-design`, `sdd-ui`, `sdd-design-system`, `sdd-ui-review`, `sdd-browser-review`, sus artefactos (`ui-design-brief`, `ui-spec`, `ui-review`, `browser-review`) y `frontend-design-SDD-INTEGRATION.md`. Las decisiones de interfaz las declara el usuario en la sección Interfaz de la spec (UI-NNN), que se trazan, implementan y validan como cualquier requisito.
- `openjev-decision-gate`: dependía de una herramienta externa no definida y el routing determinista ya prevalecía.
- `specs-template/`: incompleta y no referenciada; las plantillas viven dentro de cada skill.

## Fusionado
- `sdd-trace` → `sdd-tasks`: la verificación spec → plan ocurre antes de descomponer; la matriz queda en `tasks.md`.
- `sdd-doc-sync` → `sdd-release`: la sincronización de documentación es un control del gate.
- `sdd-debug` → `sdd-bug`: el diagnóstico es la fase 0 del bug.

## Corregido
- Campos leídos por el orquestador que ninguna plantilla definía: la spec ahora tiene `Tamaño` e `Interfaz`.
- Contradicción entre `tdd` (pragmático) y `sdd-implement` (estricto): la tarea decide qué se testea (`Tests requeridos`, con `N/A — motivo` explícito) y `tdd` aporta el método.
- `tdd`: traducido y sin dependencia de Graphify.
- Plantilla de `AGENTS.md` incompleta respecto a su procedimiento.
- Sección de migración duplicada en el plan (§13 y §16).
- `sdd-init` solo soportaba proyectos nuevos; ahora inspecciona repos existentes.

## Implementación continua
- `sdd-implement` encadena por defecto todas las tareas pendientes sin pedir "continúa" entre ellas. Cada tarea se cierra con evidencia en `tasks.md` (punto de control) antes de abrir la siguiente, y el bucle se detiene ante fallo, desvío, decisión visual pendiente o confirmación requerida. `sdd-implement T-XXX` mantiene el modo de tarea única.
- Un commit por tarea cerrada (también por bug y refactor), solo con los archivos de ese trabajo y con formato definido en la sección Commits de `AGENTS.md`. Sin push, cambios de rama, `--no-verify` ni reescritura de historia. Exige árbol limpio al empezar para no mezclar cambios ajenos.

## Optimización de tokens
- Convenciones comunes centralizadas en `AGENTS.md` (antes copiadas en 11 skills).
- Comandos de verificación descubiertos una vez y persistidos en `AGENTS.md`.
- Verificación escalonada: acotada por tarea, completa una vez en validate (con commit), reutilizada en release.
- El orquestador lee solo cabeceras y veredictos.
- Entrevistas por bloques y hallazgos de clarify por lote, con opción recomendada.
- Modo delta generalizado (plan, tasks, validate) con re-sellado de lo no afectado.
- Modo breve para specs de tamaño S.
- De 27 a 17 skills; ~25K → ~11K tokens de corpus (descriptions siempre cargadas: ~2.3K → ~1.7K).
