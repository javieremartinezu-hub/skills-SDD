---
name: sdd-doc-sync
description: Sincroniza únicamente documentación afectada después de cambios ya implementados y validados. Detecta qué documentación quedó desactualizada y aplica cambios mínimos sin reescribir contenido no relacionado ni alterar código.
---

# sdd-doc-sync — Sincronización documental

## Principio

Actualizar solo documentación realmente afectada por cambios de comportamiento, API, configuración, operación o arquitectura.

## Procedimiento

1. Lee diff/cambios recientes y artefactos SDD relacionados.
2. Localiza documentación que describe directamente lo cambiado: README, docs, ejemplos, API docs, runbooks, configuración, comentarios contractuales.
3. Modifica solo secciones desactualizadas; no “embellezcas” documentación ajena al cambio.
4. Valida enlaces/generadores/checks de docs existentes cuando estén disponibles.
5. Confirma que la documentación coincide con spec y comportamiento implementado.

## Salida

```text
DOC-SYNC: COMPLETADO | SIN CAMBIOS | BLOQUEADO
ARCHIVOS: <breve>
CHECKS: <resultado>
SIGUIENTE: <ninguno o skill>
```

## Prohibido

- Cambiar código de producto.
- Inventar comportamiento no presente en spec/implementación.
- Reescribir documentación no afectada.
