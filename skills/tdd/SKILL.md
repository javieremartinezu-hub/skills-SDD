---
name: tdd
description: >-
  Ciclo RED → GREEN → REFACTOR → VERIFICAR con el mínimo coste. Úsala al escribir o corregir código
  con tests, dentro o fuera de SDD.
---

# tdd — Ciclo de desarrollo guiado por tests

Objetivo: máxima confianza por token y por ejecución, no ceremonia.

## Ciclo

- **ENTENDER.** Comportamiento observable, código y tests existentes, convenciones del proyecto. Contexto mínimo; nada de exploración amplia.
- **RED.** El test significativo más pequeño para el comportamiento o la regresión. Confirma que falla por la razón esperada, no por imports o compilación.
- **REPRODUCIR / CAUSA RAÍZ** (bugs). Reproduce antes de tocar producción; corrige la causa, no el síntoma.
- **GREEN.** El cambio de producción mínimo y correcto. Sin refactors ajenos, abstracciones especulativas, dependencias nuevas ni requisitos futuros. Ejecuta solo los tests necesarios para estar en verde.
- **REFACTOR.** Solo con verde y solo si mejora de verdad (duplicación, nombres, flujo, cohesión). Los tests siguen verdes.
- **VERIFICAR.** Checks proporcionales al riesgo, con los comandos de `AGENTS.md` cuando existan. Revisa el diff final: toda línea se traza a la tarea; revierte el resto. Nunca declares como pasado algo no ejecutado.

## Cuándo no aplicar el ciclo completo

Docs, assets generados, configuración trivial o refactors mecánicos ya cubiertos: basta con verificar. En SDD esto debe constar en la tarea como `Tests requeridos: N/A — <motivo>`; no lo decidas tú.

## Calidad de tests

Deterministas, centrados en comportamiento, legibles y resistentes a cambios internos irrelevantes. Usa el nivel de test más barato que dé confianza suficiente; evita mocks excesivos y aserciones sobre detalles de implementación.
