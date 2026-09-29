---
name: openjev-decision-gate
description: Usa OpenJEV como capa de decisión estructurada para seleccionar flujo y gates SDD/TDD.
---
# openjev-decision-gate

## Rol
OpenJEV clasifica y decide gates; no escribe código ni inventa requisitos.

## Señales
- flow: new/bug/change/refactor/debug/review/release
- scope: local/multi-component/structural
- risk: low/medium/high/critical
- ui_impact: none/indirect/direct
- browser_verification_required
- responsive_verification_required
- accessibility_verification_required
- visual_regression_required
- security_review_required
- next_skill

## Regla de seguridad
Si OpenJEV no está disponible o devuelve una decisión inválida, usar el routing determinista de `sdd-orchestrator`. Nunca bloquear el desarrollo por depender exclusivamente de OpenJEV.
