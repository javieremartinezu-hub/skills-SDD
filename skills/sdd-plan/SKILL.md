---
name: sdd-plan
description: "Transforma una spec APROBADA en el diseño técnico specs/NNN-slug/plan.md (arquitectura, stack, componentes, datos, contratos, seguridad, errores, observabilidad, tests, migración, despliegue) con decisiones justificadas y una tabla de cobertura RF/UI que sdd-tasks verificará. Soporta modo breve (specs S) y modo delta (cambios LOCAL). No escribe código. Úsala cuando la spec esté APROBADA con SPEC CLARIFICADA: SÍ, o cuando el plan esté caducado."
---

# sdd-plan — Diseño técnico (CÓMO)

Aplica las Convenciones SDD de `AGENTS.md`.

## Precondiciones

spec `APROBADA` con aprobación registrada, `clarify.md` vigente con `SPEC CLARIFICADA: SÍ`, cero `[NECESITA ACLARACIÓN`. Si falta algo: `PLAN BLOQUEADO — … Siguiente paso: sdd-clarify`.

## Modo

- **Delta**: existe plan previo y la última fila del registro de cambios de la spec es `[LOCAL]` sin decisiones técnicas nuevas → conserva el diseño, edita solo lo afectado, actualiza `Spec-Version`, incrementa `Plan-Version` y añade una nota con los RF afectados.
- **Breve**: `Tamaño = S` → solo las secciones que aplican; registros D-NNN solo para dependencias nuevas o decisiones difíciles de revertir.
- **Completo**: el resto.

## Procedimiento

1. Verifica precondiciones e inspecciona el repositorio (stack, estructura, convenciones) para diseñar dentro de lo existente o justificar la desviación.
2. Redacta `plan.md`. Cada sección: contenido o `No aplica (motivo)`.
3. **Decisiones importantes** (difíciles de revertir, afectan a varios RF o añaden dependencia) → registro D-NNN. Elige por requisitos, simplicidad, mantenibilidad, seguridad, madurez, operación y facilidad de trabajo con agentes; nunca por moda. Respeta las restricciones de la spec.
4. **Interfaz**: si la spec tiene `UI-NNN`, describe cómo se implementan técnicamente (componentes, librería o design system ya existente, rutas). No añadas decisiones visuales: si falta una, DETENTE → `sdd-clarify`.
5. Completa la **tabla de cobertura**: cada RF, RNF y UI con las secciones o D-NNN que lo cubren.
6. Muestra el plan. Siguiente paso: `sdd-migration` si `Migración requerida: SÍ`; si no, `sdd-tasks`. DETENTE.

## Plantilla de `plan.md`

```markdown
# Plan técnico — NNN-slug

| Campo | Valor |
|---|---|
| Spec-Version | N |
| Plan-Version | M |
| Modo | COMPLETO / BREVE / DELTA |
| Migración requerida | SÍ / NO |

## 1. Arquitectura y componentes (responsabilidades)
## 2. Stack y dependencias (D-NNN)
## 3. Modelo de datos
## 4. APIs y contratos
## 5. Autenticación, autorización y seguridad
## 6. Errores, logging y observabilidad
## 7. Configuración
## 8. Interfaz (implementación técnica de UI-NNN)
## 9. Estrategia de tests (qué demuestra cada tipo, por RF/UI)
## 10. Migración y compatibilidad (clasificación COMPATIBLE / POR FASES / BREAKING, estrategia, rollback)
## 11. Despliegue y rollback
## 12. Rendimiento

## Cobertura
| Requisito | Cubierto en |
|---|---|
| RF-001 | §1, §4, D-002 |

## Decisiones
### D-001 — <título>
- Decisión:
- Motivo:
- Alternativas descartadas y por qué:
- Cubre: RF-…

## Notas de compatibilidad (modo delta)
```

## Prohibido

Escribir código o tests, modificar la spec (si está mal → `sdd-change`), añadir comportamiento o diseño visual no presente en la spec, dejar decisiones importantes sin justificar.
