# Validaciones — Estado del módulo y pendientes

> Contexto entre sesiones del módulo **Validaciones** (Auditor de Calidad de Producto). Recoge qué hay montado en el prototipo `wireframe_validaciones.html` y la lista de pendientes. El análisis funcional completo está en [ANALISIS_VALIDACIONES.md](ANALISIS_VALIDACIONES.md) (los pendientes con cliente son su §5).

## Qué hay montado (prototipo)
- **Hub de auditorías**: tabla (perímetro con tipo como subtexto · referencias · Health Score · tendencia · alertas · última ejecución · creador · estado), pestañas **Todo · En curso · Pendientes de revisión · Finalizadas**, filtro de tipo + buscador con lupa, y **acciones de fila** (CTA en hover + menú kebab: Ver informe / Re-auditar / Exportar / Eliminar).
- **Nueva auditoría**: **modal de tipos** (Gama · Proveedor/Seller · Modelo · Categoría web · Listado) → **wizard de 2 columnas** ("Tu selección"): selector + detección de referencias (buscador/lista, URL con Consultar, subida de CSV con nombre editable), caja azul "¿Qué se validará?".
- **Ejecución**: modal con **barra de progreso global**, **cancelable** (con confirmación, vuelve al wizard) y **continuar en segundo plano** (aparece como *En curso*). Sin pausa.
- **Detalle de auditoría** (`v-informe`, una sola pantalla condicional por estado):
  - Cabecera: título = perímetro, badge de estado, meta en una línea, **Health Score** arriba-derecha; **cajitas** de estado (Sin errores / Con errores leves / Con errores críticos, excluyentes) + por tipo (discrepancias / ortografía, solapan), con tooltip.
  - **Impacto por categoría de error** y **Distribución de errores** → **dropdowns colapsables** (la matriz cobra protagonismo).
  - **Matriz de correcciones**: filtros (seller · severidad · tipo de error · segmento · búsqueda), **paginado**, referencia con enlace a la ficha, y **revisión de falsos positivos** (con motivo). Modos *Pendiente de revisión* (interactivo) y *Revisado* (read-only + exportar).

## Pendiente con cliente (decisiones/datos) — detalle en §5 del análisis
1. Acceso a datos (BigQuery 1P): campos expuestos + mapeo PLP↔referencias.
2. Motor actual: walkthrough y si ya usa LLM.
3. Loop recurrente: hub + ejecuciones + histórico + comparación de periodos.
4. Granularidad/formato del informe.
5. Formato del perímetro "Listado": URLs de PDP (PDF) vs. CSV de SKUs (prototipo).
6. Significado de las letras de gama (A/M/S/C/K/B).
7. Atributos obligatorios: severidad (Bloqueante) vs. prioridad (Media).
8. Alcance BQ: ¿referencias no publicadas? + ¿expone el campo ADM?
9. Owner de "Estructura de designación": Seller (dato) vs. TIP (título).
10. Normalización del Health Score base (solo ~5/10 puntos auditables en MVP).
11. Health Score: ¿calculado por nosotros o ingerido de origen?
12. Granularidad de exports (asunción): seller = por referencia, TIP = por alerta.

## Pendiente de prototipo (por maquetar)
- **Estados del detalle** *En curso* y *Error*: hoy solo **placeholders** → falta la **versión completa** (vista de progreso real / pantalla de reintento).
- **Revisado vs. Finalizada**: decidir si se fusionan en una sola vista.
- **Re-auditoría** + comparación **Corregidos / Persisten / Nuevos** (hoy es un placeholder/alert).
- **Generar exports** (pantalla por audiencia): hoy solo una modal con placeholders. Export **interno TIP** = matriz enriquecida (por alerta); export **seller** = por referencia (con columna Campo/ubicación en el CSV).
- **Detalle de referencia** (abrir ficha): hoy enlace externo fake a leroymerlin.es.
- **Matriz**: agrupar por referencia/modelo (colapsable).
- **Configuración del motor (admin)**: sin maquetar.

## Infra / repo
- **GitHub Pages** vía **GitHub Actions** (`concurrency: cancel-in-progress: false` + `workflow_dispatch`). Deploy **encolado por incidencia de GitHub** (ago 2026); se publica solo al resolverse. Código a salvo en `main`.
- Prototipo servido en raíz: `wireframe_validaciones.html`. **Material fuente del cliente** en `docs/validaciones/docs de cliente/` — **gitignored** (no se versiona ni se publica en Pages).
