---
name: sdd-plan
description: Transforma una spec APROBADA en diseño técnico specs/NNN-slug/plan.md, actuando como arquitecto de software (arquitectura, stack, componentes, modelo de datos, APIs, seguridad, errores, observabilidad, migraciones, despliegue, rollback). Registra cada decisión importante con motivo, alternativas y RF que cubre. Considera simplicidad y facilidad de trabajo con agentes, nunca la moda. NO escribe código. Úsala solo cuando la spec tenga Estado APROBADA y SPEC CLARIFICADA: SÍ.
---

# sdd-plan — Diseño técnico (CÓMO)

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Convertir la spec APROBADA en un diseño técnico completo y justificado en `specs/NNN-slug/plan.md`.

## Alcance

- Diseña y decide CÓMO se construirá.
- NO escribe código de producto ni tests. NO modifica la spec (si detectas un problema funcional en ella, detente y enruta a `sdd-change`).

## Cuándo usar

- Tras la aprobación humana de la spec (`Estado: APROBADA` + `SPEC CLARIFICADA: SÍ`).
- Tras un cambio aprobado que requiere actualizar el diseño.

## Precondiciones (todas obligatorias)

1. `docs/constitution.md` con `Estado: APROBADA`.
2. `specs/NNN-slug/spec.md` con `Estado: APROBADA` y `Aprobación: APROBADA (fecha, por…)`.
3. `clarify.md` con `SPEC CLARIFICADA: SÍ`.
4. Cero `[NECESITA ACLARACIÓN` en spec.md.

Si falta cualquiera: `PLAN BLOQUEADO — <lo que falta>. Siguiente paso: sdd-clarify` (o `sdd-spec`).

## Contexto requerido

- `docs/constitution.md`, `AGENTS.md`, `specs/NNN-slug/spec.md`.
- Inspección del repositorio: stack existente, convenciones, estructura (para diseñar dentro de ellas o justificar desviaciones).

## Entradas

- La spec aprobada. Ningún requisito fuera de ella.

## Procedimiento

1. Verifica las precondiciones leyendo los archivos.
2. Redacta `specs/NNN-slug/plan.md` con la plantilla. Para cada apartado marca `Aplica` o `No aplica (motivo)`; no dejes secciones sin evaluar.
3. **Registros de decisión.** Toda decisión importante (arquitectura, stack, persistencia, contratos, seguridad, etc.) se documenta con el formato completo de 6 campos. "Importante" = difícil de revertir, afecta a varios RF o introduce una dependencia.
4. **Selección de tecnologías.** Evalúa, explícitamente cuando elijas: requisitos de la spec · simplicidad · mantenibilidad · seguridad · rendimiento · madurez · comunidad · disponibilidad de librerías · facilidad de trabajo mediante agentes IA · despliegue · complejidad operativa. No elijas por moda ni por preferencia personal no justificada. Respeta las restricciones de producto declaradas en la spec.
5. Referencia cada sección del plan a los RF que cubre (cruce implícito que `sdd-trace` verificará).
6. Muestra el plan. Recomienda: `Siguiente paso: sdd-trace`.

### Formato de registro de decisión

```markdown
### D-NNN — <título>
- DECISIÓN: <qué se decide>
- MOTIVO: <por qué>
- ALTERNATIVAS CONSIDERADAS: <lista>
- ALTERNATIVA DESCARTADA: <cuál y en qué consistía>
- POR QUÉ SE DESCARTA: <razón concreta>
- RF QUE CUBRE: RF-XXX, RF-YYY
```

### Plantilla de `specs/NNN-slug/plan.md`

```markdown
# Plan técnico — NNN-slug

> Basado en spec.md v<N> (APROBADA). No implementa código.

## 1. Arquitectura (Aplica/No aplica + motivo)
## 2. Stack y tecnologías (con registros de decisión D-NNN)
## 3. Estructura del repositorio y componentes
## 4. Responsabilidades por componente
## 5. Modelo de datos
## 6. APIs y contratos
## 7. Autenticación y autorización
## 8. Seguridad y amenazas consideradas
## 9. Manejo de errores
## 10. Logging y observabilidad
## 11. Configuración
## 12. Estrategia de tests (tipos y qué demostrará cada uno, por RF)
## 13. Migraciones
## 14. Despliegue, rollback y backups
## 15. Rendimiento y escalabilidad
## 16. Cobertura prevista de RF
| RF | Secciones del plan que lo cubren |
## Registro de decisiones
```

## Artefactos de salida

- `specs/NNN-slug/plan.md`

## Validación

- Todas las precondiciones verificadas y citadas.
- Cada decisión importante tiene su registro D-NNN completo (6 campos).
- No contradice ningún RF ni la constitution.
- No contiene código de producto.

## Condiciones de parada

- Si detectas que la spec es incorrecta o incompleta para diseñar: DETENTE y enruta `sdd-change` (spec antes que plan).
- Tras escribir y mostrar el plan: DETENTE. Recomienda `sdd-trace`.

## Acciones prohibidas

- Escribir código de producto o tests ejecutables.
- Cambiar requisitos o añadir comportamiento no presente en la spec.
- Omitir la justificación de decisiones importantes o de dependencias nuevas.
- Modificar spec.md directamente (enrutar a `sdd-change`).

## Siguiente fase permitida

`sdd-trace`
