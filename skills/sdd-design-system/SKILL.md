---
name: sdd-design-system
description: Define o mantiene el Design System aplicable a una feature SDD. Inspecciona tokens y componentes existentes, evita duplicación y documenta decisiones visuales compartidas sin implementar código.
---

# sdd-design-system — Design System

## Propósito

Definir o mantener el lenguaje visual compartido que la UI Spec y la implementación deben respetar.

## Cuándo usar

- Existe UI y la feature introduce o modifica patrones/tokens compartidos.
- La UI Spec requiere decisiones que deben ser consistentes con otras pantallas.

## Precondiciones

- `spec.md` y `ui-spec.md` vigentes.
- Repositorio inspeccionado para localizar Design System existente.

## Procedimiento

1. Inspecciona tokens, componentes, estilos y convenciones existentes.
2. Reutiliza antes de crear.
3. Determina si el cambio es: `REUSE`, `EXTEND` o `NEW SYSTEM` (solo cuando realmente no existe uno).
4. Documenta componentes, tokens, estados, tipografía, color, spacing, iconografía y motion compartidos.
5. Identifica cualquier nueva decisión que deba volver a `ui-spec.md`.
6. No implementes código.

## Salida

Actualiza o crea documentación del Design System en `design/` cuando exista esa convención en el proyecto. En repositorios que ya tienen una fuente de verdad para el Design System, actualiza únicamente la sección correspondiente y documenta la ruta.

## Siguiente fase

`sdd-plan`.
