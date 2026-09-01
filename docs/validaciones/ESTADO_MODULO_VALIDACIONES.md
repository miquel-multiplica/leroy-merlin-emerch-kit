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
- **Hub de auditorías**: tabla (perímetro con tipo como subtexto · referencias · Health Score · tendencia · alertas · última ejecución · creador · estado), pestañas **Todo · En curso · Pendientes de revisión · Revisadas · Borradores** (con **contador** en cada una), filtro de tipo + buscador. **Acciones de fila por estado**:
  - *En curso*: **% junto al tag**; en hover, botón **Cancelar** (rojo); kebab **Ver detalle / Cancelar**.
  - *Pendiente de revisión*: **Revisar** (CTA).
  - *Revisada*: **Ver informe**; kebab **Ver informe / Re-auditar / Exportar / Eliminar**.
  - *Borrador*: fila con **Continuar** (verde) + **papelera** (eliminar), siempre visibles, **sin ficha**.
- **Nueva auditoría**: **modal de tipos** (Gama · Proveedor/Seller · Modelo · Categoría web · Listado) → **wizard de 2 columnas** ("Tu selección"): selector + detección de referencias (buscador/lista, URL con Consultar, subida de CSV con nombre editable), caja azul "¿Qué se validará?".
- **Ejecución**: modal con **barra de progreso global**, **cancelable** (con confirmación — dentro del funnel vuelve al wizard con el perímetro), **continuar en segundo plano** y **"Ver proceso en detalle"** (abre la ficha del informe *En curso*). Sin pausa.
- **Estados de la auditoría**: **En curso · Pendiente de revisión · Revisada · Borrador · Error**. **Borrador** = auditoría *En curso* cancelada desde el **hub o su ficha** (guarda el perímetro; el modal avisa de que queda como borrador). Cancelar **dentro del funnel** no crea borrador (te deja en el wizard).
- **Detalle de auditoría** (`v-informe`, una sola pantalla condicional por estado):
  - **En curso** → **vista de progreso** (contador de referencias analizadas · % · barra · botón **Cancelar**), sin métricas que dependan del fin.
  - **Pendiente de revisión / Revisada**:
    - Cabecera: título = perímetro, badge, meta ("**Auditoría de** gama/modelo/…"), **Health Score** arriba-derecha — **provisional en gris** (sin flecha de tendencia) mientras *Pendiente de revisión*; en *Revisada*, con **color** y copy en bold/oscuro; **tooltip estilado** (`.lm-tooltip`, abre hacia abajo) con texto distinto según provisional/revisado. **En las filas del hub** el HS provisional también sale **gris y sin flecha**. **cajitas** de estado en **4 niveles excluyentes** (Sin errores / Con errores leves / Con errores críticos / **No publicable**) + por tipo (discrepancias / ortografía), con tooltip.
    - **Impacto por categoría de error** y **Distribución de errores** → **dropdowns colapsables**. La Distribución tiene 3 columnas (Designación · Descripción · Ficha técnica, con **Multimedia** anidada bajo Ficha técnica), barritas **coloreadas por severidad** y **ordenadas** por gravedad, con leyenda.
    - **Paleta única de severidad** en toda la vista: **Leve = dorado** · **Crítica = rojo oscuro** · **Bloqueante/No publicable = rojo** (WCAG AA).
    - **Matriz de correcciones**: filtros con placeholders cortos que **adaptan su ancho al texto** (Seller · Owner · Severidad · Tipo de error **agrupado por área** con todos los tipos · Segmento · búsqueda) + **"Ordenar por"** (Severidad/Referencia/Tipo/Owner/Segmento) + **toggle "Falsos positivos (N)"** (aislado a la derecha, con contador). Paginada.
    - **Falsos positivos** (la **IA no los prescribe: los detecta el humano**). Modal **"Selecciona el motivo del falso positivo"**: **cajita resumen** (fondo gris, **colapsable**) con la **referencia enlazada a la PDP** + modelo, **tipo**, **severidad** y **disparador**; **motivos** (chips) + **comentario**. Los checks de IA llevan un **badge IA** (origen de la alerta, no un veredicto). En la matriz cada fila lee como **error a corregir**; la validez la decide el revisor.
      - **En bloque (Familia → Modelo → Referencia):** si la alerta comparte **disparador** con otras, **encima de los motivos** aparece un aviso *"Hay N referencias más con el mismo error en M modelos"* + **Revisarlas** → **sub-vista "Marcar falso positivo en bloque"** dentro del mismo modal (**← Atrás** al pie, sin overlay apilado). La **referencia original** va arriba **marcada y deshabilitada** (siempre entra); las coincidencias **agrupadas por modelo** (el de la original **primero** + separador *"Otros modelos"*), **desmarcadas por defecto**, con **enlace a PDP** y **seller**, **filtro por seller** transversal, bloques **colapsables** (fila clicable, área del check ampliada) → **Confirmar selección**. De vuelta, el aviso pasa a **cajita con borde verde** *"Se aplicará a M modelos · N referencias"* + **lápiz**; el botón final es **dinámico** *"Marcar N falsos positivos"*.
      - Salen del informe al **cajón de falsos positivos** con **Motivo / Editar / Restaurar** (**restaurar** un bloque pregunta si deshacer las demás).
      - **Hipótesis de elegibilidad** (no confirmada, ver pendiente 16): checks de **juicio** (discrepancias, ortografía, SEO, administrativa) llevan botón de falso positivo; **ausencias objetivas** (sin descripción/designación, atributos vacíos, imágenes, longitud) no.
  - **Pie**: *Pendiente de revisión* → **"Marcar como revisado"** (modal de confirmación con cifras: **hallazgos válidos** —barrita roja— vs. **falsos positivos descartados** —barrita **gris**—; barritas a la altura del texto). *Revisada* → **Re-auditar** (icono) + **Exportar informes** (icono).
- **Generar informes** (modal, cierra con **X**): tres informes con botón **Descargar** — **seller/proveedor** (PDF+CSV), **interno TIP** (matriz), e **informe de falsos positivos** (Excel/CSV, para el admin/motor).
- **Configuración** (admin): **enlace** (verde, icono ajustes) junto a "Nueva auditoría" → panel admin (pantalla vacía de momento, "próximamente").

## Pendiente con cliente (decisiones/datos) — detalle en §5 del análisis
1. ✅ **Acceso a datos (BQ 1P)** — como en guías; universo = **modelos con guía eMerch**; expone atributos, seller/proveedor. **Solo publicadas**; **campo ADM sí** (fuente complementaria). *(Falta el detalle fino de campos/mapeo PLP↔refs al integrar.)*
2. Motor actual: walkthrough y si ya usa LLM.
3. Recurrencia: **manual** (relanzan cuando quieren). Quieren **histórico**: evolución del Health Score por auditoría + correcciones en el tiempo → **nos piden proponerlo**.
4. Formato/granularidad del informe → nos piden **propuesta**, partiendo de su PDF.
5. "Listado": **empezar con CSV de referencias**; a futuro, conectar y elegir modelo (auto-auditoría).
6. ✅ **Gama** — 14 valores reales (M,A,B,C,D,E,S,Z,T,L,R,P,K,V), **uno por referencia**. Aplicado en el proto.
7. ⚠️ **Atributos obligatorios reencuadrados.** La obligatoriedad de la guía se marca en los **atributos de ficha técnica** → faltar uno = **Crítica/Leve** (calidad de ficha viva), **no** "candidata a despublicar". *(Pendiente: si "obligatorio de guía" incluye también los atributos que la guía exige en designación/descripción, y con qué severidad si faltan.)*
8. ✅ **Alcance BQ** — **solo publicadas**; **ADM expuesto** (complementario).
9. ✅ **Owner de estructura de designación = TIP** (aplicar en proto).
10. Normalización del Health Score en MVP → ver 11.
11. ✅ **Health Score = viene dado** (nos pasan el valor; contacto **Javier Hernán**). **Fleco en el proto:** quitar "provisional en gris + recálculo por falsos positivos" → mostrar su valor + tendencia. **Pendiente (Javier):** ¿se calcula en vivo al auditar (snapshot) o es un valor pre-calculado?
12. Granularidad de exports (asunción): seller = por referencia, TIP = por alerta.
13. **Modelo de 4 cajas** — cajitas OK visualmente, pero **publicar = oferta + imagen** (regla de plataforma) y **despublicar es MANUAL** (emerchand/TIP). **Hay fichas publicadas que no deberían estarlo** → **se mantiene el nombre "No publicable"**, pero su lectura es: *publicada que no debería estarlo* (recomendación para despublicar manualmente, no un estado automático de plataforma). El reparto en tiers depende de la escala (ver 20).
14. Flujo falsos positivos → panel del administrador (con el export o con otro método): **pendiente**; nos pidieron el **enlace del prototipo** para verlo. *(No es reunión.)*
15. Re-auditoría: modelo del loop y transiciones (cancelar re-auditoría NO crea borrador; vuelve a Revisada).
16. ¿Qué tipos de check admiten falso positivo? **No decidido → meet.** (Hipótesis proto: solo los de juicio.)
17. Datos del falso positivo en bloque (disparador + Familia→Modelo→Referencia + seller + equivalencia) — **lo tratan en un meet**.
18. Nombre final del concepto: ¿"Auditorías" o "Validaciones"?
19. Subsección: descartada (solo "Sección").
20. ⚠️ **Escala de criticidad SIN DEFINIR** — el cliente pide *"establecer una escala de criticidad"*: qué **tipo de error → qué tier**, y cuáles llegan a **"candidata a despublicar"**. Es el **núcleo del módulo**; nuestra Bloqueante/Crítica/Leve es **propuesta**. Probablemente **sesión de trabajo**.

### Preguntas redactadas al cliente
**Por escrito (confirmables):**
- **Obligatorios de la guía:** ¿solo los atributos de la **ficha técnica**, o también los que la guía exige en **designación/descripción**? ¿qué gravedad si faltan estos últimos?
- **Health Score (Javier):** ¿se **calcula al lanzar la auditoría** o lo **actualizáis vosotros**? ¿nos dais **fecha/hora de última actualización** por referencia?
- **"No publicable":** además de faltar **designación/descripción**, ¿hay otros mínimos (p. ej. **por debajo del mínimo de imágenes**) que hagan que una ficha no debería estar publicada?

**Para la sesión de trabajo:**
- **Escala de criticidad:** qué **tipo de error → qué nivel**, y cuáles llegan a **"No publicable"**.
- **Falsos positivos — elegibilidad:** ¿qué **tipos de error** admiten falso positivo?
- **Falsos positivos — datos del bloque:** ¿el motor puede exponer el **disparador**, la jerarquía **Familia→Modelo→Referencia** y el **seller**? ¿cómo se calcula la **equivalencia**?

## Pendiente de prototipo (por maquetar)
- ~~**Estado *En curso***~~: **resuelto** — vista de progreso (contador · % · barra · Cancelar).
- ~~**Estado *Error***~~: **resuelto** — caja de aviso (conexión perdida, lo analizado no se pierde) + **Reintentar**.
- ~~**Revisado vs. Finalizada**~~: **resuelto** — fusionados en **Revisada**.
- ~~**Generar exports**~~: **resuelto** — modal "Generar informes" con 3 informes + Descargar. *Falta el detalle real del contenido de cada informe/CSV.*
- ~~**Cancelar auditoría En curso**~~: **resuelto** — desde hub y ficha (→ Borrador); pestaña Borradores.
- ~~**Falsos positivos en bloque**~~: **resuelto** — modal *"Selecciona el motivo…"* (resumen colapsable + referencia enlazada) + sub-vista *"Marcar falso positivo en bloque"* (Familia→Modelo→Referencia): original fija, modelos con separador *Otros modelos*, filtro por seller, estados del scope card (aviso vs. verde + lápiz), botón dinámico *"Marcar N falsos positivos"*, restaurar en bloque. *(Elegibilidad de checks sin confirmar — §5.16; dependencia de datos — §5.17.)*
- ~~**Health Score provisional**~~: **resuelto** — en **gris** (sin flecha, también en filas del hub) mientras *Pendiente de revisión*; **color + bold** en *Revisada*; **tooltip estilado**.
- **Detalle/contenido real de los exports** (informe seller/proveedor PDF, CSVs, informe interno TIP, informe de falsos positivos): **mapeado en §4** del análisis, **falta maquetarlo**.
- **Re-auditoría** + comparación **Corregidos / Persisten / Nuevos** (hoy es un placeholder/alert). **Ojo:** las **transiciones de estado en re-auditoría** están pendientes (§5.15) — **cancelar una re-auditoría NO debe crear borrador** (vuelve a *Revisada*, informe previo intacto); el borrador solo aplica a primeras auditorías. En código, el cancelar deberá ramificar según *¿hay ejecución previa completada?*.
- **Panel de Configuración del motor (admin)**: solo está el enlace (sin contenido aún). Aquí aterriza la cola de falsos positivos.
- **Flujo falsos positivos → admin**: cómo se materializa (¿informe de falsos positivos descargable vs. conexión directa?) — ver §5.14. De ello depende el copy de los modales de falsos positivos y de "¿Marcar como revisado?".

*(Nota: **no** hacemos "detalle de referencia" interno. Igual que el artefacto del cliente, la referencia **enlaza a la ficha real de Leroy (PDP en vivo)**, que es la fuente de verdad para revisar falsos positivos.)*

## Infra / repo
- **GitHub Pages** vía **GitHub Actions** (`concurrency: cancel-in-progress: false` + `workflow_dispatch`). Deploy **encolado por incidencia de GitHub** (ago 2026); se publica solo al resolverse. Código a salvo en `main`.
- Prototipo servido en raíz: `wireframe_validaciones.html`. **Material fuente del cliente** en `docs/validaciones/docs de cliente/` — **gitignored** (no se versiona ni se publica en Pages).
