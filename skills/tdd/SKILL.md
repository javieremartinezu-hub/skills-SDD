---
name: tdd
description: Rigorous, pragmatic TDD workflow for software development. Use for new features by default, bug investigation/fixes when regression tests add value, and other code changes when TDD materially improves correctness, confidence, design, or regression prevention. Not needed for docs-only, generated/static assets, trivial config, mechanical no-behavior refactors, or tiny changes already well covered.
---

# TDD

Use TDD where it creates engineering value, not mere process. Optimize for maximum confidence per token and tool call.

## When Active

- Feature work: default to `UNDERSTAND → RED → GREEN → REFACTOR → VERIFY`.
- Bugs: prefer `UNDERSTAND → REPRODUCE → RED → ROOT CAUSE → GREEN → REFACTOR → VERIFY`.
- Skip or keep lightweight when added workflow cost exceeds likely confidence/design benefit.

## Workflow

### UNDERSTAND
Identify the observable behavior, relevant implementation, existing tests, dependencies, affected boundaries, and project test conventions. Use minimal context. Use Graphify for non-trivial structure, dependency, impact, call-flow, or test-location discovery. Avoid broad exploration.

### RED
Write the smallest meaningful failing test for the required behavior or regression. Prefer observable behavior over implementation details. Confirm it fails for the expected reason before production changes.

### REPRODUCE / ROOT CAUSE
For bugs, reproduce the failure whenever practical before editing production code. After reproduction, identify and fix the underlying cause; do not patch symptoms when the cause can reasonably be corrected.

### GREEN
Make the smallest correct production change to pass the test. Avoid unrelated refactors, speculative abstractions, new dependencies, architecture changes, or future requirements. Run the narrowest relevant tests needed to establish green.

### REFACTOR
Refactor only after green and only when it materially improves the code: meaningful deduplication, clearer names, simpler control flow, or better cohesion. Keep tests green.

### VERIFY
Run checks proportional to risk: relevant tests, typecheck, lint, build/compile when applicable. Inspect the final diff and detect unrelated changes. Prefer targeted verification; widen only when impact or project conventions justify it. Never claim unrun checks passed.

## Test Quality

Tests should be deterministic, behavior-focused, understandable, appropriately isolated, and resistant to irrelevant implementation changes. Use the lowest-cost test level that provides sufficient confidence. Avoid excessive mocking, implementation-detail assertions, duplicated production logic, and unnecessary integration scope.

## Coordination

This skill owns the TDD workflow. Do not duplicate codebase-discovery or frontend-design roles; invoke `frontend-design` only when UI design decisions are required. For UI implementation, use the current `ui-spec.md` as the behavioral/visual contract and rely on `sdd-browser-review` for rendered UI evidence.
