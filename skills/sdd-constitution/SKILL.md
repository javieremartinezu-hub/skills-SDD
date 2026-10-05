---
name: sdd-constitution
description: Crea o enmienda docs/constitution.md, la constitución del proyecto con 8-12 principios innegociables, cortos y verificables (SDD, simplicidad, calidad, seguridad por diseño, tests, dependencias, errores, observabilidad, cambios controlados). Exige aprobación humana explícita antes de que entre en vigor. Úsala tras sdd-init o cuando el usuario pida crear, revisar o enmendar la constitución.
---

# sdd-constitution — Constitución del proyecto

Formato de bloqueo: `<ACCIÓN> BLOQUEADA — Motivo — Falta — Siguiente paso: <skill>`.

## Precondiciones

- `docs/brief.md` existe. Si no: `CONSTITUCIÓN BLOQUEADA — Falta docs/brief.md. Siguiente paso: sdd-init`.
- Si ya hay una constitution `APROBADA`, esto es una **enmienda**: conserva lo no afectado, incrementa la revisión y vuelve a `PROPUESTA` hasta su aprobación.

## Procedimiento

1. Lee el brief y, si existe, la constitution actual.
2. Redacta 8-12 principios (en una enmienda, cambia solo lo pedido). Cada uno: enunciado de una frase + "Verificación" de una frase, comprobable en una revisión.
3. Cubre como mínimo: SDD · simplicidad y mantenibilidad · seguridad por diseño · tests · dependencias · errores y observabilidad · compatibilidad · cambios controlados. Incluye en esencia:
   - La spec manda sobre el código; ningún comportamiento existe sin requisito.
   - No se integra trabajo con tests fallando.
   - Dependencias mínimas y justificadas.
   - Seguridad por defecto; errores explícitos y observables.
   - Cambios pequeños y verificables; spec, plan, código y tests sincronizados.
4. Escribe con `Estado: PROPUESTA` (en enmienda, fila `PENDIENTE` en el registro). Muestra el documento o el diff.
5. **Gate:** pregunta literalmente "¿Apruebas esta constitución como reglas innegociables del proyecto? (sí/no)". Solo un sí inequívoco referido a ella cuenta; ante ambigüedad repite la pregunta.
   - Aprobada → `Estado: APROBADA (YYYY-MM-DD, por el usuario)`. `Siguiente paso: sdd-agents`.
   - Cambios pedidos → aplica solo esos y vuelve al gate.
6. DETENTE.

## Plantilla

```markdown
# Constitución del proyecto

> Estado: PROPUESTA · Revisión: 1

## Principios
### P-01 — <Nombre>
<Enunciado.>
Verificación: <cómo se comprueba>

## Registro de enmiendas
| Revisión | Fecha | Cambio | Aprobada por |
```

## Prohibido

Aprobar implícitamente, imponer tecnologías no impuestas por el usuario, inventar principios no validados, escribir código, tests o specs.
