# Frontend Design + SDD — v5

`frontend-design` es una skill de diseño desacoplada y reutilizable. Dentro de SDD funciona como motor de decisiones visuales entre `sdd-ui-discovery` y `sdd-ui`.

## Flujo integrado

`spec → clarify + approval → ui-discovery → frontend-design → sdd-ui + UI approval → design-system → plan → trace → tasks → implement/TDD → UI/browser reviews → validate → doc-sync → release`

## Separación de responsabilidades

| Skill | Dueño |
|---|---|
| `sdd-ui-discovery` | Descubrimiento y preguntas UX/UI |
| `frontend-design` | Decisiones de diseño y patrones |
| `sdd-ui` | Contrato persistente `ui-spec.md` |
| `sdd-design-system` | Tokens/lenguaje compartido |
| `sdd-ui-review` | Auditoría estática |
| `sdd-browser-review` | Evidencia de UI renderizada |

## Reglas

- La UI Spec es versionada y queda vinculada a `Spec-Version`.
- Cambios de UI que alteran comportamiento deben volver a `sdd-change`.
- Browser Review final siempre prueba la feature completa.
- Una revisión browser de tarea solo puede validar el alcance de esa tarea.
- Las referencias visuales no autorizan copiar branding, assets o contenido.
