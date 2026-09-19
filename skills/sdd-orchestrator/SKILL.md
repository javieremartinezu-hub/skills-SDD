---
name: sdd-orchestrator
description: Gobierna el flujo Spec-Driven Development (SDD). Diagnostica artefactos y enruta al único flujo adecuado para desarrollo, bugs, diagnóstico, cambios, migraciones, refactor, auditorías, sincronización documental o release. No implementa ni modifica artefactos.
---

# sdd-orchestrator — Gobernante del flujo SDD

## Convenciones

- La fuente de verdad son los artefactos del repositorio, nunca la conversación.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `docs/brief.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`.
- Spec: `BORRADOR → CLARIFICADA → APROBADA`.
- Artefactos derivados deben coincidir en `Spec-Version` y `Plan-Version`; si no, están `CADUCADOS`.
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`.

## Propósito

Diagnosticar el estado real y elegir el único siguiente flujo válido sin saltar gates.

## Alcance

- SOLO lee, diagnostica y enruta.
- NO modifica código ni artefactos.

## Procedimiento

1. Diagnostica desde disco: constitution, AGENTS, spec activa, clarify, plan, trace, tasks y validation; marca versiones no coincidentes como `CADUCADO`.
2. **Antes del pipeline lineal, clasifica mantenimiento:**
   - Fallo claramente contrario a una spec → `sdd-bug`.
   - Síntoma/fallo con causa o clasificación incierta → `sdd-debug`.
   - Cambio de comportamiento → `sdd-change`.
   - Cambio de DB/API/formato persistido con compatibilidad/rollback relevantes → `sdd-migration`.
   - Mejora interna sin cambio observable → `sdd-refactor`.
   - Calidad/mantenibilidad → `sdd-review quality`.
   - Cumplimiento spec/plan → `sdd-review implementation`.
   - Seguridad → `sdd-review security`.
   - Dependencias → `sdd-review dependencies`.
   - Documentación desincronizada → `sdd-doc-sync`.
   - Preparar merge/deploy/release → `sdd-release`.
3. Si es desarrollo normal, usa la tabla de transiciones.
4. Responde de forma breve: estado, motivo y skill siguiente.

## Tabla de transiciones

| Estado | Siguiente paso |
|---|---|
| Sin constitution aprobada y sin brief | `sdd-init` |
| Brief presente, constitution no aprobada | `sdd-constitution` |
| Constitution aprobada, sin AGENTS.md | `sdd-agents` |
| Sin spec en curso | `sdd-spec` |
| Spec `BORRADOR` | `sdd-clarify` |
| Spec `CLARIFICADA`, aprobación pendiente | Gate humano en `sdd-clarify` |
| Spec aprobada sin plan vigente | `sdd-plan` |
| Plan vigente sin trace PASS vigente | `sdd-trace` |
| Trace PASS sin tasks vigentes | `sdd-tasks` |
| Tasks con pendientes | `sdd-implement T-XXX` |
| Tasks completas sin validación vigente | `sdd-validate` |
| `SPEC CUMPLIDA: SÍ` | Cerrada; mantenimiento según clasificación |

## Formato de bloqueo

```text
<ACCIÓN> BLOQUEADA
Motivo: <precondición ausente>
Falta: <concreto>
Siguiente paso: <skill>
```

## Salida

Máximo ~6 líneas por defecto. No expliques el framework salvo petición.

## Acciones prohibidas

- Implementar, corregir o editar artefactos.
- Saltar gates.
- Usar la conversación como sustituto de archivos actuales.
