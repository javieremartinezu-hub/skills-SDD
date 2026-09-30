---
name: sdd-ui-discovery
description: Descubre de forma progresiva las decisiones UX/UI necesarias antes de formalizar una UI Spec dentro de SDD. Inspecciona primero la spec y la interfaz existente, evita preguntas repetidas y persiste un UI Design Brief versionado.
---

# sdd-ui-discovery — UX/UI Discovery

## Propósito

Convertir la intención funcional de una spec en un **UI DESIGN BRIEF** sin inventar requisitos ni fijar una estética arbitraria.

## Cuándo usar

- Después de `sdd-clarify` cuando `UI impact = DIRECT|INDIRECT`.
- Cuando una modificación visual requiere descubrir decisiones de UX antes de actualizar `ui-spec.md`.

## Precondiciones

- `spec.md` aprobada y `clarify.md` vigente con `SPEC CLARIFICADA: SÍ`.
- Si existe `ui-design-brief.md`, leerlo y evolucionarlo en lugar de crear otro.
- Si el cambio funcional no está definido, detenerse y enrutar a `sdd-change`.

## No hacer

- No cambiar requisitos funcionales.
- No elegir tecnología.
- No modificar código.
- No convertir una preferencia del agente en requisito.

## Procedimiento

1. Leer `constitution`, `AGENTS`, `spec.md`, `clarify.md` y la UI existente cuando exista.
2. Identificar decisiones ya resueltas y no volver a preguntarlas.
3. Determinar las áreas relevantes: flujo principal, navegación, layout, densidad, jerarquía, contenido, estados, responsive, accesibilidad, motion, referencias y restricciones técnicas que tengan impacto visual.
4. Preguntar de forma progresiva únicamente las decisiones que no puedan deducirse de la spec, el producto existente o convenciones establecidas.
5. Diferenciar explícitamente `REQUISITO`, `DECISIÓN DE DISEÑO`, `PREFERENCIA` y `SUPUESTO`.
6. Presentar alternativas solo cuando la decisión tenga impacto real en la experiencia; no convertir el discovery en un cuestionario masivo.
7. Escribir o actualizar `specs/NNN-slug/ui-design-brief.md`.
8. Si una respuesta implica un cambio de comportamiento, marcar `[NECESITA ACLARACIÓN]` y enrutar a `sdd-clarify`/`sdd-change`; no ocultar el cambio dentro del brief.

## Plantilla mínima

```markdown
# UI Design Brief — NNN-slug

| Campo | Valor |
|---|---|
| Spec-ID | NNN-slug |
| Spec-Version | N |
| Brief-Version | M |
| Estado | PROPUESTA |

## Objetivo de experiencia
## Usuarios y contexto
## Flujos prioritarios
## Navegación / arquitectura de información
## Layout y densidad
## Componentes / patrones
## Estados y edge cases
## Responsive
## Accessibility
## Motion
## Content / locale
## Referencias
## Decisiones de diseño
| ID | Tipo | Decisión | Justificación | Estado |
## Preguntas abiertas
```

## Cierre

El brief está listo para diseño cuando las decisiones necesarias estén resueltas o documentadas como preguntas abiertas explícitas. En este punto `Design Status` permanece `PENDING`.

**Siguiente paso:** `frontend-design`.
