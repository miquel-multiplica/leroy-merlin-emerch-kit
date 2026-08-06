# Validaciones (Auditor de Calidad de Producto) — Análisis consolidado

> Documento de trabajo. Recoge el planteamiento del nuevo módulo **Validaciones** del eMerch Toolkit, cruzando la visión del cliente (deck "Auditor de Calidad y Gobernanza de Producto") con la estructura que ya tenemos en Guías y Descripciones. Se actualiza tras cada revisión con el cliente.

## 1. La foto actual: qué es

Un **nuevo módulo del toolkit** (cuarta sección, junto a Guías, Descripciones y Usuarios) que **audita la calidad de las fichas de producto** de forma recurrente y produce, como entregable, **informes accionables** separados por audiencia (equipo interno y seller/proveedor).

- **Usuario principal:** el **validador de gama** (equipo ecommerce, el "árbitro").
- **Qué audita en el MVP:** coherencia entre **designación + descripción** (+ **ficha técnica/atributos** si el dato está) y **cumplimiento de la guía eMerch**.
- **Sobre qué:** referencias **1P**, con acceso directo a datos (BigQuery). El perímetro se acota con filtros (modelo / proveedor / seller / gama / PLP) que resuelven a una lista de referencias.
- **Entregable:** **dos exports separados por audiencia** —
  - **Export para el seller/proveedor:** solo lo suyo (falta de datos + imágenes de sus referencias). Es lo único que sale hacia fuera.
  - **Export interno para el TIP / equipo ecommerce:** las incoherencias/congruencia y la vista de trabajo completa.
  
  Nunca un documento único compartido. *(El formato — un doc multi-guía o varios, cómo ordena las referencias — se define más adelante.)*
- **Es un ciclo, no un one-shot:** cada perímetro se **re-audita** y se **comparan periodos** (evolutivo: corregidas / persisten / nuevas).

Norte a largo plazo: el "Auditor de Calidad y Gobernanza de Producto" del cliente (4 funciones). El MVP es la base: Función 1 (coherencia, parcial) + Función 2 (cumplimiento) alimentando Función 3 (informe por seller).

---

## 2. Cómo funciona por dentro

**Motor híbrido:**
- **Reglas deterministas** para lo estructurado: medidas, unidades, formato (coma/punto, '40cm', orden de medidas).
- **Prompt / LLM** para lo semántico-contextual: material vs. marca ("teca"), color, coherencia y la "**palabrería Leroy**" (por eso el motor no es 100% determinista puro y necesita revisión humana).

**Dos tipos de comprobación, con lógica distinta:**
- **Coherencia (consistencia):** detecta un **desacuerdo** entre componentes (designación ↔ descripción ↔ ficha técnica ↔ —fase 2— premium/imagen). La herramienta dice "no coinciden", **pero no sabe cuál es el correcto** → lo resuelve un humano.
- **Cumplimiento:** contrasta contra la **guía**, que **sí es la fuente de verdad** → aquí **sí** puede decir qué está mal. *(No es un bloque aparte: es un tipo de error más dentro de la matriz.)*

**Atributos (aclaración):** no hay distinción **básicos vs. específicos** a nivel de dato → se auditan **en conjunto** (los atributos seleccionados por el e-merchandiser). El report detecta, por seller/proveedor, los **valores faltantes** en esos atributos.

**Estándar de imágenes:** vive en la **guía eMerch** (un módulo con el tipo de imagen y el nº mínimo) → el cumplimiento lo lee de ahí (imágenes = **fase 2**). Hoy **no se envía a los sellers de Marketplace** → el export sería la vía para comunicárselo.

**Separación por audiencia (regla dura):** lo que se exporta **para el seller es solo para el seller** (su falta de datos + imágenes, sin guía completa ni estrategia de familia). Lo del **TIP / equipo ecommerce es otro documento distinto** (las incoherencias internas). No se mezclan y **el seller nunca ve la parte interna**. La matriz se parte en esas dos vistas precisamente para generar **dos exports independientes**.

**Principio de agregación (perímetros multi-modelo / multi-guía):**
- **Coherencia** (guide-agnostic) → los errores **se suman literalmente** entre modelos/guías.
- **Cumplimiento** (guide-specific) → no se suma en crudo, pero **sí normalizado**: cada referencia se evalúa contra **su** guía y sale como *cumple / no cumple* (%); esos porcentajes **sí agregan**.
- **Health Score** → escala común, se calcula por referencia contra su guía y **se promedia/enrolla** a modelo → guía → categoría → seller → gama → perímetro.
- **El "qué corregir según la guía"** (info de cada fila de cumplimiento + extracto del export del seller) → se **segmenta por guía**.
- Regla mental: **coherencia y Health Score agregan siempre · cumplimiento agrega como % · el detalle-vs-guía se segmenta por guía.**

**Health Score — es su modelo existente (pág. 39); lo reutilizamos por fases (no un score paralelo).** Su modelo puntúa cada referencia sobre **10 puntos**:
- **Designación + descripción + atributos** (designación 35–250 car. sin atributos de designación faltantes · descripción ≥30 car. · atributos rellenos) → **3,5 pts**
- **Orphan status** (que el producto esté ubicado/implementado en una familia) → **1,5 pts**
- **Reviews** (≥4) → **3 pts**
- **Imágenes** (entre 2 y 10) → **2 pts**

Además tienen una lectura **ponderada por Glance Views** (tráfico a la ficha) — eso es dimensión analítica.

Qué podemos calcular por fase:
- **MVP · Health Score base:** la parte que **auditamos** — **Designación + descripción + atributos (3,5)** (+ **Orphan 1,5** si el dato de ubicación en familia está disponible). Misma escala que la suya, marcado como *base*.
- **Fase 2:** se añaden **imágenes (2)**.
- **Reviews (3):** requiere fuente de reviews (cuando esté disponible).
- **Fase 3:** la lectura **ponderada por Glance Views** y su correlación con KPIs (Data-to-Revenue).

**Modelo de ownership (quién corrige):**
- **Falta de datos / completitud → siempre el seller/proveedor.**
- **Congruencia / coherencia → el TIP** (tiene que contrastar).
- **1P:** el TIP puede modificar, previa confirmación con el seller.
- **3P:** el TIP no actúa → el **equipo de Marketplace** contacta al seller.
- → La herramienta debe **marcar cada referencia como 1P o 3P** para enrutar el owner.

**Arquitectura en 3 capas (evita el lío de vocabularios):**
- **Alerta** (un problema concreto en una referencia): *tipo* → *categoría* · *severidad* (Bloqueante/Crítica/Mejora) · owner · 1P/3P.
- **Referencia** (0..N alertas): su **Estado** por **gravedad** → **Sin errores** (0 alertas) · **Con errores leves** (1–2, ninguna crítica) · **Con errores críticos** (3 o más alertas **o** falta designación/descripción — **basta 1 alerta esencial**). Nota: NO es solo por cantidad; el nivel crítico se dispara también por una única alerta esencial. **Ojo: Estado "Con errores críticos" (por referencia) ≠ Severidad "Crítica" (por alerta, del deck)** → colisión de palabra en capas distintas, etiquetar con cuidado.
- **Auditoría** (N referencias): Health Score · reparto de referencias por Estado · distribución por tipo · impacto por categoría.

Las **cajitas** del informe tienen **dos lentes**: **Estado/gravedad** (Sin errores / Con errores leves / Con errores críticos — **excluyentes, suman el total**) y **por tipo** (con discrepancias / con faltas de ortografía — **solapan**, no suman).

**Mapa de tipos de error · severidad · prioridad (propuesta — con puntos pendientes, ver §5).** Tres ejes distintos que **no se derivan uno de otro**:
- **Tipo de error** (qué falla) → se agrupa en **categorías**.
- **Severidad** (por referencia): **Bloqueante / Crítica / Mejora** (Severity Score, PDF pág. 34).
- **Prioridad de resolución** (por categoría, motivo de negocio): **Alta / Media / Baja** (PDF Bloque 1). **Fija por categoría** — el volumen (*SKUs afectados / % catálogo*) se muestra aparte y **no altera** la prioridad.

| Categoría | Tipos que agrupa | Familia | Owner | Severidad | Prioridad · motivo |
|---|---|---|---|---|---|
| Contenido faltante | sin designación, sin descripción | Cumplimiento | Seller | Bloqueante | **Alta** · impide publicar |
| Atributos obligatorios faltantes | atributos básicos/específicos de la guía vacíos | Cumplimiento | Seller | **Bloqueante** ⚠️ | **Media** · afecta filtros de búsqueda |
| Discrepancia interna PDP | medidas/dimensiones, color, material, nº de elementos (desig ↔ desc ↔ ficha técnica) | Coherencia | TIP | Crítica | **Alta** · confunde al usuario / devoluciones |
| Estructura de designación | designación corta, estructura de título, mayúsculas admin, unidades, coma decimal, dims reordenadas | Cumplimiento | Seller* | Mejora | **Baja** · SEO / legibilidad |
| Ortografía | faltas en designación o descripción | Calidad | TIP | Mejora | **Baja** · conversión |
| Imágenes *(fase 2)* | mínimo no alcanzado, obligatorias | Cumplimiento | Seller | — | — |

**Regla de severidad** (por referencia): *Bloqueante* = falta contenido esencial o atributos obligatorios (no publicable) · *Crítica* = contradicción entre componentes (info engañosa) · *Mejora* = formato/estructura/ortografía.

**Jerarquía tipo/categoría:** una **categoría** (nivel alto, 5) agrupa varios **tipos** (checks granulares). Dónde se ve cada eje: la **matriz** muestra *tipo* + *severidad* por fila · **"Impacto por categoría de error"** muestra *prioridad + motivo* por **categoría** (todas) · **"Distribución de errores"** muestra los *tipos granulares* por campo (mismo nivel que la columna "Tipo" de la matriz). Orden en el informe: métricas → distribución (colapsable) → impacto → matriz.

**Tipos granulares → categoría · severidad · origen/motor** (checks reales del artefacto + nuestras adiciones). El **origen** dice de dónde sale la alerta: **Guía de estilo** (cumplimiento) o **Coherencia entre piezas de la ficha** — y si es coherencia, si se resuelve por **comparación de datos** (determinista) o por **LLM** (semántico). Enlaza con el "motor híbrido" de §2.

| Tipo de error (check) | Categoría | Severidad | Origen / motor |
|---|---|---|---|
| Sin designación · Sin descripción | Contenido faltante | Bloqueante | Contenido mínimo (determinista) |
| Discrepancia de medidas · Dimensión extra no recogida · Diferente nº de dimensiones · Medida no encontrada · Unidades distintas · Coma decimal | Discrepancia interna PDP | Crítica | **Coherencia · comparación de datos** (determinista) |
| Color · Material incoherentes | Discrepancia interna PDP | Crítica | **Coherencia · LLM** (semántico: "gris"≟"antracita", "teca") |
| **Info en ADM no reflejada** | Discrepancia interna PDP | Crítica | Coherencia · comparación de datos (+ LLM si es semántico) |
| Dims reordenadas | Estructura de designación | Mejora | Coherencia · comparación de datos (determinista) |
| Designación corta (<35) · Descripción < 80 car. · Estructura de título (orden/color) · **Posible designación administrativa** | Estructura de designación | Mejora | **Guía de estilo** (cumplimiento · determinista) |
| Atributos obligatorios · Atributos básicos faltantes | Atributos obligatorios faltantes | Bloqueante ⚠️ | **Guía de estilo** (cumplimiento · determinista) |
| Ortografía (designación / descripción) | Ortografía | Mejora | Calidad · diccionario / LLM |

**Designación administrativa (ADM) — glosario.** Hay **dos** designaciones: la **administrativa/ADM** (nombre **interno** de back-office, normalmente en MAYÚSCULAS, abreviado) y la **comercial** ("designación cliente larga", la que ve el cliente). De ahí dos checks:
- **"Posible designación administrativa"**: la designación pública **parece la interna** (la detecta por venir *todo en mayúsculas*) → han puesto el nombre interno en el campo público. → categoría *Estructura de designación* · Mejora.
- **"Info en ADM no reflejada"**: el ADM tiene **datos que no aparecen** en la ficha comercial/descripción. → categoría *Discrepancia interna PDP* · Crítica.

*Ambos dependen del **campo ADM en origen** → confirmar que BigQuery lo expone (ver §5.8).*

\* *Owner "Estructura de designación": el dato crudo puede aportarlo el seller pero no aparecer en la designación (construcción del título) → owner a confirmar (ver §5).*

**Datos:** acceso directo (BigQuery) es **casi requisito**; el crawling queda casi descartado por eficiencia. **1P: hay acceso directo. 3P: puede que no** → 3P (y crawling) para más adelante. Los filtros de perímetro (incluida la PLP) solo acotan **qué** referencias entran; el **dato siempre se lee de la fuente directa**, no de la página.

**Herramienta actual del cliente (baseline).** Hoy usan un *"Validador de Fichas de Producto"* propio: un motor de **reglas deterministas en el navegador** que audita **designación y descripción** (sin BigQuery ni LLM). Tiene tres módulos: **Validador** (subir el **CSV de una gama** → analizar → tabla de resultados + detalle por referencia → export CSV, normal y "un error por fila"), **Evolutivo semanal** (comparar → **✅ Corregidos · ➖ Persisten · 🆕 Nuevos**) e **Histórico**. Sus checks van tipificados con emoji (📐 medidas/dimensiones, 🎨 color, 🪵 material, 🔤 ortografía, ⚠️ formato, ❌ falta, 🔄 orden) y un **Estado** por SKU (OK / Advertencias / Errores críticos → en nuestro módulo lo renombramos a *Sin errores / Con errores leves / Con errores críticos*). Nuestro módulo es la **evolución**: motor híbrido, coherencia con **ficha/atributos** (no solo texto), cumplimiento vs guía, Health Score, revisión de FP y exports por audiencia. *(Artefacto y CSV de ejemplo en `docs/validaciones/docs de cliente/`.)*

---

## 3. Usuarios y roles

| Rol | Qué hace en la herramienta |
|---|---|
| **Validador de gama** (ecommerce) | Usuario principal. Lanza auditorías, revisa falsos positivos, genera y envía los informes. |
| **TIP** (Información Producto) | Owner de la **congruencia**. Corrige incoherencias internas; en 1P modifica (confirmando con seller). Consume el informe. |
| **Equipo Marketplace** | Enruta lo de **3P**: contacta al seller (el TIP no juega en 3P). |
| **Seller / Proveedor** | Owner de la **falta de datos**. **Recibe el export** (no entra a la herramienta en el MVP). |
| **Admin de sección/tipología** | Mantiene el motor: prompt, documentos .md y vocabulario de su ámbito. |

**Permisos:** hay que segmentar la visibilidad **1P vs 3P** (rol/permiso nuevo, lo pidieron explícito).

---

## 4. Secciones, páginas y flujos

Módulo **Validaciones**, espejo estructural de Descripciones.

### Mapa de pantallas
1. Hub de auditorías
2. Nueva auditoría
3. Informe de auditoría (vista interna)
4. Generar exports (por audiencia)
5. Configuración del motor (admin)

### Estados de una ejecución
**En curso → Pendiente de revisión → Revisado → Finalizada** (+ Error). *(No hay concepto de "borrador" — eso en Descripciones significa otra cosa.)* El estado terminal es **Finalizada** (no "Enviada"/"Con seguimiento"): la herramienta **produce los artefactos pero no controla el flujo posterior** (corrección del seller / gobernanza va por fuera, en Jira), así que no promete un seguimiento que no gestiona. El **perímetro** persiste entre ejecuciones para el siguiente ciclo.

---

### 1 · Hub de auditorías
La entidad que persiste es el **perímetro/auditoría recurrente**; cada pasada es una **ejecución (snapshot)** con su Health Score y sus alertas.
- **Columnas:** perímetro auditado (con **chip de tipo** dentro de la celda: Modelo / Gama / Proveedor-Seller / Categoría web (PLP) / Listado) · nº de referencias · **Health Score (base)** · **tendencia** (↑/↓ vs. ejecución anterior) · nº de alertas · última ejecución · creador · estado.
- **Pestañas:** **Todo · En curso · Pendientes de revisión · Finalizadas** (revisadas + finalizadas). *(Los nombres de pestaña coinciden con los estados.)*
- **Filtros:** sección/tipología, 1P/3P (este último aplica cuando entre 3P).
- **Acciones por fila:** Ver informe · Exportar · **Re-auditar** · eliminar.
- **Entradas:** botón "Nueva auditoría" **y** desde Guías ("Empezar validación" de esa guía/categoría).

> **Punto de diseño a afinar:** cómo se articula el hub como **loop recurrente** (perímetros con histórico de ejecuciones + comparación de periodos / evolutivo). Es de lo más valioso del módulo y conecta con su evolutivo semanal; lo dejamos abierto.

### 2 · Nueva auditoría
Muy ligera (no es un funnel largo). En el prototipo: **modal de tipos (botones anchos, sin radios: clicas y avanzas) → wizard de 2 columnas**. Izquierda: **tipo de perímetro** (card con icono + lápiz para cambiarlo) y su **selector**. Derecha: **"Tu selección"** — cabecera con el perímetro + **SKUs detectados** + caja azul **"¿Qué se validará?"**.

- **Perímetro = un solo tipo por auditoría** (no se combinan varios filtros). Tipos:
  - **Modelo · Gama · Proveedor/Seller** (**un único selector unificado**, con **tag 1P/3P**) → buscador + lista; al elegir, la selección se confirma en la card derecha (para cambiarla, **lápiz en su esquina superior derecha** → resetea y vuelve al listado).
    - *Gama = clasificación **transversal** del surtido por **letra** (A/M/S/C/K/B); **no** es una familia de producto — corta todas las secciones. En el prototipo el selector son las letras (con subtexto "descripción pendiente"); **el significado de cada letra está por confirmar** (ver §5).*
  - **Categoría web** → se pega la **URL de la PLP** y se pulsa **Consultar** (validación de formato tolerante: admite sin `http`, con o sin `www`).
  - **Listado** → subir un listado de referencias con **nombre propio editable** (se edita inline en la card derecha). **Sí está en el PDF del cliente** (pág. 37, "Cómo alimentamos la herramienta" → *B · Listado de URLs de PDPs*, 1P y 3P), pero **con matiz de formato**: el cliente lo describe como **lista de URLs de PDPs**, y el prototipo lo montó como **CSV de SKUs** (por analogía con Descripciones). **Formato a alinear.**
- Todos resuelven a una lista de referencias cuyos **datos se leen de BigQuery (1P)**. Si el perímetro va ligado a una guía/categoría, se activan los checks de **cumplimiento**.
- **Empezar auditoría de calidad** → progreso por lotes → *Pendiente de revisión*. **El lote es transparente para el usuario**: solo ve **una barra de progreso global** cuya métrica es **SKUs procesados / total** (+ %) y que **no reinicia por lote**. El lote se reserva para resiliencia (reanudar tras fallo) y throughput del LLM, **no** como unidad de UI (no se muestra "Lote X/Y"). La ventana es **cancelable** (la confirmación **descarta el progreso y vuelve al wizard con el perímetro intacto** — no hay que reconfigurar) y permite **continuar en segundo plano** (la auditoría aparece como *En curso* en el hub). **Sin pausa.**

*(No hay paso de "reglas": todas las familias de comprobación van activas por defecto y se configuran desde Admin.)*

**Por qué por lotes:** un "lote" es un trozo de la lista de referencias (p. ej. 500). No se procesan las decenas de miles de golpe, sino por trozos, por cuatro motivos:
1. **Rendimiento** — evitar bloqueo/timeout de la página.
2. **El cuello de botella es el LLM** — lo estructurado (reglas) es instantáneo; lo semántico (material/color/coherencia) pasa por prompt → coste, latencia y rate limits; el lote regula el throughput.
3. **Progreso + segundo plano** — se ve "3.500 / 12.000" y se puede dejar corriendo.
4. **Resiliencia** — si falla en el lote 8, los 7 anteriores están hechos → se reanuda.
Para el usuario es transparente: solo ve una barra de progreso.

### 3 · Informe de auditoría (vista interna, el corazón)
Lo que ve el **validador de gama**. **Soporta perímetros multi-modelo y multi-guía**: por defecto todo **agregado**, con filtros para trocear.

- **Cabecera · Salud:**
  - **Contexto:** qué se auditó (perímetro), cuándo, quién, y **con qué versión del motor** (trazabilidad).
  - **Cifras:** referencias auditadas · **Health Score (base)** · conformes / no conformes · nº de alertas (total y por severidad) · reparto **coherencia (TIP) vs. completitud (seller)**.
  - **Desglose 1P/3P:** solo si el perímetro **mezcla** ambos (en el MVP, todo 1P → no aparece).
  - **Tendencia vs. periodo anterior** (cuando hay ejecución previa).
  - **Acciones rápidas:** revisar · re-auditar · exportar.
- **Impacto por tipo de error:** agrupación/priorización por tipo, con **severidad** (reglas simples) y **owner** (TIP/seller). Incluye los tipos de **cumplimiento**. Coherencias se **suman**; cumplimientos se muestran como **% conforme/no conforme**.
- **Matriz de trabajo — dos vistas: TIP / Seller:**
  - **Columnas por fila:** referencia · modelo · guía · tipo de error · **ubicación** (designación / descripción / ficha) · diagnóstico · **acción correctiva** · owner · 1P/3P · severidad.
  - **Agrupada por modelo** (grupos colapsables).
  - **Filtros:** guía / modelo / seller / tipo / severidad / 1P-3P.
  - **Acciones de fila:** marcar **falso positivo**, abrir la ficha (link a PDP), (fase posterior) marcar como corregido.
  - **Descargable** CSV/XLSX.

*(Bloque "Cumplimiento vs guía" eliminado → es un tipo de error dentro de la matriz. Bloque "Impacto en negocio" eliminado del MVP → Data-to-Revenue es fase 3.)*

### 4 · Generar exports (por audiencia)
Dos entregables **independientes**, nunca uno compartido:
- **Export del seller/proveedor** (hacia fuera): **PDF minimalista y defensivo** — solo **lo que no cumple + qué corregir**, de **sus** referencias/familias; **sin** guía completa, **sin** estrategia de familia, **sin** orden de imágenes — **+ CSV/XLSX** de su matriz. El **CSV** incluye: **SKU · nombre del proveedor/seller · identificación 1P/3P · atributos sin completitud de dato**. *(No hay "PIM" — era un error de su slide.)*
- **Export interno TIP / equipo ecommerce**: la vista de trabajo completa (incoherencias/congruencia), con el detalle para corregir o enrutar (1P → TIP, 3P → Marketplace).
- El formato/estructura de cada documento (un doc multi-guía vs. varios, cómo ordena las referencias) queda **a definir**.

### 5 · Configuración del motor (admin)
Gestión del "cerebro" del motor, no solo listas:
- **Prompt** + **documentos .md** que lo alimentan (criterios, casuísticas, ejemplos), **por sección/tipología**.
- **Vocabulario/listas:** excepciones ortográficas, tabla de colores (+equivalencias), materiales (desambiguación tipo "teca"), abreviaturas/símbolos.
- **Versionado / trazabilidad:** cada versión queda registrada y se sabe con cuál se generó cada informe (para mantenerlo "explicable y repetible").
- **Loop de mejora:** un falso positivo revisado en la pantalla 3 llega aquí como propuesta para afinar el prompt/.md.

### Flujos clave

**Principal:** Nueva auditoría → ejecución por lotes → *Pendiente de revisión* → **revisión de falsos positivos** → *Revisado* → **exports (interno + seller)** → (siguiente ciclo) re-auditoría.

**Revisión de falsos positivos (cómo funciona):**
1. Tras la auditoría → informe *Pendiente de revisión* con todas las alertas.
2. El validador de gama marca cada alerta **válida** o **falso positivo** (puede ser en bloque: "todas las de este tipo/término").
3. Al marcar **falso positivo** se pide un **motivo/categoría** (término técnico válido · equivalencia de color · material mal clasificado · contexto…) y: la alerta **sale del informe** + el caso entra en una **cola hacia Configuración del motor** para incorporarlo al vocabulario/prompt/.md → deja de saltar la próxima vez.
4. Al terminar la depuración, el informe pasa a *Revisado* y ya se puede **exportar/enviar**.
Esto cierra el paso 3 → paso 4 de su workflow dentro de la herramienta (hoy en un Drive suelto).

**Gobernanza (fuera de la herramienta):** informe → Jira (Calidad del Catálogo / Información Producto) → TIP / Marketplace / seller corrigen → re-auditoría. La herramienta **produce los artefactos**, no gestiona Jira.

---

## 5. Pendiente de descubrir

**Sigue bloqueando diseño / alcance:**
1. **Acceso a datos:** confirmar BigQuery 1P y **qué campos expone** (ficha técnica/atributos, seller/proveedor, 1P/3P, sección/tipología, **orphan/ubicación en familia** para el Health Score) y si existe el **mapeo PLP/categoría-web ↔ referencias**.
2. **Motor actual:** walkthrough (cómo suben, output, entorno) y **si ya usa LLM** por detrás o el "prompt" son plantillas/instrucciones.
3. **El loop recurrente:** cómo se articulan hub, ejecuciones, histórico y **comparación de periodos** (evolutivo).
4. **Granularidad y formato del informe:** unidad (categoría/guía), agregación (modelo), y si el entregable es un doc multi-guía o varios.
5. **Formato del perímetro "Listado":** el cliente lo contempla como **lista de URLs de PDPs** (PDF pág. 37); el prototipo lo montó como **CSV de SKUs**. Alinear el formato (URLs de PDP vs. SKUs) y, si es CSV, si las referencias deben validarse contra el catálogo antes de aceptarlas (como en Descripciones). *Relacionado:* el PDF describe la ingesta como URL-based (A: PLP, B: listado de URLs), mientras el análisis asume acceso directo **1P vía BigQuery** con selectores (modelo/gama/proveedor/seller) → confirmar la vía real (ver punto 1).
6. **Significado de las letras de gama** (A/M/S/C/K/B): qué tier/criterio de surtido representa cada una y si una referencia puede pertenecer a más de una. Necesario para el subtexto del selector de "Gama" y para la lectura del informe.
7. **Atributos obligatorios faltantes — severidad vs. prioridad:** el PDF los trata como **Bloqueante** (pág. 34) pero prioridad **Media** (Bloque 1). Hipótesis del cliente: si *impide publicar*, al menos no confunde (no está visible); si *está publicada y confunde*, es peor. Confirmar si faltar obligatorios impide publicar o solo degrada. *(Propuesta provisional: Bloqueante + Media, por ser ejes distintos.)*
8. **Alcance y campos de la consulta a BigQuery:** (a) ¿traerá también las referencias que **hoy no aparecen en la web** (no publicadas)? Determina si las **bloqueantes/no-publicadas** entran en el perímetro, en el conteo y en el Health Score — y condiciona el punto 7. (b) ¿Expone el **campo de designación administrativa (ADM)**? Es necesario para los dos checks de ADM ("posible designación administrativa" e "info en ADM no reflejada").
9. **Owner de "Estructura de designación":** el dato crudo puede aportarlo el **seller**, pero su **presencia en la designación** (construcción del título) puede ser tarea del **TIP**. Confirmar el owner (propuesta provisional: Seller).

---

## 6. Fases

**MVP — solo 1P**
- Entrada: acceso directo **1P (BigQuery)**, **perímetro por un solo tipo** (**modelo / gama / proveedor-seller unificado / categoría web (PLP) / listado**). *El PDF (pág. 37) describe la ingesta como (A) URL de PLP y (B) listado de URLs de PDPs; el "listado" del prototipo es CSV de SKUs → **formato a alinear**.*
- Detección: **designación + descripción** (+ **ficha/atributos** si hay dato) + **cumplimiento de guía**.
- **Health Score base** (criterios auditables, misma escala que pág. 39).
- Informe interno con **impacto por tipo de error** + **matriz TIP/seller**, con marca 1P/3P y severidad básica.
- **Dos exports separados por audiencia** (seller: PDF acotado + CSV/XLSX; interno TIP/ecommerce).
- **Revisión de falsos positivos** (En curso → Pendiente de revisión → Revisado → Finalizada) con **loop** al motor.
- **Configuración del motor** (prompt + .md + vocabulario), versionada, por sección/tipología.
- **Histórico de cambios desde el día 1** (para alimentar la fase 3 y el evolutivo).
- **Re-auditoría bajo demanda** sobre la auditoría previa (mismo perímetro) — **no periódica**: se lanza cuando saben que **se han corregido los datos**. Compara contra la ejecución anterior y muestra **Corregidos / Persisten / Nuevos**. *(La automática/programada es fase posterior.)*

**Fase 2**
- **3P** (+ crawling solo si no hay dato directo), **contenido premium** (Contentful, vía API/headless; reto = formato heterogéneo, ~90k refs), **imágenes** (color/medidas), **Health Score completo** (+imágenes/reviews), **evolutivo / comparación de periodos** consolidado, **alerta de cambio de datos → re-auditoría**, comparativa/rankings por sección, pack agregado por seller.

**Fase 3**
- **Analítica Data-to-Revenue** (integración con Digital Analytics, correlación de tramos del Health Score con KPIs), **re-auditoría automática**.
- *(Categorización en navegación queda fuera — la trabaja ADEO.)*
