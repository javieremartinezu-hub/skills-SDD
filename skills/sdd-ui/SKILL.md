---
name: sdd-ui
description: Formaliza y mantiene la UI/UX como contrato SDD persistente en specs/NNN-slug/ui-spec.md. Vincula la UI a la versión de la spec, incorpora decisiones de frontend-design y aplica un gate de aprobación cuando la experiencia cambió de forma material.
---

# sdd-ui — UI/UX Specification

## Propósito

Convertir el resultado de discovery + diseño en el **contrato UI persistente** que implement, reviews y validate utilizarán como fuente de verdad visual.

## Precondiciones

- `spec.md` aprobada.
- `clarify.md` vigente.
- `ui-design-brief.md` y, cuando se ejecutó, salida de `frontend-design` disponibles.
- Si existe `ui-spec.md`, leerla antes de modificarla.

## Reglas

- No inventar comportamiento funcional.
- No resolver `[NECESITA ACLARACIÓN]` de la spec en silencio.
- Reutilizar el Design System existente antes de crear nuevos tokens/patrones.
- Toda modificación debe registrar `Spec-Version` y `UI-Spec-Version`.
- Una UI Spec desalineada con `Spec-Version` es CADUCADA y no supera ningún gate.

## Procedimiento

1. Leer spec, brief, output de diseño y UI existente.
2. Determinar si se crea una nueva UI Spec o un delta sobre una versión existente.
3. Formalizar: objetivo de experiencia, usuarios/roles, flows, vistas, layout, navegación, componentes, visual language, design tokens, typography, colors, spacing, icons, states, responsive, accessibility, motion, content/locale, references y browser verification.
4. Definir criterios `UI-001...` y, cuando corresponda, mapearlos a RF.
5. Definir qué viewports, estados y flujos requieren Browser Review.
6. Escribir `specs/NNN-slug/ui-spec.md` con `Estado: PROPUESTA` para una nueva versión.
7. Mostrar el delta/resumen y solicitar aprobación explícita cuando la UI cambió materialmente. Para cambios puramente mecánicos que no alteran UX, puede conservarse aprobación previa y documentar el motivo.
8. Con aprobación explícita, actualizar `Estado: APROBADA` y registrar `Aprobación: APROBADA (fecha, por el usuario)`.
9. DETENER.

## Cabecera canónica

```markdown
# UI Spec — NNN-slug

| Campo | Valor |
|---|---|
| Spec-ID | NNN-slug |
| Spec-Version | N |
| UI-Spec-Version | M |
| Estado | PROPUESTA |
| Aprobación | PENDIENTE |
```

## Artefacto

`specs/NNN-slug/ui-spec.md`

## Validación

- `Spec-Version` coincide con `spec.md`.
- `UI-Spec-Version` incrementa cuando cambia el contrato.
- Existe al menos un criterio UI cuando realmente hay UI.
- Los criterios de browser están definidos para los flujos visibles relevantes.
- Aprobación solo se registra tras respuesta inequívoca del usuario.

## Siguiente fase

Tras `Estado: APROBADA`: `sdd-design-system` si aplica; en caso contrario `sdd-plan`.
