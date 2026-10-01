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
- **Hub de auditorías**: tabla (**ID** · perímetro con tipo como subtexto · referencias · Health Score · tendencia · alertas · **fecha** · creador · estado). El **ID** es la primera columna, con el formato de Descripciones (`#1000+id`, cifras tabulares, color `--text-secondary-dark`) y **clicable para abrir el informe**; la columna de fecha se llamaba *«última ejecución»* y se acortó a **Fecha**. Anchos repartidos para dar aire al perímetro (24%) y a los CTA de hover (15%), con las seis columnas centrales al 55%, pestañas **Todo · En curso · Pendientes de revisión · Revisadas · Borradores** (con **contador** en cada una), filtro de tipo + buscador. **Acciones de fila por estado**:
  - *En curso*: **% junto al tag**; en hover, botón **Cancelar** (rojo); kebab **Ver detalle / Cancelar**.
  - *Pendiente de revisión*: **Revisar** (CTA).
  - *Revisada*: **Ver informe**; kebab **Ver informe / Re-auditar / Eliminar** *(el «Exportar» del kebab se retiró: era un stub con alert y duplicaba el botón «Exportar informes» del pie del informe, que es el punto de entrada real)*.
  - *Borrador*: fila con **Continuar** (verde) + **papelera** (eliminar), siempre visibles, **sin ficha por ninguna vía**: *Continuar* y su **ID** abren el **funnel con el perímetro ya seleccionado**, y `openInforme` redirige al funnel si recibe un borrador. *(Antes `Continuar` lo pasaba a «En curso» y abría la ficha saltándose el perímetro; y el ID clicable —añadido con la columna nueva— dejaba entrar a un informe completo con matriz y Health Score a 0.)* `vAudContinuar` queda separada de `vAudRelanzar`, que solo sirve ya para **Error → Reintentar**. Hay un borrador en los datos de ejemplo (`#1011 · Proveedor · Saint-Gobain`) para que la pestaña no salga vacía — no un proveedor **dual** (Roca, Grohe, Bosch), que pide además elegir 1P/3P y dejaría el valor sin resolver al retomarlo.
- **Nueva auditoría**: **modal de tipos** (Gama · Proveedor/Seller · Modelo · Categoría web · Listado) → **wizard de 2 columnas** ("Tu selección"): selector + detección de referencias (buscador/lista, URL con Consultar, subida de CSV con nombre editable), caja azul "¿Qué se validará?".
- **Ejecución**: modal con **barra de progreso global**, **cancelable** (con confirmación — dentro del funnel vuelve al wizard con el perímetro), **continuar en segundo plano** y **"Ver proceso en detalle"** (abre la ficha del informe *En curso*). Sin pausa.
- **Estados de la auditoría**: **En curso · Pendiente de revisión · Revisada · Borrador · Error**. **Borrador** = auditoría *En curso* cancelada desde el **hub o su ficha** (guarda el perímetro; el modal avisa de que queda como borrador). Cancelar **dentro del funnel** no crea borrador (te deja en el wizard).
- **Detalle de auditoría** (`v-informe`, una sola pantalla condicional por estado):
  - **En curso** → **vista de progreso** (contador de referencias analizadas · % · barra · botón **Cancelar**), sin métricas que dependan del fin.
  - **Pendiente de revisión / Revisada**:
    - Cabecera: **título = el ID de la auditoría** (`#1002`, como en Descripciones), badge, y debajo la **metadata**: perímetro en semibold · fecha · creador · versión del motor — sin los conectores *«Auditado»* ni *«por»*, y sin repetir *«Auditoría de <tipo>»*, que ya dice el perímetro. **acciones arriba a la derecha** en tamaño xs (32px, sin iconos): *Reabrir revisión* y *Re-auditar* como terciarios (fondo blanco, sin borde, hover a `--green-hover`) y la primaria según estado (*Marcar como revisado* / *Exportar informes*). **La bottom-bar desaparece de la vista fija** y se convierte en una **barra flotante que solo aparece al hacer scroll** (`position:fixed`, a partir de 140px; oculta arriba del todo), con los botones **a 44px, centrados y los secundarios con borde** — más presencia que los de la cabecera porque flota sobre el contenido. *(Nota técnica: el scroll lo lleva la ventana, no `.page-body`, porque `.screen` usa `min-height:100vh`.)* **Health Score** arriba-derecha — ⚠️ **ya no está en la cabecera**: se movió a la **izquierda de la franja de pestañas**, con la cifra antes de la etiqueta. Y se **retiró el tratamiento «provisional»** (gris sin flecha mientras *Pendiente de revisión*) **junto con el icono y el tooltip**. Era el reflejo en UI del pendiente §5.11 —el HS se recalculaba al depurar falsos positivos—; al quitarlo, el proto **ya no distingue provisional de depurado**, algo coherente con que el valor *venga dado* por el cliente, pero que habrá que revisar cuando §5.11 se cierre. **En las filas del hub** el HS provisional también salía gris y sin flecha. **cajitas** de estado en **4 niveles excluyentes** (Sin errores / Con errores leves / Con errores críticos / **No publicable**) + por tipo (discrepancias / ortografía), con tooltip.
    - **Una sola tarjeta blanca** agrupa las cifras y los dos desplegables (patrón del informe de generación masiva de Descripciones): arriba la fila de cifras —**total de referencias suelto a la izquierda** y las seis cajas de estado dentro, con su barra de color y el divisor antes del grupo discrepancias/ortografía—, y debajo, **dentro de la misma tarjeta**, las dos franjas **a todo el ancho** separadas solo por filete, sin bordes laterales ni radios propios. La fila de cifras va en **una sola línea** (`flex-wrap:nowrap`, cajas comprimibles), a costa de que algunas etiquetas se partan en dos líneas; cuando eso pasa **todas las cajas crecen a la vez** (`align-items:stretch`), las cifras siguen alineadas porque el contenido se ancla arriba, y **la barrita de color crece con la caja**. El total va a 23px —entre las cajitas (19px) y el título de página (28px)—.
    - **Impacto por categoría de error** y **Distribución de errores** → ya no son dos acordeones apilados sino **dos pestañas** en una franja dentro de la tarjeta, con el **Health Score a la izquierda** y las pestañas a la derecha. Clicar una inactiva cambia de panel; **clicar la activa lo cierra**, y el chevron se invierte para comunicarlo. Arrancan **las dos cerradas**. *Motivo: con los dos acordeones abiertos la tarjeta llegaba a ~1.050px antes de la matriz (Distribución ~620px, Impacto ~240px); con pestañas el máximo es ~700px y el mínimo vuelve a ~185px, porque nunca se apilan.* Las pestañas ocupan **todo el alto de la franja**, así que el subrayado verde queda pegado al borde inferior. La Distribución tiene 3 columnas (Designación · Descripción · Ficha técnica, con **Multimedia** anidada bajo Ficha técnica), barritas **coloreadas por severidad** y **ordenadas** por gravedad, con leyenda.
    - **Paleta única de severidad** en toda la vista: **Leve = dorado** · **Crítica = rojo oscuro** · **Bloqueante/No publicable = rojo** (WCAG AA).
    - **Matriz de correcciones**: filtros con placeholders cortos que **adaptan su ancho al texto** (Seller · Owner · Severidad · Tipo de error **agrupado por área** con todos los tipos · Segmento · búsqueda) **sin selector de orden**: la matriz va **siempre ordenada por severidad** (`_infSort='sev'`, fijo). Se retiró porque, con la misma forma y posición que los filtros, se leía como uno más, y filtrar y ordenar son operaciones distintas. Para acotar por gravedad ya está el filtro de Severidad; si en el futuro hace falta ordenar, el camino son las **cabeceras clicables de la matriz** —los cinco criterios coincidían con columnas existentes—, no devolver el select a la fila. El **buscador cierra la fila de filtros**. + **toggle "Falsos positivos (N)"** (aislado a la derecha, con contador). Paginada.
    - **Falsos positivos** (la **IA no los prescribe: los detecta el humano**). Modal **"Selecciona el motivo del falso positivo"**: **cajita resumen** (fondo gris, **colapsable**) con la **referencia enlazada a la PDP** + modelo, **tipo**, **severidad** y **disparador**; **motivos** (chips) + **comentario**. Los checks de IA llevan un **badge IA** (origen de la alerta, no un veredicto). En la matriz cada fila lee como **error a corregir**; la validez la decide el revisor.
      - **Flujo en dos pasos (cuando hay coincidencias):** si el mismo **disparador** ha saltado en otras referencias, el modal se convierte en un proceso de **dos pasos** y revisar las coincidencias **deja de ser opcional**. **Paso 1 · Motivo** (título negro, indicador *Paso 1 de 2* en gris a la derecha): motivos + comentario, y una **caja informativa azul** *"El mismo disparador ha saltado en N referencias más, en M modelos. Las revisarás en el siguiente paso"* — es un **aviso, no una decisión**. El CTA es **"Siguiente →"**: no se puede cerrar marcando una sola sin haber visto la lista. **Paso 2 · Revisar coincidencias** (*Paso 2 de 2*): la **referencia original** arriba marcada y deshabilitada (siempre entra); las coincidencias **agrupadas por modelo** (el de la original **primero** + separador *"Otros modelos"*), **desmarcadas por defecto** y con los bloques **desplegados de entrada** (el objetivo del paso es **verlas**), con **enlace a PDP** y **seller**, **filtro por seller** transversal y **"Seleccionar todas"** (alterna a *"Quitar todas"*) para que el caso "aplica a todo" no cueste N clics. Pie: **← Atrás** + contador *referencias · modelos* + botón dinámico **"Marcar N falsos positivos"** (*"Marcar 1 falso positivo"* si no se marca ninguna). **Confirmar sin marcar es una salida legítima y explícita.** *Por qué así: el diseño anterior (aviso ámbar ignorable + "Revisarlas →" opcional) resolvía el "no decidir" en silencio como "solo esta" — un **default oculto**. Se descartó la alternativa de radios de alcance en una sola pantalla porque pedía un compromiso **a ciegas**: sin ver las coincidencias no se puede saber si el descarte aplica a ellas (comparten disparador, no contexto).*
      - **Sin coincidencias → un solo paso.** Si el check no admite bloque (`bulk:false`) o no hay otras referencias con el mismo disparador, no hay indicador de paso, ni caja informativa, ni paso 2: el CTA es directamente **"Marcar falso positivo"**. *(Verificado sobre los datos del proto: 5 tipos van a 2 pasos — ortografía, color, material y los dos de SEO — y 2 tipos a 1 paso — dimensiones y designación administrativa.)*
      - Salen del informe al **cajón de falsos positivos** con **Motivo / Editar / Restaurar** (**restaurar** un bloque pregunta si deshacer las demás).
      - **Hipótesis de elegibilidad** (no confirmada, ver pendiente 16): checks de **juicio** (discrepancias, ortografía, SEO, administrativa) llevan botón de falso positivo; **ausencias objetivas** (sin descripción/designación, atributos vacíos, imágenes, longitud) no.
  - **Pie**: *Pendiente de revisión* → **"Marcar como revisado"** (modal de confirmación con cifras: **hallazgos válidos** —barrita roja— vs. **falsos positivos descartados** —barrita **gris**—; barritas a la altura del texto). *Revisada* → **Re-auditar** (icono) + **Exportar informes** (icono).
- **Generar informes** (modal, cierra con **X**): tres informes con botón **Descargar**. La modal es **solo acciones**, organizada en **una tarjeta por destinatario** (*Proveedor · TIP / ecommerce · Informe de falsos positivos*) con **una fila por entregable** —el formato en peso ligero— y un botón **idéntico** a la derecha (icono + *Descargar*): **primary** los del proveedor, **secundario** los internos, para que se lea de un vistazo qué sale hacia fuera. El **selector de proveedor** va arriba a la derecha de su tarjeta y es global para sus dos filas. Se retiraron los textos descriptivos de cada opción y el subtexto de cabecera, y **desaparece la vista previa del PDF** — el documento se valida imprimiéndolo, no con una miniatura dentro de un modal. *Lo que decía cada descripción, que sigue siendo la definición de cada informe:*
  - **Informe seller / proveedor** — PDF defensivo + CSV: solo lo que no cumple de **sus** referencias, sin guía completa ni parte interna. Lleva **selector de proveedor**, porque el informe es siempre de uno aunque el perímetro abarque varios.
  - **Informe interno TIP / ecommerce** — matriz completa de coherencia, con el detalle para corregir o enrutar (1P → TIP, 3P → Marketplace).
  - **Informe de falsos positivos** (Excel o CSV) — motivos y comentarios de las alertas descartadas, por tipo y regla. Insumo interno para el **Motor de validaciones** (admin).
  - *Los dos informes por audiencia son **independientes** (nunca se comparten); el de falsos positivos es interno.*
- **Detalle original de la modal**: tres informes con botón **Descargar** — **seller/proveedor** (PDF+CSV), **interno TIP** (matriz), e **informe de falsos positivos** (Excel/CSV, para el admin/motor).
- **Configuración del motor** (admin): **enlace** (verde, icono ajustes) junto a "Nueva auditoría" → **panel "Reglas del motor de validaciones"** (pantalla propia con navbar y ancho completo):
  - **Árbol de niveles** a la izquierda (con buscador): **Prompt general** ▸ **Familia** ▸ **Modelo** (el código del modelo a la **izquierda** del nombre, en gris). *Prototipado con "familia" como nivel intermedio a modo de hipótesis (pendiente de confirmar cuál es el nivel real sobre el modelo — ver §5 / preguntas).* La **guía solo aplica a sus modelos**.
  - **Editor de prompt por nivel** a la derecha: textarea del prompt de ese nivel, **"última modificación"** (fecha · autor), enlace a un **resumen de la guía** aplicable (modal de referencia; el motor **nunca reedita la guía**), y **"Guardar cambios"** (deshabilitado si no hay cambios en ese nivel). Modelo de reglas: **duras generales primero y lo específico gana** (general ▸ familia ▸ modelo).
  - **Versionado del motor** (**card en 3ª columna, sticky** a la derecha del editor — antes barra horizontal): badge **"Motor vX"** + **"Publicado …"** + **"Ver historial"** (icono reloj); si hay cambios sin publicar → chip ámbar **"Cambios sin publicar · N niveles"** + **"Ver detalles"** (modal-lista de los niveles editados, clicables), botón **"Publicar nueva versión"** y enlace ghost **"Cancelar cambios"** (descarta todos los pendientes, con confirmación, revirtiendo el contenido a la última versión publicada). En el árbol los niveles afectados salen con **pelotita naranja + negrita**. Publicar abre un **diálogo para nombrar la versión** (spinner "Publicando…" + confirmación); se registra con **título + autor**. **"Ver historial"** → **drawer lateral derecho** con las versiones (badge · título · fecha · autor · **Restaurar**) y un **dropdown** que lista con bullets **los niveles tocados** en cada versión.
  - Al **publicar** una versión, el diálogo muestra un **spinner "Publicando…"** en la misma capa y luego un **mensaje de confirmación** ("Motor vX publicado") antes de cerrarse (patrón proceso→confirmación).
  - *Fuera del MVP:* la **bandeja/cola de propuestas de falsos positivos** será un **archivo** (export), no una conexión directa que reedite el prompt (ver §5.14). **Reglas por proveedor**: pregunta futura.
  - **"Vocabulario y listas"** (nodo padre en el árbol, hermano de "Prompt general"): se **despliega** en un bloque por lista — **Excepciones ortográficas**, **Colores y equivalencias**, **Materiales**, **Abreviaturas y símbolos**. Pulsar el padre **abre el primer bloque** (no tiene contenido propio). Cada bloque es un **textarea tipo Markdown** (no chips) con **texto de ejemplo de cómo documentar** cada entrada — se prefirió texto plano sobre chips: admite matiz/ejemplos, escala, versiona limpio (diff) y es coherente con los .md que alimentan el motor. Cada bloque **versiona por separado** (pelotita naranja propia + rollup en el padre; en "cambios sin publicar"/historial aparece como "Vocabulario: <bloque>"). *Contenido = ejemplos; el real lo aporta el cliente/motor.*
  - *Pendiente:* los **prompts reales** por nivel (el proto usa texto de relleno) — *no prioritario ahora*.

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
10. Normalización del Health Score en MVP → ver 11. *(El cliente dice tenerlo **ya normado** en su lado; falta que nos pasen el modelo.)*
11. ⚠️ **Health Score — EN DUDA, pendiente de ver con cliente.** En principio **viene dado**, pero **no tenemos claras las implicaciones** de eso sobre lo que estamos construyendo: si el valor es externo y no lo recalculamos nosotros, queda por resolver qué relación guarda con los hallazgos de la auditoría, con los falsos positivos descartados y con el tratamiento "provisional" que hoy tiene el proto. **No tocar el proto hasta hablarlo con el cliente.** Preguntas abiertas: ¿se calcula en vivo al auditar (snapshot) o es pre-calculado? ¿nos dan fecha/hora de última actualización por referencia? ¿qué se muestra si el HS no refleja lo que la auditoría acaba de encontrar?

   **De dónde sale esto (cita literal del cliente):** *"Health Score: cómo se normaliza y si lo calculamos nosotros o viene dado. Nosotros ya lo tenemos normado. Aquí nombro a mi compañero @JAVIER HERNAN porque quizás podamos utilizar su trabajo."* Dos lecturas que conviene no perder:
   - **"Ya lo tenemos normado"** → la **normalización existe en su lado** (responde en buena parte al punto 10). Falta que nos den el modelo y la granularidad.
   - **"Quizás podamos utilizar su trabajo"** → es **tentativo**, no un compromiso de entrega. Redacciones anteriores de este doc decían *"nos pasan el valor"*, que afirma más de lo que se dijo.

   **Javier Hernán** = **empleado de Leroy Merlin**, compañero de quien respondió; se le nombra como **posible fuente de trabajo ya hecho** sobre el Health Score, no como el proveedor confirmado del dato. *(Rol y equipo exactos: sin confirmar.)*
12. Granularidad de exports (asunción): seller = por referencia, TIP = por alerta.
13. **Modelo de 4 cajas** — cajitas OK visualmente, pero **publicar = oferta + imagen** (regla de plataforma) y **despublicar es MANUAL** (emerchand/TIP). **Hay fichas publicadas que no deberían estarlo** → **se mantiene el nombre "No publicable"**, pero su lectura es: *publicada que no debería estarlo* (recomendación para despublicar manualmente, no un estado automático de plataforma). El reparto en tiers depende de la escala (ver 20).
14. Flujo falsos positivos → panel del administrador (con el export o con otro método): **pendiente**; nos pidieron el **enlace del prototipo** para verlo. *(No es reunión.)*
15. Re-auditoría: modelo del loop y transiciones (cancelar re-auditoría NO crea borrador; vuelve a Revisada).
16. ¿Qué tipos de check admiten falso positivo? **No decidido → meet.** (Hipótesis proto: solo los de juicio.)
17. Datos del falso positivo en bloque (disparador + Familia→Modelo→Referencia + seller + equivalencia) — **lo tratan en un meet**.
18. Nombre final del concepto: ¿"Auditorías" o "Validaciones"?
19. Subsección: descartada (solo "Sección").
20. ⚠️ **Escala de criticidad SIN DEFINIR** — el cliente pide *"establecer una escala de criticidad"*: qué **tipo de error → qué tier**, y cuáles llegan a **"candidata a despublicar"**. Es el **núcleo del módulo**; nuestra Bloqueante/Crítica/Leve es **propuesta**. Probablemente **sesión de trabajo**.

### Confirmado en el doc de dudas y respuestas (`Dudas y repsuestas doc validaciones.pdf`)
Cruce hecho (el doc es la fuente que ya consolidamos; su última página enlaza a nuestro análisis). **Confirma decisiones:**
- ✅ **Separar los exports por owner** — *"aquí se mezclan cosas del TIP y del seller, ¿no sería mejor separar?"* → **"correcto"** (pág. 31).
- ✅ **Reglas de owner** (pág. 6): el **seller/proveedor SIEMPRE es owner de falta de datos**; toda la **congruencia es del TIP** (contrasta). En **1P** el TIP modifica previa confirmación con el seller; en **3P** el TIP no juega → **marketplace** llama al seller (de ahí la utilidad de identificar 1P/3P).
- ✅ **PDF defensivo** (pág. 32): no pasarle la guía entera, solo el trozo que aplique; solo ve sus familias; proteger la estrategia de familia; darle solo lo que no cumple; *"el orden de las imágenes no hace falta"*.
- ✅ **Estándar de imágenes** (pág. 7): está en la **guía eMerch** (módulo con **tipo de imagen + nº mínimo**) → confirma que la regla es *categoría + mínimo*, **no** #FFFFFF/1500px (eso era del brief, no de la guía). A los sellers de marketplace **hoy no se les envía** ese estándar.
- ✅ **Atributos en conjunto** (pág. 7): no distinguen básicos/específicos a nivel de dato → auditar juntos; el report detecta los valores faltantes en los att seleccionados por seller/proveedor.
- ✅ **Bloque 4 (impacto en negocio)** (pág. 33): *"+18%/−14% NO son reales"* → verde, se afinaría con analítica (Función 4).
- ✅ **Re-auditoría manual** (pág. 37-38): lanzar una nueva auditoría manual sobre la previa; la automática, fases posteriores. Ámbito de la re-auditoría = el de la auditoría previa (seller / categoría web / modelo).
- ✅ **Panel admin del motor** (pág. 2): necesario para alimentar el prompt (casuísticas nuevas cada semana, vocabulario, *"admins por sección o tipología"*).
- ✅ **CSV del seller incluye 1P/3P** (pág. 33): *"el sku, el nombre del proveedor/seller, identificación de gama 1P y 3P, los att sin completitud de dato"* → devuelto al CSV del seller.
- ✅ **Health Score** (págs. 8-9): scoring /10 = Orphan 1,5 · Designación+Descripción+Atributos 3,5 · Reviews 3 · Imágenes 2. **Reviews** no lo auditamos → HS parcial en MVP.
- ✅ **Descartado / fase 2** ya reflejado: crawling y 3P, contenido premium (Contentful, ~90k refs, api/headless), análisis visual de imagen (color/medidas), categorización en navegación (la hace **ADEO**), evolutivo/rankings.

**Matices nuevos (para tener en cuenta, no urgentes):**
- **Agregación por modelo** — Fer la ve mejor que por PLP (pág. 5).
- **Multi-documento por proveedor** — todas las refs de su gama y luego **N documentos por categoría/sección** (pág. 5).
- **Roles/permisos** — harán falta roles para ver **solo 1P o solo 3P**; el **validador de gama** (equipo ecommerce, **no** info de producto ni emerchants) es el **usuario principal** de la herramienta (pág. 1).

### Preguntas redactadas al cliente
**Por escrito (confirmables):**
- **Obligatorios de la guía:** ¿solo los atributos de la **ficha técnica**, o también los que la guía exige en **designación/descripción**? ¿qué gravedad si faltan estos últimos?
- **Health Score (Javier):** **(a) Granularidad — la pregunta previa:** ¿nos entregáis el valor **por referencia**? Lo damos por hecho (vuestro modelo puntúa cada referencia sobre 10) y es lo que necesitamos: si lo tenemos por referencia podemos componer el HS de **cualquier alcance** (modelo, guía, categoría, seller, gama, perímetro) promediando. Si nos lo dais **ya agregado** a algún nivel, un HS "por seller" puede no existir y habría que construirlo o no mostrarlo. **(b)** ¿se **calcula al lanzar la auditoría** (foto de ese instante) o lo **actualizáis vosotros** periódicamente? **(c)** ¿nos dais **fecha/hora de última actualización** por referencia? **(d) Sobre qué se calcula el que ve el proveedor.** Vuestro deck (pág. 29) pone el Health Score en la cabecera del **informe por proveedor**, junto a *SKUs auditados*, *SKUs conformes* y *SKUs con alertas* — lo que indica que es la media sobre **todas sus referencias auditadas**, no solo sobre las que fallan. Lo confirmamos porque cambia el significado: si fuera solo el de las fichas con errores, el número sería siempre bajo y **bajaría cuanto mejor fuese su catálogo**. **(e) Y nos faltan dos cifras de esa cabecera.** Nuestro informe solo contiene los hallazgos, así que conocemos las referencias **con** errores pero no las **conformes** ni el total auditado del proveedor. ¿Nos los dais, o los derivamos de BigQuery?
- **"No publicable":** además de faltar **designación/descripción**, ¿hay otros mínimos (p. ej. **por debajo del mínimo de imágenes**) que hagan que una ficha no debería estar publicada?
- **Contacto del proveedor en el PDF:** el informe del seller cierra con *«contacta con tu responsable e-merch»*. Como en **3P el TIP no juega** y es **marketplace** quien llama al seller, ¿a quién debe dirigirle ese cierre — vale el e-merch para todos los casos, o cambia según 1P/3P? *(Descartada la pregunta anterior sobre quién dispara y envía el export: la herramienta solo genera y descarga los documentos; el envío y la subtarea de Jira ocurren fuera y no condicionan el diseño. Se mantiene la nota de que, aunque los exports sean distintos, **el equipo de Leroy accede a ambos**.)*
- **Jerarquía sobre el modelo:** ¿cuál es el nivel inmediatamente **superior al modelo** (arquetipo de guía) en vuestra estructura — la **familia** (merchandising), la **categoría web (PLP)** u otra cosa? Lo necesitamos para organizar el **panel del motor** (prompt general ▸ nivel intermedio ▸ modelo).

**Para la sesión de trabajo:**
- **Escala de criticidad:** qué **tipo de error → qué nivel**, y cuáles llegan a **"No publicable"**.
- **Falsos positivos — elegibilidad:** ¿qué **tipos de error** admiten falso positivo?
- **Falsos positivos — datos del bloque:** ¿el motor puede exponer el **disparador**, la jerarquía **Familia→Modelo→Referencia** y el **seller**? ¿cómo se calcula la **equivalencia**?

## Pendiente de prototipo (por maquetar)
- ~~**Estado *En curso***~~: **resuelto** — vista de progreso (contador · % · barra · Cancelar).
- ~~**Estado *Error***~~: **resuelto** — caja de aviso (conexión perdida, lo analizado no se pierde) + **Reintentar**.
- ~~**Revisado vs. Finalizada**~~: **resuelto** — fusionados en **Revisada**.
- ~~**Generar exports (modal)**~~: **resuelto** — modal "Generar informes" con 3 informes + acciones. *(El contenido real de cada informe está **a medias** — ver abajo.)*
- ~~**Cancelar auditoría En curso**~~: **resuelto** — desde hub y ficha (→ Borrador); pestaña Borradores.
- ~~**Falsos positivos en bloque**~~: **resuelto** — modal *"¿Falso positivo?"* (resumen colapsable + referencia enlazada) + sub-vista de coincidencias (Familia→Modelo→Referencia): original fija, modelos con separador *Otros modelos*, filtro por seller, restaurar en bloque. *(Elegibilidad de checks sin confirmar — §5.16; dependencia de datos — §5.17.)*
- ~~**Flujo del falso positivo — coincidencias obligatorias**~~: **resuelto (2026-09-21)** — el paso de revisar coincidencias pasa de **desvío opcional** a **paso 2 de un proceso**, con lista desplegada de entrada, *"Seleccionar todas"* y confirmación sin marcar como salida válida; **un solo paso** cuando no hay coincidencias. Ver el detalle en *Qué hay montado*. *(Sigue dependiendo de §5.17: si el motor no puede exponer disparador/jerarquía/equivalencia, el paso 2 no tiene datos que enseñar y el flujo vuelve a ser de un paso.)*
- ⚠️ **Health Score provisional** — **montado, pero su validez está en duda.** El proto lo trata en **gris** (sin flecha, también en filas del hub) mientras *Pendiente de revisión*, y en **color + bold** en *Revisada*, con **tooltip estilado**. Ese comportamiento asume que el HS se recalcula con la revisión; si el valor **viene dado de fuera**, la premisa puede no sostenerse. **Pendiente de la conversación con cliente (§5.11) antes de cambiar nada.**
- ⚠️ **Contenido real de los exports — PDF del seller rehecho (2026-09-21), pendiente de validar en navegador.**
  - **CSV** (seller / interno TIP / falsos positivos): **hechos y validados** (columnas según la plantilla del cliente; Owner:Seller lleva completitud+imágenes **con 1P/3P** —pág. 33—; Owner:TIP lleva coherencia+estructura). Los dos que no son de falsos positivos abren con el **bloque de identidad** `Referencia · Producto · Modelo · Nombre del modelo`: *Producto* es la **designación comercial de la referencia** —como en la plantilla del cliente— y el código y el nombre del modelo van en columnas separadas y contiguas.
  - **PDF del seller — rediseñado de cero contra la guía eMerch** (`docs de cliente/guia-gua-emerch-60.pdf`), que es la referencia del "debe ser". Su maqueta se leyó rasterizando el PDF, no solo su texto: **barra verde `#78be20`** vertical en el borde izquierdo de la portada, **banda `#494f60`** de borde a borde en las páginas interiores con un bloque verde al inicio, y **logo abajo a la derecha**:
    - **Formato: A4 apaisado**, tipo slide, con **tres hojas**: portada · qué hay que corregir · por qué merece la pena.
    - **Portada** propia, con el **perímetro como elemento dominante** (42px), proveedor, fecha e **ID de auditoría**, y tres cifras (referencias con acciones · puntos a corregir · Health Score, este último ya tomado del registro de auditoría, no hardcodeado).
    - **Organizado por las secciones de la guía** (Designación · Descripción · Atributos · Multimedia), a dos columnas, con **la regla de la guía una sola vez por sección** — antes se repetía idéntica en cada fila.
    - **Resumen agregado, no listado fila a fila.** Al cablearlo a los datos reales se vio que un proveedor puede tener **miles de puntos a corregir** (3.580 en el ejemplo), así que cada sección muestra **tipo de error · acción · nº de referencias**. El detalle uno a uno vive en el CSV, como dice §4 del análisis.
    - **Portada**: proveedor como titular (el informe es **siempre de un proveedor**, aunque la auditoría abarque varios), con el perímetro como línea de contexto; cuatro cifras —referencias con acciones, **bloqueantes**, **leves** y Health Score, contadas **por referencia y con la peor severidad mandando**—, el bloque *Por qué merece la pena* justo debajo y el logo al pie.
    - **Contenido en una columna**, con la acción en la misma línea que el tipo de error, ordenado **por severidad y luego por volumen**, sin ejemplos ni bolitas de color.
    - **Logo incrustado en línea (SVG)**, no enlazado, y `print-color-adjust: exact` para que **fondos y colores se impriman** sin depender de la casilla «Gráficos de fondo» del diálogo de impresión — era lo que hacía desaparecer las barras y dejaba el título en gris sobre blanco.
    - **Vínculo explícito con el CSV**: el documento lo **nombra** (`informe-seller-<slug>.csv`) y comparte el **ID de auditoría** con él (también en el nombre del CSV), para que sean un par identificable. Ese ID **ya no es inventado**: es el mismo `#1000+id` que se ve en el hub y en la cabecera del informe.
    - **Bloque de cifras de negocio** con jerarquía (número grande + frase). Son **placeholder**: se sustituirán por datos propios de Leroy validados por analítica (Función 4). *No llevan nota interna en el documento — debe parecer real.*
    - **Selector de proveedor** en el modal de informes: el informe es **por proveedor** (el perímetro tiene varios) y se elige cuál.
    - **Datos reales**: ya no usa el array dummy `_expRows`; se alimenta de `_infFindings` filtrando `owner: Seller` + proveedor.
  - **Generación: `window.print()` con `@page { size: A4 landscape }`** — **fuera html2pdf/html2canvas**. Se abandonó tras tres intentos fallidos (PDF en blanco por `z-index:-1` detrás del fondo de página; luego contenido desplazado y cortado). Print-to-PDF **no rasteriza**, así que esa familia de bugs desaparece, el **texto queda seleccionable** y no depende de un CDN externo. Contrapartida: se descarga desde el diálogo de impresión ("Guardar como PDF"), un clic más.
  - **Franja de footer reservada por página**: un `tfoot` que se repite en cada hoja impresa deja 24 mm libres abajo, de modo que el logo fijo **no puede pisar** la última fila de una tabla; si un bloque no cabe, salta a la página siguiente. Además, más aire entre bloques.
  - **Estado:** **impresión validada en navegador (2026-09-23)** — saltos de página, fondos y logo fijo correctos. Queda solo **validarlo con cliente**.
  - **Bloque 4 (impacto en negocio) fuera** (verde, depende de Función 4).
- ~~**Re-auditoría — lanzar nueva ejecución**~~: **resuelto** — **Re-auditar** (botón del informe + kebab del hub) **abre el funnel de nueva auditoría con el mismo perímetro ya seleccionado** (tipo + valor), en lugar de lanzarla a ciegas: así se puede revisar o ajustar el perímetro antes de ejecutar. Al lanzarla se crea una auditoría **NUEVA** con el **mismo perímetro y fecha nueva** (En curso); la Revisada original **queda intacta** (modelo *"fila nueva por ejecución"*, confirmado con cliente: re-auditoría **manual** sobre la previa). Como es una fila separada, **cancelar la re-auditoría no afecta a la original** (se resuelve solo la duda de §5.15).
- **Re-auditoría — sin evolutivo (decidido):** las re-auditorías son **ejecuciones independientes, NO vinculadas** entre sí (cada una es una auditoría fresca del mismo perímetro con su fecha). Por tanto **no hay comparación automática Corregidos / Persisten / Nuevos** en la herramienta → **fuera de alcance**. *(El evolutivo semanal que el cliente hace hoy queda fuera de este alcance / futuro o por otra vía.)* **No hay nada que prototipar aquí.**
- ~~**Reabrir una Revisada**~~: **resuelto** — textlink **"Reabrir revisión"** en el pie del informe Revisada → confirmación (avisa de que vuelve a *Pendiente de revisión* y desactualiza los exports) → reactiva las acciones de falso positivo y el botón "Marcar como revisado". Ciclo: Revisada → (Reabrir) → Pendiente → (Marcar como revisado) → Revisada.
- ~~**Panel de Configuración del motor (admin)**~~: **resuelto** — panel "Reglas del motor de validaciones" (árbol General ▸ Familia ▸ Modelo, editor de prompt por nivel, resumen de guía, última modificación con autor, **versionado** con publicar/nombrar/historial/restaurar y seguimiento de cambios sin publicar). *Pendiente: confirmar el nivel real sobre el modelo (¿familia?); contenido real de los prompts; cómo aterriza la cola de falsos positivos (archivo).*
- **Flujo falsos positivos → admin**: cómo se materializa (¿informe de falsos positivos descargable vs. conexión directa?) — ver §5.14. De ello depende el copy de los modales de falsos positivos y de "¿Marcar como revisado?".
- ⚠️ **Estados de error y vacíos del modal de informes** — hoy el modal **asume que todo va bien**: pulsas y descarga. Casos a maquetar:
  - **Proveedor sin hallazgos** — el select lo ofrece pero no tiene nada que corregir. → Fila del PDF deshabilitada con el motivo, no un documento vacío.
  - **Informe interno TIP vacío** — ningún hallazgo con `owner: TIP` en el alcance. → Mismo tratamiento.
  - **Sin falsos positivos marcados** — su informe no tiene sentido. → Fila deshabilitada: *«no has marcado ninguno»*. Es el estado **de entrada** de toda auditoría, así que es el vacío más frecuente.
  - **Auditoría sin proveedores en el alcance** — el selector se queda sin opciones. → Estado vacío de la tarjeta del proveedor.
  - **Fallo al generar** — la consulta falla o caduca (volúmenes de decenas de miles de filas). → Mensaje de error en la propia fila + **Reintentar**, sin cerrar el modal.
  - **Impresión cancelada** — el usuario cierra el diálogo del navegador sin guardar. → No es un error: no debe dejar rastro ni marcar el informe como generado.
  - **Descarga múltiple bloqueada por el navegador** — al bajar varios ficheros seguidos. → Aviso de cómo permitirla.
  - **Exports desactualizados** — al reabrir una revisión avisamos en el modal, pero la auditoría **no guarda ningún estado** que lo refleje después. → Decidir si se marca (chip *«informes desactualizados»*) o se acepta que el aviso se pierda.
  - **Estados no exportables** (En curso · Borrador · Error) — hoy el botón no existe, pero conviene dejarlo escrito para que no vuelva.

*(Nota: **no** hacemos "detalle de referencia" interno. Igual que el artefacto del cliente, la referencia **enlaza a la ficha real de Leroy (PDP en vivo)**, que es la fuente de verdad para revisar falsos positivos.)*

## Estado a 1 de octubre de 2026

> Foto consolidada para retomar. La anterior era del 30 de septiembre.

### Lo que ha pasado esta semana

- **Se definió el alcance del MVP** y se construyó el prototipo recortado. Todo en
  `MVP_VALIDACIONES.md` y `wireframe_validaciones_recortado.html`.
- **Se enviaron las preguntas al cliente** (`PREGUNTAS_CLIENTE.md`) y **contestaron el 30**.
  Han respondido la mayoría; quedan cuatro abiertas.
- **El jueves hay sesión con ellos**, con gente de Marketplace, y traen los criterios de
  clasificación de cada casuística.
- **El 1 de octubre se rehízo el panel del motor** sobre el modelo de reglas con ámbito y capas,
  cargado con el contenido real de los diccionarios del repo. Detalle más abajo.

### El MVP — seis recortes, ninguno cerrado con cliente

1. **Fuera el panel de configuración del motor.** Lo más caro del módulo y el modelo de reglas
   está a punto de rehacerse. Las reglas las configura desarrollo, fuera de la herramienta.
2. **Perímetros de 6 a 3**: Gama, Proveedor/Seller y Listado CSV. Fuera Modelo, Categoría web y
   Sección. El listado se recuperó con un argumento propio: subir un fichero puede ser más fácil
   de habilitar que resolver selectores contra el catálogo.
3. **Fuera los borradores.** El funnel es corto. Cancelar una auditoría en curso elimina la fila.
4. **Fuera Distribución de errores e Impacto por categoría.** Analítica de solo lectura; los
   filtros del listado y el CSV cubren la necesidad de priorizar.
5. **Falsos positivos simplificados**, no eliminados: se simplifica la mecánica, no el dato.
6. **El PDF pierde el eje de secciones de la guía**: *Qué hay que corregir* pasa a ser una sola
   lista por severidad. Cierra el solape con el recorte 4, porque las dos calculaban lo mismo.

**Fuera también** el atajo de Re-auditar. **Se mantienen** el PDF del proveedor, Reabrir revisión
—es la única corrección si alguien marca como revisada por error— y la escala de criticidad con
nuestra propuesta por defecto.

### Lo que confirmó el cliente el 30 de septiembre

- **La escala Bloqueante / Crítica / Leve les parece correcta.** Falta el mapeo error → nivel, y
  **lo traen el jueves**. El mayor bloqueante del módulo pasa a ser una sesión con fecha.
- **El Health Score llega por referencia**, calculado por ellos sobre sus reglas de calidad. El
  HS por proveedor es viable y se queda en el MVP.
- **Nuestra hipótesis sobre falsos positivos era correcta**: las ausencias objetivas no admiten
  descarte, van a la escala de criticidad.
- **Una misma categoría web puede contener referencias de modelos distintos.** Confirmado.
- **Quieren versionado de guía**, y confirman que hoy no existe.
- **El nivel superior al modelo es la sección** (jardín, cocinas…), con el matiz de que un modelo
  puede estar en varias. Se aclara el jueves.

### Lo que cambió en el producto a raíz de esas respuestas

- **El Health Score deja de ser nuestro.** Se calculaba como media ponderada de nuestras
  severidades; ahora se simula el valor que Leroy da por referencia y se promedia el conjunto,
  que es el mecanismo real. Los números salen realistas y correlacionan con el reparto de
  errores: Roca 55 · IlumStore 62 · Simon 64 · LuzHogar 69 · Grohe 71 · Bosch 83 · Saint-Gobain 84.
- **Y deja de tratarse como provisional.** El gris sin flecha mientras la auditoría estaba
  pendiente asumía que lo recalculaba nuestra revisión. Ahora se pinta con su color y su
  tendencia en todos los estados, **incluidas las auditorías con Error**: el score existe aunque
  nuestra ejecución no se complete, porque es dato suyo.
- **Los tres CSV ganan Sección y Categoría web**, que es lo que pidieron poder filtrar. Al
  hacerlo salió que no todos los hallazgos traían el mismo campo —unos `seccion`, otros
  `familia`—, y se unificó.
- **Confidencialidad, conclusión nuestra:** filtrar por proveedor solo tiene sentido en el
  informe interno. Si el CSV del proveedor tuviera columna de proveedor filtrable, es que
  contiene datos de otros. El suyo sigue sin columna Seller.

### Check nuevo, ya montado

- **Atributo de la designación sin informar.** La designación menciona un atributo —un casquillo
  E27, un acabado cromado— que **no está informado en la ficha**. Montado con **owner Seller** y
  severidad **leve**, y por tanto **sí sale en el informe del proveedor**. No admite falso
  positivo: que un atributo esté informado o no es objetivo, no es un juicio.

  *Sobre la lectura de su frase.* Dicen *«un att que esté en la designación y no esté marcado
  como obligatorio debería ser una alerta»*, y admite dos lecturas que en realidad son los dos
  extremos de la misma cadena: **la causa** es que la guía no lo exige, y **la consecuencia** es
  que el seller no lo ha informado. Lo que decide el owner es por qué extremo se actúa. Se montó
  por el del seller porque es accionable hoy; cambiar la guía es sistémico y lento. **A
  confirmar el jueves**, porque si lo que quieren es corregir la guía, esto cambia de owner y
  sale del informe del proveedor. *A confirmar el jueves que el enfoque les cuadra.*

### Taxonomía de niveles — nuestra posición, pendiente de confirmar

Jordi propuso (30 de septiembre) separar los errores en tres niveles y poner una **regla de corte
aparte de la nota**: si hay alguno del nivel alto, la ficha no se publica. Hoy su motor funciona
con la media: nota ≥ 90 → publicable, lo que deja pasar fichas con un fallo grave. Separar la
nota (calidad) de la puerta (publicabilidad) lo arregla sin tener que ponderar la media.

Converge con la escala que Leroy aprobó, así que lo que queda es **acordar qué implica cada
nivel**. Nuestro corte, sobre un eje de completitud:

| Nivel | Qué es | Qué implica |
|---|---|---|
| **Bloqueante** | **Falta algo.** Sin designación, sin descripción, sin atributos obligatorios, por debajo del mínimo de imágenes. La ficha está incompleta. | **No publicable** |
| **Crítica** | **Algo se contradice.** Título «gris» y descripción «antracita»; «120 cm» en el título y 100 en el atributo. La ficha está completa pero es incorrecta. | Publicada, corrección prioritaria |
| **Leve** | **Forma.** Ortografía, unidades, longitud, estilo. | Publicada |

**Por qué este corte y no otro:** Leroy definió «no publicable» exactamente así —faltar
designación o descripción, o que existan pero tengan 2 o 5 caracteres—. Todo ausencia, ninguna
contradicción. La puerta va sobre **la completitud**, no sobre el riesgo de compra.

*La duda honesta que se asume:* una discrepancia de medidas puede generar más devoluciones que
una descripción ausente. Se elige completitud porque es objetiva, automatizable y defendible
ante un proveedor sin entrar a valorar.

**El informe ya está montado así, no hay que cambiar nada.** Las cajas de cifras son *Sin
errores · Con errores leves · Con errores críticos · No publicable*: tres cubos disjuntos por
peor severidad más el de las conformes. No existe una caja de «bloqueantes» aparte — **la de No
publicable ya es ese nivel**, nombrada por su consecuencia en vez de por su gravedad, que además
se lee mejor: *«1.633 no publicables»* dice qué significa, *«1.633 bloqueantes»* solo dice cuánto
pesa.

Lo único que queda desalineado es el vocabulario interno: en la matriz y en el CSV esa severidad
se etiqueta **Bloqueante**, y en la tarjeta **No publicable**. Es el mismo nivel con dos nombres,
uno de gravedad y otro de consecuencia. Funciona, pero conviene decidir si se unifica.

### El motor, tras leer su repo (30 de septiembre)

Jordi compartió `jordimx/description-engine`. Es un monorepo: `ciceron` (core agnóstico) +
capabilities (`validador-fichas`, `lm-emerch-ia`, `demo-descriptions`) + `apps/demo` como host.
Las conclusiones de diseño quedaron escritas **en su repo**, en `DESIGN_INSIGHTS.md`, para que las
tenga delante quien decida allí. Aquí va lo que nos condiciona a nosotros.

**Lo que el motor hace hoy.** 15 validaciones deterministas sobre tres textos —designación,
descripción y designación administrativa—: estructura, coherencia entre designación y
descripción (color, material, dimensiones, cantidad), unidades, ortografía por lista cerrada de
~110 erratas, y coherencia con el campo administrativo. Sin LLM todavía. Nota por ficha: media de
las 15, umbral 90.

**Lo que no ve, y nos afecta de lleno.** `ProductRecord` transporta solo esos tres textos:
**ni atributos ni multimedia**. Eso deja fuera las comprobaciones que nacen de la guía de estilo.
De los 20 tipos de error del prototipo, **9 no se pueden calcular hoy** — y entre ellos **4 de
los 6 bloqueantes**: atributos obligatorios faltantes, atributos básicos faltantes, imágenes por
debajo del mínimo y falta categoría de imagen.

Importa porque **la cifra de *No publicable*** del informe que se envía al proveedor **se apoya
sobre todo en esos cuatro**. Es el argumento central del documento.

**La salida probable.** Esas comprobaciones **ya existen en el módulo de Guías**, que valida CSV
de proveedor contra la guía y devuelve `MISSING_MANDATORY`, `INVALID_VALUE`, `INVALID_MEDIA_TYPE`
y `MISSING_MEDIA`. La lógica está; lo que cambia es que allí se aplica a un fichero subido y aquí
haría falta contra el catálogo vivo. La decisión —reutilizar lo de Guías o hacer crecer el
contrato del motor— es de Jordi, y ya sabe que le falta.

**Y no lleva proveedor.** El schema tiene ref, gama, sección, subsección, tipo, subtipo, modelo,
idModelo y segmento 1P/3P, pero **no proveedor ni seller**. Todo nuestro módulo gira alrededor
del informe por proveedor. O el host lo une, o el contrato crece.

**Dos cosas que salen bien sin haberlas buscado:**

- **Los pesos ya existen.** `DeterministicCheck extends Criterion` y el total se calcula con
  `weightedTotal` usando `weight ?? 1`; `validador-fichas` simplemente no los asigna. Aplicar la
  escala que trae Leroy el jueves es **asignar quince números, no construir nada**. Lo único que
  falta es el corte duro, que el core no tiene.
- **`idModelo` frente a `modelo`** —identificador estable frente a nombre para mostrar— es la
  misma decisión que tomamos en las columnas del CSV, tomada por separado.

**Lo que esto le hace al recorte.** El MVP cortó el panel del motor por coste y porque el modelo
de reglas iba a rehacerse. El efecto secundario es que **se llevó por delante justo lo que peor
teníamos modelado**, así que hoy **el prototipo recortado está más cerca de lo construible que el
completo**. No fue previsión, pero es el mejor argumento para defender el MVP — mejor que las
horas.

### El panel del motor — rehecho el 1 de octubre

Al leer el repo del motor (2026-09-30) se vio que **el modelo que prototipamos no era el suyo**:
nuestro panel era un editor de prompt por nodo de un árbol General ▸ Familia ▸ Modelo, y lo que
el motor tiene son overrides de (criterio, reason, ámbito) más una colección de diccionarios. El
1 de octubre se rehizo entero.

**Una sola entidad: la regla.** Diccionario y regla eran la misma cosa vista a dos alturas, y lo
único que las distinguía era que unas no tenían ámbito. Ahora una regla es *una afirmación que el
motor consulta al validar*, y lleva:

- **Tipo** — Vocabulario, Equivalencia, Correspondencia, Corrección, Contexto.
- **Validación** a la que alimenta.
- **Ámbito** — general o sección.

Son 15 reglas con **1.580 entradas reales extraídas del repo**: los 531 colores, las 505
correspondencias a familia ADEO, las 111 erratas, las 98 siglas del ADM, los 117 marcadores de
composición. Ya no hay ninguna lista inventada ni ninguna muestra parcial.

**Dos niveles y capas.** Una sección hereda todo lo general y su capa dice solo lo que cambia:
lo que **añade** y lo que **anula**. Anular no borra — la entrada se queda tachada con quién,
cuándo y por qué, y se puede reactivar. La razón es la de siempre: un hueco no tiene autor.

De ahí sale una lectura nueva que el panel avisa arriba: **una entrada general anulada en dos o
más secciones no son dos secciones raras, es una entrada general mal puesta**. Es la cola de
limpieza del vocabulario general.

**El término es la unidad dentro de un grupo.** En las reglas de equivalencia, añadir «lona» a
los plásticos no es una entrada nueva: es un miembro más de uno de los grupos. Los grupos se ven
plegados, un término por línea al abrirlos, y las capas apuntan a un término dentro de su grupo.

**La cola de descartes es el otro trabajo del panel.** Tres pestañas: *Por resolver*, *Ignorados*
y *Reglas*. Los descartes se agrupan por (disparador, motivo) y cada caso apunta a su regla
destino y a la operación —añadir o anular—, así que la modal de resolver ya no pregunta por el
mecanismo: solo **dónde**. Lo que se escribe sale del motivo del descarte.

**Lo que no cambió y sigue abierto:** el nivel intermedio entre sección y modelo (pregunta B3,
pendiente de cliente). Mientras no esté, el panel trabaja con dos niveles y el ámbito de modelo
está fuera.

### Lo que el motor no puede hacer todavía

Verificado en `apply-overrides.ts` el 1 de octubre, y es lo que habría que hablar con Jordi:

- El modelo de ámbitos **no es jerárquico, es acumulativo**: `overrides.filter(scopeMatches)`
  aplica todos los que casan, sin precedencia por especificidad.
- Solo existen dos niveles, `'all'` y `'model'`. **El nivel sección no existe**, y el comentario
  del fichero dice que fue deliberado.
- Un override **solo puede restar**. No hay forma de devolver un hallazgo que otra capa quitó,
  así que *«en general sí, aquí no»* hoy **no se puede ni expresar**.

Y hay un matiz que conviene que vea: las capas sobre el **vocabulario** y las capas sobre el
**resultado** no actúan en el mismo momento. *«Carpintería no acepta gris como color válido»*
cambia la entrada del check y hay que componerla antes de ejecutarlo; *«esta alerta de gris no
cuenta»* se aplica después. Lo que existe hoy es lo segundo; lo que el panel necesita es lo
primero, y es un mecanismo nuevo.

### Qué admite descartarse como falso positivo — criterio cerrado

El criterio es el que ya le mandamos al cliente en la A2, llevado hasta el final: **se descarta
lo que implica un juicio del motor, no las ausencias objetivas**. Operativamente: *si la
validación no se apoya en ninguna lista, no hay conocimiento que añadir y el descarte no lleva a
ninguna parte*.

Con ese criterio salieron tres tipos de `FP_CFG` el 1 de octubre, en los dos prototipos:

- **Falta semántica SEO en designación / en descripción** — que la guía exija un término y la
  ficha no lo diga es una ausencia objetiva. Y una regla de sinónimos sería contraproducente: el
  requisito SEO es literal porque es lo que la gente escribe en el buscador. Si la alerta está
  mal, lo que está mal es la guía.
- **Posible designación administrativa** — es `esTodoMayusculas(designacion)`, una heurística
  sobre la forma del texto. Sin lista detrás.
- **Discrepancia de cantidad** — solo compara números.

Quedan **cinco tipos que admiten descarte, y los cinco tienen diccionario detrás**: Ortografía en
designación, Ortografía en descripción, Discrepancia de color, Discrepancia de material y
Discrepancia de dimensiones.

**Consecuencia pendiente:** si una alerta de SEO está mal porque la guía pide algo que no aplica,
hace falta una vía para decirlo —**«la guía está mal»**— que no vive en el panel del motor. Es la
pregunta C2 y no está diseñada. Antes se colaba disfrazada de falso positivo; ahora se ve el
hueco.

### Tres cosas encontradas en los diccionarios de color y material

Salieron al cargar el contenido real. Son para Jordi, no son decisiones nuestras:

- **Los dos diccionarios de color se contradicen y no se nota.** `coloresEquiv` resuelve primero
  por `ADEO_FAMILIA_COLOR` y solo cae a `COLOR_GRUPOS` si alguno de los dos colores no está en el
  mapeo. Como el mapeo tiene 505 entradas, los grupos casi nunca se consultan — y cuando lo
  harían, dirían otra cosa: `antracita`, `grafito` y `carbón` están en el grupo del **negro** y en
  el mapeo son **Gris-plata**.
- **Un término puede estar en dos grupos de equivalencia.** `salmón` está en el grupo de
  *naranja* y en el de *rosa*, y `colorGrupo()` devuelve el primero que casa, así que el segundo
  es letra muerta. Falta decidir si eso debe poder pasar.
- **Los grupos no tienen nombre canónico declarado.** Son listas de sinónimos y el primer término
  hace de canónico por convención. Funciona —*blanco, negro, gris…* / *metal, madera, plástico…*—
  pero si alguien reordena un grupo, cualquier referencia a él por su primer término queda
  huérfana. Para producción haría falta un id estable.

### El panel de administración — lo pensado el 30 de septiembre

Conversación del 2026-09-30 sobre qué debería tener ese panel. **Nada de esto es MVP.** Lo que se
construyó el 1 de octubre está arriba; aquí queda el razonamiento del que salió y lo que todavía
no está hecho.

**Tres trabajos distintos**, que antes se mezclaban en uno: **resolver** lo que llega, **ver** qué
está activo y qué hace, y **revertir**. Los tres están montados.

**Lo que entonces llamábamos «tres salidas de un descarte»** —convertir en regla, corregir el
diccionario, ignorar— **se quedó en dos**, porque diccionario y regla resultaron ser lo mismo con
ámbito distinto. La modal pregunta solo **dónde**, y la tercera salida sigue siendo *ignorar*.
Lo que sí se mantiene intacto es el fondo del argumento: *corregir el origen deja la validación
funcionando y taparla no*, que ahora se expresa como la diferencia entre **añadir** conocimiento
y **anular** una entrada.

**Principio propuesto, y sigue en pie:** en Validaciones una regla debería **nacer de un
descarte, nunca de la nada**. *(En Descripciones no aplica igual: allí las reglas moldean lo que
se genera, no silencian nada, y por eso el §5 de Jordi sí contempla crearlas desde cero.)*

**La señal de una regla corta es la reincidencia**, no el volumen: que una entrada silencie mucho
significa que el fenómeno es transversal. Montado en dos sitios — el aviso *Alcance insuficiente*
en la cola, y la banda de *entradas generales anuladas en dos o más secciones*.

Desactivar antes que borrar: es reversible y no pierde el rastro. Montado.

**Y una métrica que solo puede dar este panel:** alertas levantadas, descartadas y su porcentaje,
auditoría tras auditoría. Es la tasa real de falsos positivos del motor, que teníamos como
pregunta abierta para desarrollo. Si baja con el tiempo, el bucle funciona; si no baja, los
descartes no se están convirtiendo en nada útil.

**Lo único con consecuencia inmediata** era qué tiene que capturar el MVP para que ese panel se
pueda construir después, porque no puede inventar datos que no se guardaron. Faltaba una cosa:
**de qué auditoría vino cada descarte**. Añadida como columna `Auditoría` a los dos CSV internos
—el del TIP y el de falsos positivos—; en el del proveedor no, porque el ID ya va en el nombre
del fichero y en la portada del PDF. Sin esa columna se pierden la **corroboración** entre
auditorías distintas y la **reincidencia**.

### Pendiente de cliente

- **Cómo se agrupan los falsos positivos** — sin respuesta, y es de lo que depende todo el
  marcado en bloque. Es la que más falta hace.
- **Sobre qué universo se calcula el Health Score del proveedor** — contestan *«aspiramos a tener
  una foto global de la calidad de ese seller»*, que apunta a todas sus referencias, pero no lo
  dicen. Necesita una re-pregunta de sí o no.
- **Si estar por debajo del mínimo de imágenes hace una ficha no publicable** — contestaron con
  otro ejemplo.
- **La designación comercial** — sin respuesta.
- **Los umbrales de longitud de designación y descripción.** Hoy por debajo de 35 y 80 caracteres
  es *leve*. Ellos añaden que una designación de 2 o 5 caracteres hace la ficha **no publicable**,
  pero no dan el umbral. ¿Cuál es, y hay algún grado entre medias o es un salto directo de leve a
  no publicable?
- **Qué tipos de error admiten falso positivo y cuáles no** — nos hace falta el detalle, y
  depende de la escala que traen el jueves.
- **A quién dirige el PDF su línea de contacto** — el jueves, con Marketplace.
- **Los diccionarios de su artefacto**: ¿están validados? Algunas abreviaturas aceptadas parecen
  confusas para el usuario final. *Pregunta nueva, para el jueves.*
- **Los umbrales de longitud** (C1) y **los atributos que la guía exige en la descripción** (C2),
  ambas redactadas en `PREGUNTAS_CLIENTE.md`. La segunda tiene el mismo fondo que el check nuevo:
  la alerta puede estar señalando la guía y no el producto, y eso cambia quién corrige y si sale
  o no en el informe del proveedor.

### Decisiones nuestras, abiertas

- **Si el PDF sale del MVP.** Dijeron que *«sería suficiente un documento con columnas donde
  poder filtrar»*. No piden quitar el PDF, pero ese *«sería suficiente»* debilita el argumento de
  mantenerlo. **Pendiente de validar el jueves**, con una pregunta directa: ¿el proveedor
  necesita un documento que le ordene el trabajo, o se apaña con un fichero que puede filtrar?
- **Si pedimos la fecha de actualización por referencia.** Preguntamos si nos dan fecha y hora
  del último cálculo de cada Health Score; contestan que hoy no la envían pero que pueden
  añadirla en su próxima recarga completa. Serviría para poder decir *«Health Score a fecha de
  X»* y defender el número si un proveedor lo discute. **Valor bajo**: su score se recalcula a
  diario, así que la fecha de la auditoría —que sí tenemos— nunca se aleja más de un día. Yo lo
  mencionaría de pasada el jueves, sin convertirlo en una petición formal.

### Dependencias fuera de Validaciones

- **Versionado de guía.** Lo quieren, y hoy no existe en el módulo de Guías. Sin versionado allí,
  aquí no hay nada que guardar. Va al backlog de Guías, no al nuestro.
- **Mapeo categoría web ↔ referencias.** Sin confirmar. La columna del CSV se simula.
- **Tasa real de falsos positivos del motor.** Pregunta para desarrollo, no para cliente: ¿qué
  porcentaje de alertas resultaron falsos positivos en una ejecución real, y cuántos venían de
  checks deterministas frente a los de IA? De ese número depende si el flujo de falsos positivos
  es una función central o una casilla marginal.
- **Las reglas de la guía en el motor** — atributos obligatorios y mínimos de multimedia. De esto
  dependen 9 de nuestros 20 tipos de error y 4 de los 6 bloqueantes.
- **El proveedor en el contrato del motor.** Sin él, el perímetro por proveedor —el eje del
  módulo— no se puede resolver.
- **Los pesos y el corte duro.** Cuando estén, nuestras severidades y su nota serán el mismo dato.

### Sin diseñar: flujos y casuísticas

- ~~**Histórico y evolutivo**~~ — **aparcado**: las ejecuciones van por separado.
- **Roles y permisos 1P/3P** — el cliente los pidió y el validador de gama es el usuario
  principal. Nada prototipado.
- **«La guía está mal»** — la vía para escalar una alerta cuyo problema es el documento y no la
  ficha. No es un falso positivo y no vive en el panel del motor. Enlaza con la C2.
- **Volver de la resolución a la regla** — al resolver un descarte, el toast dice a qué regla ha
  ido pero no lleva. Hay que ir al tab y poner el ámbito a mano.
- **Ver desde General qué secciones tienen capa** — hoy solo se ve si ya estás en el ámbito. El
  aviso de arriba solo cubre las anulaciones repetidas, no las adiciones.


## Infra / repo
- **GitHub Pages** vía **GitHub Actions** (`concurrency: cancel-in-progress: false` + `workflow_dispatch`). Deploy **encolado por incidencia de GitHub** (ago 2026); se publica solo al resolverse. Código a salvo en `main`.
- Prototipo servido en raíz: `wireframe_validaciones.html`. **Material fuente del cliente** en `docs/validaciones/docs de cliente/` — **gitignored** (no se versiona ni se publica en Pages).
