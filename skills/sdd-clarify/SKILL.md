---
name: sdd-clarify
description: >-
  Audita la spec como QA senior, resuelve los hallazgos con el usuario y conduce su aprobación
  humana. Úsala con una spec en BORRADOR, modificada por sdd-change o creada por sdd-retro.
---

# sdd-clarify — Clarificación y aprobación de la spec

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

`spec.md` existe; si no, bloquea hacia `sdd-spec`.

## Procedimiento

1. Lee `spec.md` y `docs/constitution.md`. Si viene de `sdd-change`, audita solo las secciones tocadas según el último registro de cambios, más su coherencia con el resto.
2. **Auditoría.** Registra hallazgos `H-NN` en `clarify.md` revisando: ambigüedades · contradicciones (internas y con la constitution) · duplicados · RF no verificables · casos límite ausentes · estados sin transiciones · errores sin definir · suposiciones implícitas · huecos de seguridad funcional · tecnología o diseño interno colado · Interfaz: UI-NNN que contradicen RF o decisiones visuales necesarias no declaradas · Tamaño mal clasificado.
3. **Resolución por lote.** Presenta todos los hallazgos agrupados por tema. Para cada uno ofrece una opción recomendada y alternativas breves, para que el usuario pueda responder en un solo mensaje. Excepción: en hallazgos de Interfaz no propongas diseño, pide la decisión. En specs RETRO, cada anomalía se decide: intencional (queda) o defecto (RF reescrito con lo esperado y marcado `(defecto)`). Aplica a `spec.md` solo lo que el usuario decide; lo no decidido sigue `[NECESITA ACLARACIÓN]`.
4. **Re-auditoría incremental** de las secciones editadas, hasta que no haya hallazgos nuevos.
5. **Éxito:** cero `[NECESITA ACLARACIÓN`, cero contradicciones, todo RF/UI verificable, `Fuera de alcance` no vacío. Escribe `SPEC CLARIFICADA: SÍ` y pon `Estado` en `CLARIFICADA`. Si no se cumple: `SPEC CLARIFICADA: NO` con los pendientes, y DETENTE.
6. **Gate de aprobación.** Pregunta literalmente: "¿Esta especificación representa realmente la funcionalidad que quieres construir? (sí/no)". En RETRO: "¿Esta spec describe correctamente el comportamiento actual y los defectos marcados? (sí/no)". Solo un sí inequívoco referido a esta spec cuenta; ante ambigüedad repite.
   - Sí → `Estado: APROBADA`, `Aprobación: APROBADA (YYYY-MM-DD, por el usuario)`. `Siguiente paso: sdd-plan`.
   - No → `Aprobación: PENDIENTE`. `Siguiente paso: ajustar con sdd-clarify o sdd-change`.
7. DETENTE.

## Plantilla de `clarify.md`

```markdown
# Clarificación — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Ronda | R |

| ID | Tipo | Hallazgo | Sección/RF | Decisión del usuario |
|---|---|---|---|---|

SPEC CLARIFICADA: SÍ | NO
```

## Prohibido

Corregir sin decisión del usuario, inventar requisitos o diseño para cerrar hallazgos, interpretar respuestas ambiguas como aprobación, diseñar o implementar.
