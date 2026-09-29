---
name: sdd-validate
description: Validación final de una spec mediante evidencia RF → PLAN → TASK → CODE → TEST. Ejecuta checks reales, revisa casos límite, constitution y diff/scope; emite SPEC CUMPLIDA: SÍ o NO. No corrige código.
---

# sdd-validate — Validación final

## Precondiciones

- Spec aprobada.
- Plan/trace/tasks vigentes y `TRAZABILIDAD: PASS`.
- Todas las tasks `- [x]` con evidencia.

## Procedimiento

1. Para cada RF/RNF prueba cadena `RF → plan → task → implementación → test`.
2. Ejecuta checks reales aplicables: tests, lint, format check, typecheck, build, análisis estático, auditoría de dependencias/seguridad. No inventes comandos. Si los RF afectan UI, interfaz o flujo web, exige además la prueba funcional E2E en navegador real con evidencia visual (captura o DOM) y consola sin errores JavaScript; sin esa evidencia, `SPEC CUMPLIDA: NO`.
3. Verifica casos límite, criterios de finalización y constitution.
4. Revisa el diff/implementación global para detectar cambios fuera de scope, comportamiento sin RF, duplicación evidente, contratos rotos o sobreingeniería que afecte mantenibilidad.
5. Cuando haya migraciones, exige evidencia de compatibilidad/validación/rollback prevista en plan.
6. Escribe `validation.md` con evidencia y veredicto.
7. Si todo cumple: `SPEC CUMPLIDA: SÍ`; si no: `NO` y enruta cada hueco a `sdd-implement`, `sdd-bug`, `sdd-change`, `sdd-migration` o `sdd-refactor`.

## Plantilla

```markdown
# Validación — NNN-slug
> Spec-Version: N · Plan-Version: M

## Cadena de evidencia
| RF/RNF | Plan | Tarea | Código | Test/comando | Resultado |

## Casos límite y criterios
## Checks del proyecto
## Scope y compatibilidad
## Migraciones (si aplica)
## Constitution
## Veredicto
SPEC CUMPLIDA: SÍ | NO
```

## Salida

```text
VALIDACIÓN: PASS | FAIL
RF/RNF: <cubiertos>/<total>
CHECKS: <resumen>
HUECOS: <ninguno o breve>
SIGUIENTE: <skill o ninguna>
```

## Prohibido

- Modificar código/tests para hacer pasar la validación.
- Aceptar evidencia no ejecutada.
