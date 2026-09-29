---
name: sdd-browser-review
description: Verifica mediante navegador la interfaz web realmente renderizada contra la UI Spec y los flujos de aceptación.
---
# sdd-browser-review — Browser Verification

## Propósito
Demostrar que la aplicación real, no solo su código, cumple la UI Spec y los criterios funcionales visibles.

## Obligatorio cuando
- cambia una pantalla;
- cambia layout/CSS;
- cambia un componente visible;
- cambia navegación;
- cambia formularios o interacción;
- cambia estados visibles;
- cambia comportamiento que pueda afectar UI.

## Procedimiento
1. Obtener URL/entrypoint y entorno.
2. Abrir la aplicación con el navegador disponible para ZCode.
3. Ejecutar los flujos definidos por la spec.
4. Verificar contenido, layout, componentes y estados.
5. Verificar interacciones reales.
6. Verificar consola/network cuando aporte evidencia.
7. Probar viewports definidos por UI Spec.
8. Comprobar accessibility aplicable.
9. Capturar screenshots/evidencia.
10. Comparar cada criterio UI-XXX y RF visible con lo observado.
11. Registrar PASS/FAIL y severidad.

## No confiar únicamente en
- inspección del código;
- snapshots unitarios;
- DOM esperado;
- afirmaciones del agente.

## Matriz mínima de evidencia
| ID | URL | Viewport | Pasos | Esperado | Observado | Resultado | Evidencia |
|---|---|---|---|---|---|---|---|

## Responsive
Si UI Spec define viewports, deben probarse. Si no los define y el producto es responsive, documentar los viewports elegidos.

## Estados
Probar o provocar los estados definidos: loading, empty, success, error, permission denied, validation, offline/stale, disabled, etc.

## Accessibility
Comprobar cuando aplique keyboard, focus, labels/names, roles, contraste, errores y reduced motion. No afirmar conformidad formal con WCAG sin una evaluación suficiente.

## Criterio
Una discrepancia contra un requisito aprobado es FAIL aunque el código sea técnicamente válido.

## Salida
`BROWSER REVIEW: PASS|FAIL` + matriz de evidencia + screenshots/referencias + hallazgos.
