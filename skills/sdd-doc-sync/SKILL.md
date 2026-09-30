---
name: sdd-doc-sync
description: Sincroniza únicamente documentación afectada por una implementación ya validada. Persiste doc-sync.md, valida checks de documentación disponibles y no modifica código ni requisitos.
---

# sdd-doc-sync — Documentation Sync

## Precondiciones

- `validation.md` con `SPEC CUMPLIDA: SÍ`.
- `spec.md` vigente.

## Procedimiento

1. Lee diff final y artefactos SDD relacionados.
2. Localiza documentación directamente afectada: README, docs, ejemplos, API docs, runbooks, configuración y comentarios contractuales.
3. Actualiza solo lo desactualizado.
4. Ejecuta validadores/generadores/enlaces de docs existentes en el proyecto; no inventes comandos.
5. Escribe `specs/NNN-slug/doc-sync.md`.
6. Emite `DOC-SYNC: COMPLETADO | SIN CAMBIOS | BLOQUEADO`.
7. DETENTE.

## Prohibido

- Cambiar código de producto.
- Inventar comportamiento.
- Reescribir documentación ajena al cambio.

## Siguiente fase

`sdd-release`.
