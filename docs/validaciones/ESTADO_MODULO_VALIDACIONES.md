# Validaciones — Estado del módulo y pendientes

> Contexto entre sesiones del módulo **Validaciones** (Auditor de Calidad de Producto). Recoge qué hay montado en el prototipo `wireframe_validaciones.html` y la lista de pendientes. El análisis funcional completo está en [ANALISIS_VALIDACIONES.md](ANALISIS_VALIDACIONES.md) (los pendientes con cliente son su §5).

## Para el equipo que construye el motor (dev / IA)
El **motor lo diseña e implementa el equipo**; estos docs no dan su arquitectura, sino **qué se ha construido, qué reglas debe cumplir y qué falta**. Ruta de entrada:
1. **Abrir `wireframe_validaciones.html`** — es la **UX construida** y la fuente de verdad de comportamiento (pantallas, informe, matriz, estados, filtros/orden).
2. **`ANALISIS_VALIDACIONES.md` §2** — las **reglas que el motor debe cumplir**: taxonomía de checks (tipo → categoría), **severidad** (Bloqueante/Crítica/Leve), **origen** (Guía / General), **motor** por check (determinista / comparación / LLM), y el **cálculo del Estado** (4 cajas con precedencia: Bloqueante→No publicable · Crítica/3+→críticos · Leve→leves).
3. **`ANALISIS` §5** — lo que está **bloqueado por decisión/dato de cliente**.

**Se puede empezar ya:** checks **deterministas** de guía (atributos exigidos, concatenación de la designación, nº de imágenes vs. mínimo) y generales (longitud, unidades, administrativa, ortografía); el **esquema de alerta** (implícito en las columnas de la matriz: ref · modelo · tipo · categoría · diagnóstico · corrección · severidad · origen · owner · segmento · campo/ubicación); y el **cálculo del Estado**.
**Bloqueado / pendiente:** **Health Score** (cómo se calcula/normaliza o si viene de origen — §5.10/11), **campos exactos de BigQuery** (§5.1/8), checks **LLM** (color/material, SEO en texto, coherencia semántica) y **análisis de contenido de imagen** (fase 2).

## Qué hay montado (prototipo)
- **Hub de auditorías**: tabla (perímetro con tipo como subtexto · referencias · Health Score · tendencia · alertas · última ejecución · creador · estado), pestañas **Todo · En curso · Pendientes de revisión · Finalizadas**, filtro de tipo + buscador con lupa, y **acciones de fila** (CTA en hover + menú kebab: Ver informe / Re-auditar / Exportar / Eliminar).
- **Nueva auditoría**: **modal de tipos** (Gama · Proveedor/Seller · Modelo · Categoría web · Listado) → **wizard de 2 columnas** ("Tu selección"): selector + detección de referencias (buscador/lista, URL con Consultar, subida de CSV con nombre editable), caja azul "¿Qué se validará?".
- **Ejecución**: modal con **barra de progreso global**, **cancelable** (con confirmación, vuelve al wizard) y **continuar en segundo plano** (aparece como *En curso*). Sin pausa.
- **Detalle de auditoría** (`v-informe`, una sola pantalla condicional por estado):
  - Cabecera: título = perímetro, badge de estado, meta en una línea, **Health Score** arriba-derecha; **cajitas** de estado en **4 niveles excluyentes** (Sin errores / Con errores leves / Con errores críticos / **No publicable**) + por tipo (discrepancias / ortografía, solapan), con tooltip.
  - **Impacto por categoría de error** y **Distribución de errores** → **dropdowns colapsables** (la matriz cobra protagonismo). Las barritas de Distribución van **coloreadas por severidad** y **ordenadas** por gravedad (Bloqueante → Crítica → Leve), con leyenda.
  - **Paleta única de severidad** en toda la vista (chips de la matriz + cajitas de estado + barritas): **Leve = dorado** · **Crítica = rojo oscuro** · **Bloqueante/No publicable = rojo** (contraste WCAG AA verificado).
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
- ~~**Revisado vs. Finalizada**~~: **resuelto** — fusionados en **Revisada** (estado terminal único; "Finalizada" no correspondía a ninguna acción de la herramienta).
- **Re-auditoría** + comparación **Corregidos / Persisten / Nuevos** (hoy es un placeholder/alert).
- **Generar exports** (pantalla por audiencia): hoy solo una modal con placeholders. Export **interno TIP** = matriz enriquecida (por alerta); export **seller** = por referencia (con columna Campo/ubicación en el CSV).
- **Matriz**: **agrupar por referencia/modelo** (colapsable) → así se ven juntas todas las alertas de un mismo SKU (la matriz es por alerta).
- **Configuración del motor (admin)**: sin maquetar.

*(Nota: **no** hacemos "detalle de referencia" interno. Igual que el artefacto del cliente, la referencia **enlaza a la ficha real de Leroy (PDP en vivo)**, que es la fuente de verdad para revisar falsos positivos. Pendiente: usar la **URL real de la PDP** en el enlace, no la placeholder actual.)*

## Infra / repo
- **GitHub Pages** vía **GitHub Actions** (`concurrency: cancel-in-progress: false` + `workflow_dispatch`). Deploy **encolado por incidencia de GitHub** (ago 2026); se publica solo al resolverse. Código a salvo en `main`.
- Prototipo servido en raíz: `wireframe_validaciones.html`. **Material fuente del cliente** en `docs/validaciones/docs de cliente/` — **gitignored** (no se versiona ni se publica en Pages).
