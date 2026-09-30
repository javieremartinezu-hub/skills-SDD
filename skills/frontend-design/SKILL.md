---
name: frontend-design
description: Diseña interfaces web modernas, usables, accesibles y responsive a partir de requisitos, UI Design Brief o una interfaz existente. Puede ejecutarse standalone o como etapa de diseño dentro de SDD y nunca sustituye la UI Spec persistente.
---

# frontend-design — Diseño frontend

## Rol

Es el motor de diseño visual y de experiencia. Genera decisiones de interfaz coherentes con producto, contenido, accesibilidad y contexto. En SDD, entrega decisiones estructuradas a `sdd-ui`; `sdd-ui` es la única skill que publica el contrato persistente `ui-spec.md`.

## No hace

- No cambia requisitos funcionales.
- No diseña arquitectura técnica del sistema.
- No implementa código.
- No sustituye TDD.
- No reemplaza `sdd-ui`, `sdd-ui-review` ni `sdd-browser-review`.
- No copia branding, contenido o assets de referencias.

## Entradas

- `spec.md` y `ui-design-brief.md` cuando se ejecuta desde SDD.
- Interfaz existente para evolución incremental.
- Restricciones de producto y Design System existente.

## Proceso

1. Analizar objetivos, usuarios, tareas y contexto.
2. Inspeccionar la interfaz y Design System existentes antes de crear patrones nuevos.
3. Identificar decisiones de información, navegación, layout, componentes, estados, responsive, accessibility, motion, contenido y visual language.
4. Seleccionar o adaptar patrones adecuados según la tarea y el contenido.
5. Evitar interfaces genéricas y decoración sin función.
6. Para cambios incrementales, producir un **UI Spec Delta** en lugar de rediseñar toda la superficie.
7. En modo SDD, actualiza `specs/NNN-slug/ui-design-brief.md` en una sección `## Design Direction` y establece `Design Status: READY` cuando las decisiones necesarias estén resueltas. No cambies requisitos funcionales.
8. Entregar una salida estructurada:

```text
UI DESIGN OUTPUT
- Layout:
- Navigation:
- Visual language:
- Components:
- States:
- Responsive:
- Accessibility:
- Motion:
- Content/locale:
- Reuse from existing design system:
- New tokens/patterns required:
- Open decisions:
- UI acceptance implications:
```

9. En SDD, no editar `ui-spec.md`: `sdd-ui` es su único dueño. Transferir las decisiones persistidas en el brief.

## Referencias

Las referencias incluidas en `references/` sirven para orientar patrones. Deben usarse para extraer características de interacción/visualidad, no para copiar identidad.

## Siguiente paso SDD

`sdd-ui`.
