---
name: sdd-design-system
description: Define o mantiene el lenguaje visual y tokens compartidos del producto.
---
# sdd-design-system

## Propósito
Mantener un contrato visual reutilizable.

## Estructura
```text
design/
├── foundations.md
├── tokens.md
├── components.md
├── patterns.md
└── motion.md
```

## Regla
Antes de crear estilos nuevos, buscar tokens/componentes existentes. No introducir colores, spacing, radius o motion arbitrarios sin justificación.

## Motion
Definir duración/easing y comportamiento `prefers-reduced-motion`.
