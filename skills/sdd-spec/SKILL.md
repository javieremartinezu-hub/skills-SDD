---
name: sdd-spec
description: Genera la especificación funcional specs/NNN-slug/spec.md (QUÉ y POR QUÉ, nunca CÓMO) entrevistando al usuario por bloques. Requisitos con IDs estables RF/RNF, formato EARS, marcador [NECESITA ACLARACIÓN], tamaño S/M/L y una sección Interfaz donde se registran, sin inventar nada, las decisiones visuales que declara el usuario (UI-NNN). Úsala para una funcionalidad nueva sin spec propia, tras sdd-agents o cuando sdd-orchestrator lo indique.
---

# sdd-spec — Especificación funcional

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

- Constitution `APROBADA` y `AGENTS.md` existente; si no, bloquea hacia `sdd-constitution` / `sdd-agents`.
- Si hay otra spec en curso, avisa y confirma que se quiere una nueva.

## Procedimiento

1. Lee constitution, brief y las specs relacionadas (para no duplicar requisitos). Número = máximo `NNN` existente + 1; slug kebab-case corto. Nunca reutilices números.
2. **Entrevista por bloques** de 3-5 preguntas sobre: problema · actores · comportamiento · permisos · estados · datos (semántica, no modelo físico) · límites y volúmenes · errores · seguridad funcional · casos límite · fuera de alcance · criterios de aceptación. Puedes proponer un valor sugerido; solo se registra si el usuario lo confirma. Lo no decidido queda como `[NECESITA ACLARACIÓN: …]`.
3. **Interfaz.** Pregunta si la funcionalidad tiene interfaz. Si sí, pide al usuario sus decisiones (vistas, componentes, estados visibles, textos, responsive, referencias) y regístralas tal cual con IDs `UI-NNN`. No propongas ni completes diseño: lo que falte y sea necesario queda como `[NECESITA ACLARACIÓN]`.
4. Aplica a cada RF/UI el test de verificabilidad: "¿puedo escribir un test o una comprobación objetiva que lo demuestre?". Si no, reescríbelo o márcalo.
5. Propón el `Tamaño` (`S`: ≤3 RF, sin migración, sin cambios de permisos/seguridad ni dependencias nuevas; si no, `M`/`L`).
6. Escribe la spec, muéstrala. `Siguiente paso: sdd-clarify`. DETENTE.

## EARS

`EL SISTEMA <r>.` · `CUANDO <evento>, EL SISTEMA <r>.` · `MIENTRAS <estado>, EL SISTEMA <r>.` · `DONDE <condición>, EL SISTEMA <r>.` · `SI <e>, ENTONCES EL SISTEMA <r>.`

## Plantilla de `spec.md`

```markdown
# <Título>

| Campo | Valor |
|---|---|
| Spec-ID | NNN-slug |
| Versión | 1 |
| Estado | BORRADOR |
| Aprobación | PENDIENTE |
| Tamaño | S / M / L |
| Interfaz | SÍ / NO |

## Contexto y problema
## Objetivo
## Actores
## Historias de usuario
## Requisitos funcionales
- RF-001: …
## Requisitos no funcionales
- RNF-001: …
## Estados
## Permisos
| Rol | Acción | Permitido |
## Errores
| Condición | Comportamiento observable |
## Casos límite
## Seguridad funcional
## Interfaz (declarada por el usuario; omitir si Interfaz = NO)
- UI-001: <decisión tal como la declaró el usuario> (RF relacionados)
## Fuera de alcance
## Criterios de finalización
- [ ] …
## Decisiones pendientes

## Registro de cambios
| Versión | Fecha | Clasificación | Cambio |
| 1 | YYYY-MM-DD | — | Creación |
```

## Validación

- IDs únicos; cada RF/UI verificable o marcado.
- Sin tecnología, arquitectura ni modelo físico salvo restricción explícita del usuario.
- Sin decisiones de interfaz que no haya declarado el usuario.
- `Fuera de alcance` y `Criterios de finalización` presentes.

## Prohibido

Diseñar arquitectura, elegir tecnologías, escribir código, resolver ambigüedades por tu cuenta, aprobar la spec.
