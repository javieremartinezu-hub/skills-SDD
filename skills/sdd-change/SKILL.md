---
name: sdd-change
description: >-
  Gestiona cambios funcionales o de interfaz declarada sobre una spec existente ("añade MFA",
  "cambia el comportamiento de X", "nueva regla", "cambia esta pantalla"). Modifica PRIMERO spec.md,
  clasifica el impacto como LOCAL o ESTRUCTURAL, versiona e invalida derivados, y reinicia el ciclo
  desde sdd-clarify con trabajo incremental. Prohibido tocar código antes de que la spec cambie y se
  re-apruebe.
---

# sdd-change — Cambio desde la spec

Aplica las Convenciones SDD de `AGENTS.md`.

## Principio

Ningún cambio funcional empieza por el código. Si el usuario insiste: `CAMBIO BLOQUEADO — La spec debe cambiar y aprobarse antes que el código. Siguiente paso: completar este flujo`.

## Precondiciones

Existe la spec afectada. Si el pedido es una funcionalidad genuinamente nueva: `CAMBIO BLOQUEADO — requiere spec nueva. Siguiente paso: sdd-spec`.

## Procedimiento

1. Localiza la spec y los RF/UI afectados.
2. **Clasifica:**
   - `LOCAL`: cambia comportamiento o interfaz declarada dentro del diseño existente; no toca contratos públicos, persistencia, permisos/seguridad, RNF, arquitectura ni otras specs.
   - `ESTRUCTURAL`: toca cualquiera de esas áreas o cruza componentes/specs.
   Presenta un resumen breve del impacto (requisitos, tareas y tests afectados).
3. **Edita `spec.md`.** Los modificados conservan ID, los nuevos continúan la numeración, los eliminados quedan como `RF-XXX: (eliminado en vN — motivo)`. Revisa coherencia en errores, estados, permisos, seguridad, casos límite, interfaz, fuera de alcance y criterios. Las decisiones de interfaz las declara el usuario; no las propongas.
4. **Versiona:** incrementa `Versión`, pon `Estado: BORRADOR` y `Aprobación: PENDIENTE`, y añade al registro de cambios `[LOCAL]` o `[ESTRUCTURAL]`, qué cambió, por qué y qué requisitos toca. No edites ningún derivado: quedan caducados por versión.
5. Lo que el usuario no decida queda `[NECESITA ACLARACIÓN]`.
6. Muestra el diff resumido y DETENTE:

```text
CAMBIO REGISTRADO (spec vN, [LOCAL|ESTRUCTURAL])
Siguiente paso: sdd-clarify
Después: plan [delta si LOCAL] → [migration] → tasks (reabre solo lo afectado y re-sella el resto) → implement → validate [incremental si LOCAL] → release
```

## Prohibido

Modificar código, tests u otros artefactos; registrar aprobación (pertenece a `sdd-clarify`); resolver ambigüedades sin el usuario.
