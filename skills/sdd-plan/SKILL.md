---
name: sdd-plan
description: Convierte una spec APROBADA en diseño técnico. Prioriza compatibilidad con el stack existente, simplicidad, reutilización, seguridad, migraciones/rollback y mínimo número de nuevas dependencias o abstracciones. Para cambios locales usa modo delta; no escribe código.
---

# sdd-plan — Diseño técnico

## Precondiciones

- Constitution aprobada.
- Spec con `Estado: APROBADA`, aprobación humana y `SPEC CLARIFICADA: SÍ`.
- Cero `[NECESITA ACLARACIÓN]`.

## Procedimiento

1. Inspecciona repositorio y plan anterior si existe.
2. Si el último cambio de spec está marcado `[LOCAL]`, usa **DELTA/COMPATIBILIDAD**: conserva diseño válido y toca solo lo afectado. Si es `[ESTRUCTURAL]`, usa revisión completa de secciones afectadas.
3. Diseña la solución más simple que cumpla la spec dentro del stack/patrones existentes.
4. Antes de introducir componente, capa, patrón o dependencia nueva, documenta qué necesidad concreta resuelve y por qué lo existente no basta.
5. Evalúa contratos, datos, auth, seguridad, errores, observabilidad, configuración, tests, despliegue y rendimiento solo según relevancia real.
6. Si existen cambios de DB/API/formato persistido, incluye estrategia de compatibilidad, migración, validación de datos y rollback; si requiere trabajo especializado, enruta a `sdd-migration`.
7. Registra decisiones difíciles de revertir con motivo, alternativas y RF cubiertos.
8. Referencia secciones del plan a RF; escribe `plan.md` y DETENTE con `Siguiente paso: sdd-trace`.

## Principios para IA

- Reuse-first antes de crear.
- Menor superficie de cambio posible.
- No diseñar para requisitos futuros no presentes.
- Dependencias nuevas son excepción, no default.
- Compatibilidad hacia atrás salvo decisión explícita de la spec.

## Plantilla mínima de `plan.md`

```markdown
# Plan técnico — NNN-slug
> Spec-ID: ... · Spec-Version: N · Plan-Version: M

## Arquitectura y componentes
## Stack/dependencias y justificación
## Datos y persistencia
## APIs/contratos
## Autenticación/autorización y seguridad
## Errores/observabilidad/configuración
## Estrategia de tests
## Migraciones/compatibilidad/rollback
## Despliegue
## Rendimiento/escalabilidad (si aplica)
## Cobertura de RF
## Registro de decisiones
```

## Prohibido

- Escribir código/tests.
- Agregar requisitos.
- Introducir complejidad “por si acaso”.
