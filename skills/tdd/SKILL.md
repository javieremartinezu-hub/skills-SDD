---
name: tdd
description: Workflow TDD pragmático para features, bugs y cambios donde los tests aportan confianza. Usa UNDERSTAND → RED → GREEN → REFACTOR → VERIFY, con scope mínimo, reuse-first y self-review; evita proceso innecesario para cambios sin comportamiento.
---

# TDD — Test-Driven Development pragmático

## Cuándo usar

- Features: por defecto cuando existe comportamiento verificable.
- Bugs reproducibles: test de regresión antes del fix cuando sea viable.
- Refactors: tests baseline/caracterización para proteger comportamiento.
- Puede ser ligero u omitirse en docs, assets estáticos, config trivial o cambios mecánicos ya cubiertos.

## Workflow

### UNDERSTAND
Entiende comportamiento observable, código relacionado, tests y límites. Define scope inicial y busca implementaciones existentes antes de crear otras.

### RED
Crea el test mínimo significativo. Debe fallar por la razón esperada, no por ruido de setup.

### GREEN
Haz el cambio mínimo correcto. No agregues comportamiento futuro, dependencias, capas o abstracciones innecesarias.

### REFACTOR
Solo después de verde y solo si mejora claramente simplicidad, cohesión, nombres o duplicación. Mantén el scope.

### VERIFY
Ejecuta checks proporcionales al riesgo: tests, typecheck, lint, build/compile y otros realmente disponibles. No inventes comandos ni afirmes checks no ejecutados.

### SELF-REVIEW
Inspecciona el diff: scope, cumplimiento, duplicación, abstracciones innecesarias, contratos, casos borde y calidad real del test.

## Calidad de tests

- Deterministas y centrados en comportamiento.
- Nivel de test más barato que dé confianza suficiente.
- Evita mocks excesivos, assertions sobre detalles internos y duplicar lógica de producción dentro del test.

## Regla de eficiencia

Maximiza confianza por token y tool call. No conviertas TDD en burocracia si no aporta evidencia adicional.
