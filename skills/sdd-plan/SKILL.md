---
name: sdd-plan
description: >-
  Transforma una spec APROBADA en diseño técnico specs/NNN-slug/plan.md, actuando como arquitecto de software (arquitectura, stack, componentes, modelo de datos, APIs, seguridad, errores, observabilidad, migraciones, despliegue, rollback). Registra cada decisión importante con motivo, alternativas y RF que cubre. Considera simplicidad y facilidad de trabajo con agentes, nunca la moda. NO escribe código. Úsala solo cuando la spec tenga Estado APROBADA y SPEC CLARIFICADA: SÍ.
---

# sdd-plan — Diseño técnico (CÓMO)

## Convenciones comunes del framework (léelas primero)

- La fuente de verdad son los **artefactos en el repositorio**, nunca la conversación. Relee los archivos antes de decidir.
- Artefactos: `docs/constitution.md` · `AGENTS.md` · `specs/NNN-slug/{spec,clarify,plan,trace,tasks,validation}.md`
- Veredictos: `SPEC CLARIFICADA: SÍ|NO` · `TRAZABILIDAD: PASS|FAIL` · `SPEC CUMPLIDA: SÍ|NO`.
- La cabecera canónica de `spec.md` es la tabla `| Campo | Valor |`: consulta sus filas `Estado`, `Aprobación` y `Versión`; nunca busques `Estado: …` como texto libre.
- Formato de bloqueo: `<ACCIÓN> BLOQUEADA / Motivo / Falta / Siguiente paso: <skill>`

## Propósito

Convertir la spec APROBADA en un diseño técnico completo y justificado en `specs/NNN-slug/plan.md`.

## Alcance

- Diseña y decide CÓMO se construirá.
- NO escribe código de producto ni tests. NO modifica la spec (si detectas un problema funcional en ella, detente y enruta a `sdd-change`).

## Cuándo usar

- Tras la aprobación humana de la spec (fila `Estado` en `APROBADA` + `SPEC CLARIFICADA: SÍ`).
- Tras un cambio aprobado que requiere actualizar el diseño.

## Precondiciones (todas obligatorias)

1. `docs/constitution.md` con `Estado: APROBADA`.
2. `specs/NNN-slug/spec.md` con las filas `Estado` en `APROBADA` y `Aprobación` en `APROBADA (fecha, por…)`.
3. `clarify.md` con `SPEC CLARIFICADA: SÍ`.
4. Cero `[NECESITA ACLARACIÓN` en spec.md.

Si falta cualquiera: `PLAN BLOQUEADO — <lo que falta>. Siguiente paso: sdd-clarify` (o `sdd-spec`).

## Contexto requerido

- `docs/constitution.md`, `AGENTS.md`, `specs/NNN-slug/spec.md`.
- Inspección del repositorio: stack existente, convenciones, estructura (para diseñar dentro de ellas o justificar desviaciones).

## Entradas

- La spec aprobada. Ningún requisito fuera de ella.

## Procedimiento

1. Verifica las precondiciones leyendo los archivos. Si existe un plan de una versión anterior de la spec, lee la última fila del `Registro de cambios` de spec.md para obtener la clasificación persistida `[LOCAL]` o `[ESTRUCTURAL]` y evalúa la compatibilidad con el diseño existente.
   - **Modo DELTA/COMPATIBILIDAD:** si el cambio es LOCAL y no exige decisiones técnicas nuevas, conserva el diseño, actualiza el vínculo `Spec-Version`, incrementa `Plan-Version` y añade una nota breve de compatibilidad indicando RF afectados y por qué el diseño sigue siendo válido. No reescribas secciones sin cambios.
   - **Modo COMPLETO:** si el cambio es ESTRUCTURAL o exige decisiones nuevas, actualiza las secciones afectadas y sus registros de decisión.
   Si ya existe un plan para esta misma versión de spec, incrementa su `Plan-Version`.
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

> Spec-ID: NNN-slug · Spec-Version: N · Plan-Version: M · Estado de la spec: APROBADA
>
> No implementa código. Toda revisión debe conservar estos vínculos para que `sdd-trace` pueda detectar artefactos caducados.

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
## 13. Migraciones (detalle; el estado canónico también aparece en §16)
## 14. Despliegue, rollback y backups
## 15. Rendimiento y escalabilidad
## 16. Migraciones y compatibilidad
- `Migration Required: YES | NO`
- Clasificación prevista: `COMPATIBLE | POR FASES | BREAKING | N/A`
- Estrategia: …
- Rollback: …

## 17. UI / Browser Planning (si aplica)
- `UI Verification Required: YES | NO`
- `UI Review Required: YES | NO`
- `Browser Review Required: YES | NO`
- Viewports: …
- Flujos críticos: …
- Estados a verificar: …
- Evidencia esperada: …

## 18. Cobertura prevista de RF
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

## UI y browser planning

Si la feature tiene UI, el plan debe incluir:
- componentes/patrones afectados;
- Design System/tokens;
- estados;
- responsive;
- accesibilidad;
- estrategia de browser verification;
- viewports;
- flujos críticos;
- evidencia esperada.

La trazabilidad UI debe ser `UI-XXX → plan → task → componente → browser evidence`. El plan debe declarar explícitamente si Browser Review es requerido y qué evidencia espera.
