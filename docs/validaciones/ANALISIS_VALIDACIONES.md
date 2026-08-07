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

**Health Score — es su modelo existente (pág. 39); lo reutilizamos por fases (no un score paralelo).** Su modelo puntúa cada referencia sobre **10 puntos** y **se presenta como % (0–100)** — el score /10 se lee como porcentaje (la cabecera del deck muestra p. ej. «74%», pág. 29):
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
- **Alerta** (un problema concreto en una referencia): *tipo* → *categoría* · *severidad* (**Bloqueante / Crítica / Leve**) · owner · segmento (1P/3P).
- **Referencia** (0..N alertas): su **Estado** lo marca **la peor alerta que tiene** (gravedad), con la acumulación por cantidad como vía secundaria. Cuatro estados, por precedencia:
  - **No publicable** — ≥1 alerta **Bloqueante** (falta designación/descripción o atributos obligatorios: no alcanza mínimos → no se publica).
  - **Con errores críticos** — ≥1 alerta **Crítica** (contradicción entre piezas de la ficha, ya publicada → engaña) **o** **3+ alertas** acumuladas.
  - **Con errores leves** — 1–2 alertas, **todas Leve** (formato / estructura / ortografía).
  - **Sin errores** — 0 alertas.
- **Auditoría** (N referencias): Health Score · reparto de referencias por Estado (4 cajas) · distribución por tipo · impacto por categoría.

**Coherencia severidad ↔ estado.** Cada severidad de alerta empuja a su caja 1:1 → **Bloqueante → No publicable · Crítica → Con errores críticos · Leve → Con errores leves**. La colisión de la palabra "crítico" desaparece (alerta Crítica → ficha crítica, refuerza en vez de contradecir); lo único distinto es **Bloqueante** (severidad de la alerta) vs **No publicable** (estado de la ficha) → se lee como **causa → efecto**, no es colisión.

**Modelo propuesto (4 estados) — diverge del artefacto del cliente en 3 puntos, a validar (ver §5):**
1. **Mejora → Leve** (naming): el deck usa "Mejora" (habla de *oportunidad*); lo pasamos a "Leve" para que las tres severidades hablen de *gravedad* y encajen con las cajitas en una sola escala.
2. **La falta esencial sale de "críticos" y va a "No publicable"**: es Bloqueante (no está en la web); el artefacto la metía en "críticos" solo por no tener caja propia.
3. **Una contradicción (Crítica) escala sola a "críticos"** aunque sea la única alerta; el artefacto, al contar solo cantidad, la dejaba en "advertencias/leves".
La vía de **acumulación (3+ alertas)** del artefacto **se conserva** igual.

Las **cajitas** del informe tienen **dos lentes**: **Estado/gravedad** (Sin errores / Con errores leves / Con errores críticos / **No publicable** — **excluyentes, suman el total**) y **por tipo** (con discrepancias / con faltas de ortografía — **solapan**, no suman).

**Paleta única de severidad** (una sola escala en chips de la matriz, cajitas de estado y barritas de "Distribución de errores", para leerse coherente): **Leve = dorado** (`#b8860b` gráfico / `#7a5a00` texto) · **Crítica = rojo oscuro** (`#7a0d0d`) · **Bloqueante/No publicable = rojo** (`#c61112`) · Sin errores = verde. Contraste **WCAG AA** verificado (texto ≥4.5:1, gráficos ≥3:1). En "Distribución de errores" cada barrita se pinta por la severidad de su tipo y las filas se **ordenan por gravedad** (Bloqueante → Crítica → Leve, y dentro por %).

**Mapa de tipos de error · severidad · prioridad (propuesta — con puntos pendientes, ver §5).** Tres ejes distintos que **no se derivan uno de otro**:
- **Tipo de error** (qué falla) → se agrupa en **categorías**.
- **Severidad** (por alerta): **Bloqueante / Crítica / Leve** (Severity Score, PDF pág. 34 — el deck la nombra "Mejora"; la renombramos a "Leve", ver divergencia arriba).
- **Prioridad de resolución** (por categoría, motivo de negocio): **Alta / Media / Baja** (PDF Bloque 1). **Fija por categoría** — el volumen (*SKUs afectados / % catálogo*) se muestra aparte y **no altera** la prioridad.

| Categoría | Tipos que agrupa | Familia | Owner | Severidad | Prioridad · motivo |
|---|---|---|---|---|---|
| Contenido faltante | sin designación, sin descripción | Cumplimiento | Seller | Bloqueante | **Alta** · impide publicar |
| Atributos obligatorios faltantes | atributos básicos/específicos de la guía vacíos | Cumplimiento | Seller | **Bloqueante** ⚠️ | **Media** · afecta filtros de búsqueda |
| Discrepancia interna PDP | medidas/dimensiones, color, material, nº de elementos (desig ↔ desc ↔ ficha técnica) | Coherencia | TIP | Crítica | **Alta** · confunde al usuario / devoluciones |
| Estructura de designación | designación corta, estructura de título, mayúsculas admin, unidades, coma decimal, dims reordenadas | Cumplimiento | Seller* | Leve | **Baja** · SEO / legibilidad |
| Ortografía | faltas en designación o descripción | Calidad | TIP | Leve | **Baja** · conversión |
| Imágenes | nº < mínimo global y por categoría de imagen *(MVP)* · análisis de contenido *(fase 2)* | Cumplimiento | Seller | Bloqueante | **Alta** · impide publicar |

**Regla de severidad** (por alerta): *Bloqueante* = falta contenido esencial o atributos obligatorios (no publicable) · *Crítica* = contradicción entre componentes (info engañosa) · *Leve* = formato/estructura/ortografía.

**Jerarquía tipo/categoría:** una **categoría** (nivel alto, 5) agrupa varios **tipos** (checks granulares). Dónde se ve cada eje: la **matriz** muestra *tipo* + *severidad* por fila · **"Impacto por categoría de error"** muestra *prioridad + motivo* por **categoría** (todas) · **"Distribución de errores"** muestra los *tipos granulares* por campo (mismo nivel que la columna "Tipo" de la matriz). Orden en el informe: métricas → distribución (colapsable) → impacto → matriz.

**La guía de estilo — qué especifica** (artefacto *"Generador de guías emerch"*, ver §5 fuentes). La guía es, **por modelo/categoría**, *lo que debe tener una ficha*:
- **Designación:** la **concatenación de atributos** que forma el título y su **orden** (p. ej. *Estilo (ATT_13826) + Color de la estructura (ATT_04261) + Tipo de casquillo (ATT_00869)*) + la **semántica SEO** que debe aparecer.
- **Descripción:** la **semántica SEO** exigida + **qué atributos** deben aparecer en el texto.
- **Ficha técnica:** los **atributos básicos y específicos** obligatorios.
- **Multimedia:** el **mínimo global de imágenes** y, por **categoría de imagen** (Packshot, Ambient, Technical zoom, Material zoom, 360, Product benefit, Packaging…), su **mín/máx**.

La guía es **dato estructurado** (IDs de atributo `ATT_*`, términos SEO, min/máx de imágenes) → los checks de *presencia de atributo* y *conteo de imágenes* son **deterministas**; comprobar que el atributo/SEO **aparece en el texto libre** (descripción, título) necesita **LLM**.

**Dos orígenes de regla** (dimensión distinta de la *categoría* y del *motor*):
- **Guía de estilo** — cumplimiento del spec de arriba, **específico por categoría/modelo**.
- **Reglas generales** — *prompt transversal* que aplica a **todo** el catálogo, da igual la categoría: designación administrativa, longitud, unidades / coma decimal, mayúsculas/formato, ortografía, y las **coherencias** internas (contradicciones desig↔desc↔ficha).

**Tipos granulares → categoría · severidad · origen · motor:**

| Tipo de error (check) | Categoría | Severidad | Origen | Motor |
|---|---|---|---|---|
| Sin designación · Sin descripción (campo vacío) | Contenido faltante | Bloqueante | **General** | determinista |
| Atributo **obligatorio** de la guía ausente en **ficha técnica** | Atributos obligatorios faltantes | Bloqueante | **Guía** | determinista |
| Atributo exigido por la guía no aparece en la **descripción** | Estructura de designación | Leve | **Guía** | LLM / determinista |
| Designación no sigue la concatenación de atributos de la guía (ATTs / orden) | Estructura de designación | Leve | **Guía** | comparación (determinista) |
| Falta semántica SEO exigida (designación / descripción) | Estructura de designación | Leve | **Guía** | LLM |
| **Nº de imágenes < mínimo** (global y por categoría de imagen) | Imágenes | Bloqueante | **Guía** | determinista *(MVP: solo cuenta imágenes)* |
| Análisis de **contenido** de imagen (color del producto ≠ descrito, medidas de la imagen…) | Imágenes | Crítica | **General** (coherencia) | visión / LLM *(fase 2)* |
| Designación corta · Descripción corta | Estructura de designación | Leve | **General** | determinista |
| **Posible designación administrativa** (nombre interno en campo público) | Estructura de designación | Leve | **General** | determinista (mayúsculas) |
| Unidades no normalizadas · Coma decimal · Dims reordenadas | Estructura de designación | Leve | **General** | comparación (determinista) |
| Discrepancia de medidas · Dimensión extra · Diferente nº de dimensiones · Medida no encontrada | Discrepancia interna PDP | Crítica | **General** (coherencia) | comparación de datos |
| Color · Material incoherentes | Discrepancia interna PDP | Crítica | **General** (coherencia) | LLM (semántico: "gris"≟"antracita") |
| **Info en ADM no reflejada** | Discrepancia interna PDP | Crítica | **General** (coherencia) | comparación (+ LLM) |
| Ortografía (designación / descripción) | Ortografía | Leve | **General** | diccionario / LLM |

**Designación administrativa (ADM) — glosario.** Hay **dos** designaciones: la **administrativa/ADM** (nombre **interno** de back-office, normalmente en MAYÚSCULAS, abreviado) y la **comercial** ("designación cliente larga", la que ve el cliente). De ahí dos checks:
- **"Posible designación administrativa"**: la designación pública **parece la interna** (la detecta por venir *todo en mayúsculas*) → han puesto el nombre interno en el campo público. → categoría *Estructura de designación* · Leve.
- **"Info en ADM no reflejada"**: el ADM tiene **datos que no aparecen** en la ficha comercial/descripción. → categoría *Discrepancia interna PDP* · Crítica.

*Ambos dependen del **campo ADM en origen** → confirmar que BigQuery lo expone (ver §5.8).*

\* *Owner "Estructura de designación": el dato crudo puede aportarlo el seller pero no aparecer en la designación (construcción del título) → owner a confirmar (ver §5).*

**Datos:** acceso directo (BigQuery) es **casi requisito**; el crawling queda casi descartado por eficiencia. **1P: hay acceso directo. 3P: puede que no** → 3P (y crawling) para más adelante. Los filtros de perímetro (incluida la PLP) solo acotan **qué** referencias entran; el **dato siempre se lee de la fuente directa**, no de la página.

**Herramienta actual del cliente (baseline).** Hoy usan un *"Validador de Fichas de Producto"* propio: un motor de **reglas deterministas en el navegador** que audita **designación y descripción** (sin BigQuery ni LLM). Tiene tres módulos: **Validador** (subir el **CSV de una gama** → analizar → tabla de resultados + detalle por referencia → export CSV, normal y "un error por fila"), **Evolutivo semanal** (comparar → **✅ Corregidos · ➖ Persisten · 🆕 Nuevos**) e **Histórico**. Sus checks van tipificados con emoji (📐 medidas/dimensiones, 🎨 color, 🪵 material, 🔤 ortografía, ⚠️ formato, ❌ falta, 🔄 orden) y un **Estado** por SKU (OK / Advertencias / Errores críticos → en nuestro módulo lo renombramos a *Sin errores / Con errores leves / Con errores críticos*). Nuestro módulo es la **evolución**: motor híbrido, coherencia con **ficha/atributos** (no solo texto), cumplimiento vs guía, Health Score, revisión de falsos positivos y exports por audiencia. *(Artefacto y CSV de ejemplo en `docs/validaciones/docs de cliente/`.)*

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
**En curso → Pendiente de revisión → Revisada** (+ **Error** · + **Borrador**). El estado terminal es **Revisada** (no "Finalizada"/"Enviada"/"Con seguimiento"): la herramienta **produce los artefactos pero no controla el flujo posterior** (corrección del seller / gobernanza va por fuera, en Jira), así que no promete un seguimiento que no gestiona. *(Decisión: se **fusionó "Finalizada" con "Revisada"** — "Finalizada" no correspondía a ninguna acción de la herramienta.)*

- **Borrador** (lateral): una **primera** auditoría *En curso* **cancelada desde el hub o su ficha** queda como **Borrador** con el **perímetro guardado** (el modal avisa). Aparece en la pestaña *Borradores* como fila con **Continuar / Eliminar** (sin ficha). Cancelar **dentro del funnel** de creación **no** crea borrador → te deja en el wizard con el perímetro intacto. *(Alineado con Descripciones.)*
- **Re-auditoría — transiciones (pendiente, ver §5.15):** cancelar una **re-auditoría** **no** debe crear borrador (ya existe un informe previo). Propuesta provisional: la auditoría = **perímetro con histórico de ejecuciones**; re-auditar lanza una nueva ejecución sobre la misma fila; **cancelarla aborta y vuelve al último estado completado (*Revisada*)**, con el informe previo intacto. Regla de código: el cancelar ramifica según *¿hay ejecución previa completada?* (sin previa → Borrador; con previa → vuelve a Revisada).

El **perímetro** persiste entre ejecuciones para el siguiente ciclo.

---

### 1 · Hub de auditorías
La entidad que persiste es el **perímetro/auditoría recurrente**; cada pasada es una **ejecución (snapshot)** con su Health Score y sus alertas.
- **Columnas:** perímetro auditado (con **chip de tipo** dentro de la celda: Modelo / Gama / Proveedor-Seller / Categoría web (PLP) / Listado) · nº de referencias · **Health Score (base)** · **tendencia** (↑/↓ vs. ejecución anterior) · nº de alertas · última ejecución · creador · estado.
- **Pestañas:** **Todo · En curso · Pendientes de revisión · Revisadas · Borradores** (con contador). *(Los nombres de pestaña coinciden con los estados.)*
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

### 4 · Generar informes
La modal ofrece **tres informes** (los dos por audiencia son **independientes**, nunca un doc compartido; el tercero es interno para el motor). Cada uno con su botón **Descargar**:
- **Informe del seller/proveedor** (hacia fuera): **PDF minimalista y defensivo** — solo **lo que no cumple + qué corregir**, de **sus** referencias/familias; **sin** guía completa, **sin** estrategia de familia, **sin** orden de imágenes — **+ CSV/XLSX** de su matriz. El **CSV** incluye: **SKU · nombre del proveedor/seller · identificación 1P/3P · atributos sin completitud de dato**. *(No hay "PIM" — era un error de su slide.)*
- **Informe interno TIP / equipo ecommerce**: la vista de trabajo completa (incoherencias/congruencia), con el detalle para corregir o enrutar (1P → TIP, 3P → Marketplace).
- **Informe de falsos positivos** (Excel o CSV, interno): motivos + comentarios de las alertas descartadas, por tipo/regla → **insumo para Configuración del motor (admin)**. *(Su flujo exacto y si es descarga o conexión directa: ver §5.14.)*
- El formato/estructura de cada documento (un doc multi-guía vs. varios, cómo ordena las referencias) queda **a definir**.

**Contenido propuesto de cada informe** *(el PDF de guía `guia-gua-emerch-60.pdf` en `docs de cliente/` es la referencia del "debe ser": portada · Designación [concatenación de atributos + vista previa + ejemplos] · Descripción [atributos incluidos + ejemplos] · Atributos básicos y específicos · Multimedia [categorías de imagen mín-máx + regla de nombrado]).* 

**1. Informe del seller/proveedor — PDF (defensivo) + CSV.** Solo **sus** referencias y **solo lo suyo** (completitud + imágenes; **sin** coherencias internas, **sin** guía completa, **sin** estrategia de familia).
- **PDF** (por seller, tono constructivo "qué falta / cómo corregir"), estructurado como la guía:
  - Portada: seller · perímetro · fecha · resumen (nº refs, % conforme, nº refs con acciones).
  - Por referencia con incidencias: ref · modelo · designación actual · y, agrupado por sección de guía: **campos obligatorios vacíos** (designación/descripción) · **atributos obligatorios/básicos faltantes** (nombre legible + `ATT_*`) · **imágenes** (tiene X / mínimo Y, categorías que faltan — Packshot, Ambient… con la regla de nombrado) · estructura/SEO si aplica.
  - Cierre: cómo subir las correcciones / contacto.
- **CSV (por referencia):** `Referencia · Modelo · Designación actual · Segmento (1P/3P) · Estado · Campos vacíos · Atributos obligatorios faltantes (lista) · Nº imágenes / mínimo · Categorías de imagen faltantes · Acción principal`. Filtrado a sus refs; sin coherencia.

**2. Informe interno TIP / ecommerce — CSV (matriz enriquecida, una fila por alerta).** La vista de trabajo completa (coherencia + cumplimiento).
- **Columnas:** `Referencia · Modelo · Guía/Categoría · Tipo de error · Categoría de error · Severidad · Campo/ubicación (designación/descripción/ficha/multimedia) · Diagnóstico · Corrección sugerida · Origen (Guía/General) · Owner (TIP/Seller) · Segmento (1P/3P) · Seller · Estado de la referencia`. *(Equivale al "un error por fila" del cliente —Ref·Gama·Id modelo·Modelo·Estado·Campo·Error— enriquecido.)*
- *Opcional:* segunda hoja **resumen por referencia** (`Referencia · Modelo · Health Score · Estado · nº alertas por severidad`).

**3. Informe de falsos positivos — Excel/CSV (interno, para el admin/motor).** Una fila por alerta descartada como falso positivo → insumo para afinar el motor.
- **Columnas:** `Referencia · Modelo · Tipo de error · Categoría · Origen (Guía/General) · Motor (determinista/comparación/LLM) · Regla/check · Severidad · Diagnóstico original · Motivo(s) del FP · Comentario · Campo/ubicación · Seller · Perímetro · Revisor · Fecha`. Pensado para **pivotar por tipo/regla/motivo** y decidir la acción del motor (whitelist, sinónimos, umbral, prompt). Ver §5.14.

**Lo que exporta hoy su artefacto (dos formatos, por *granularidad* — no por audiencia; ejemplos en `docs de cliente/`):**
- **"Exportar resultados CSV"** → **una fila por referencia** (13 col., delimitado por `;`): el catálogo de entrada + `Estado` + `Errores Designación` + `Errores Descripción` (las alertas de cada campo **agregadas** en una celda). = catálogo anotado.
- **"Exportar (un error por fila)"** → **una fila por alerta** (7 col.): `Ref · Letra de gama · Id modelo · Modelo · Estado · Campo (Designación/Descripción) · Error`. `Estado` se repite por fila; incluye las OK como placeholder (`Campo = —`). = lista plana de trabajo.

**Cómo encaja con lo nuestro:** su **"un error por fila" ≈ nuestra matriz de trabajo**, pero **mínimo**. Nuestro **export interno TIP** = ese formato **enriquecido** con *owner · severidad · categoría · acción correctiva · guía · 1P/3P · seller*, conservando la columna **Campo/ubicación** (designación/descripción/**ficha técnica**) aunque en la UI de la matriz la ocultemos (redundante con el diagnóstico). **La granularidad encaja con la audiencia** (no son dos botones de formato sueltos): el **seller corrige por producto** → su export va **por referencia** (agregado por SKU, como "resultados", filtrado a **sus** refs y solo lo suyo — completitud/atributos, sin coherencia interna). El **TIP trabaja alerta a alerta** (marca falsos positivos, enruta, pivota por owner/severidad/tipo) → su export va **por alerta** (la matriz enriquecida). *Opcional:* el export interno puede incluir además un **resumen por referencia**, pero es secundario.

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
2. **La IA no prescribe falsos positivos: los detecta el humano.** El motor solo **levanta alertas** (unas por checks deterministas, otras por checks de IA — estas se marcan con un badge **IA**, que indica el **origen** de la alerta, no un veredicto). El validador de gama decide alerta a alerta si es **válida** o **falso positivo**. En la matriz, cada fila lee como un **error a corregir** (diagnóstico + corrección); la validez es el juicio del revisor, no está pre-escrita en la fila.
3. Un **falso positivo** = el motor marcó como error algo **válido** (un término/tecnicismo/sinónimo que aún no reconoce, o un contexto que malinterpretó) → sirve para **enseñar al motor** que es válido. Al marcarlo se piden **motivos** (multiselección, contextuales al tipo) + **comentario**. *(Las etiquetas de motivo por tipo son propuesta a validar — ver §5.)*
4. **Marcado (UI del prototipo).** Al pulsar *Falso positivo* se abre **"Selecciona el motivo del falso positivo"**: una **cajita resumen** (fondo gris, **colapsable**) con la **referencia (enlace a la PDP)** + modelo debajo, **tipo**, **severidad** y —si aplica— el **disparador**; luego los **motivos** (chips, multiselección) y un **comentario**. Si la alerta tiene un **disparador compartido** con otras, **encima de los motivos** aparece un aviso: *"Hay **N** referencias más con el mismo error en **M** modelos"* + botón **Revisarlas**.
5. **En bloque — alcance Familia → Modelo → Referencia.** El **"modelo" es el arquetipo con el que la guía define las reglas**; un mismo disparador aparece en muchas **referencias** repartidas por varios **modelos** de una **familia**. *Revisarlas* abre una **sub-vista "Marcar falso positivo en bloque"** dentro del mismo modal (con **← Atrás** al pie, **no** un overlay apilado): la **referencia original** va **arriba, marcada y fija** (siempre entra; no se puede desmarcar); debajo, las coincidencias **agrupadas por modelo** (el de la original **primero** + separador *"Otros modelos"*), **desmarcadas por defecto**, cada una con su **referencia (enlace a PDP)** y su **seller**, con **filtro por seller** (transversal, un seller puede estar en varios modelos). Se **selecciona por modelo** (arrastra sus referencias) o por referencia y se pulsa **Confirmar selección**. De vuelta al modal, el aviso pasa a una **cajita con borde verde** *"Se aplicará a **M** modelos · **N** referencias"* (con **lápiz** para reeditar) y el botón final es **dinámico**: *"Marcar N falsos positivos"*.
6. La alerta (o el bloque) **sale del informe** al cajón de falsos positivos, con **Motivo / Editar / Restaurar**. **Restaurar** una marcada en bloque **pregunta si deshacer también las demás**. La propuesta resultante (*"«término» válido en modelos […] · N referencias"*) es **insumo para Configuración del motor**; **cómo aterriza ahí es TBD** (ver §5.14) — probablemente fuera de la herramienta, y el admin lo aplica como considere.
7. Al terminar la depuración, el informe pasa a *Revisada* y ya se puede **exportar/enviar**. Esto cierra el paso 3 → paso 4 de su workflow dentro de la herramienta (hoy en un Drive suelto).

**Qué checks admiten falso positivo — no decidido (ver §5.16).** Hipótesis del prototipo: los checks de **juicio/interpretación** (discrepancias, ortografía, SEO semántico, designación administrativa) llevan botón de FP; las **ausencias objetivas** (sin descripción/designación, atributos vacíos, nº de imágenes, longitud) **no** (son hechos verificables, no un error de juicio). El **bloque** solo aparece cuando hay **disparador compartido**. **Sin confirmar** — puede que todos los tipos deban admitirlo.

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
10. **Normalización del Health Score base (MVP):** en MVP son auditables ~**5–7 de los 10 puntos** (designación/descripción/atributos = 3,5 + orphan = 1,5 + **conteo de imágenes**, que sí entra en MVP aunque el *análisis de contenido* de imagen sea fase 2); falta reviews (3) y la parte de imagen que exige visión. Confirmar cuánto del componente de imágenes (2 pts) cubre el solo conteo. ¿Cómo se expresa el número que se muestra? Opciones: (a) **% sobre los puntos auditables** (normalizado, comparable), (b) **/10 con los componentes no disponibles a 0** (penaliza y no es comparable con el score completo), (c) mostrarlo explícitamente como **base/parcial**. Afecta al número de la cabecera y a la **tendencia** entre ejecuciones. *(Se presenta como % — ver §2.)*
11. **¿Health Score calculado o ingerido?** ¿Lo **computa nuestro módulo** (a partir de los componentes que auditamos) o **viene ya calculado como dato de origen** (su sistema actual de medición de calidad / BigQuery expone el campo)? Determina si solo lo **mostramos** vs. lo **recalculamos**, y se relaciona con la normalización (punto 10) y con los campos de origen (§5.8). *(El PDF pág. 39 sugiere que ya tienen el modelo; confirmar si el valor viene dado o se recalcula.)*
12. **Granularidad de los exports (asunción a validar):** asumimos **seller = por referencia** (agregado por SKU) y **TIP = por alerta** (matriz enriquecida). Confirmar con cliente si es así, o si prefieren otra combinación (ambas granularidades por audiencia, un único formato, o el "resumen por referencia" también en el interno). Ver §4.
13. **Modelo de estados de la referencia (4 cajas) — diverge del artefacto en 3 puntos:** (a) severidad **"Mejora" → "Leve"** (naming, para una escala única de gravedad); (b) la **falta esencial** sale de "críticos" y pasa a **"No publicable"** (es Bloqueante = no está en la web); (c) una **contradicción (Crítica)** escala sola a "críticos" aunque sea la única alerta (el artefacto la dejaba en "advertencias" por contar solo cantidad). Se conserva la vía de **acumulación (3+ alertas)**. Confirmar con cliente que el estado se rija por **gravedad de la peor alerta** (con 3+ como vía secundaria), no solo por cantidad. Ver §2.
14. **Flujo de los falsos positivos → admin (pendiente de definir):** hoy al marcar un falso positivo se guardan motivos + comentario y "van a la cola de Configuración del motor". Falta definir **cómo se materializa esa conexión**: ¿el módulo escribe directamente en la config del motor, o se **exporta un informe de falsos positivos** (motivos + comentarios + tipo/regla) que es **con lo que trabaja el admin** para afinar prompt/.md/vocabulario? De ello depende el **copy del modal** (hoy provisional: *"Marca uno o varios motivos y añade un comentario si quieres. La alerta saldrá del informe y el caso irá a Configuración del motor para afinar la regla."*) y si hace falta un **export de falsos positivos** además de los dos exports por audiencia. *(También queda pendiente el **copy final del modal "¿Marcar como revisado?"**, que dependerá de ese mismo flujo de mejora del prompt del admin.)*
15. **Re-auditoría — transiciones de estado (pendiente):** definir el modelo del loop y, en concreto, que **cancelar una re-auditoría NO cree borrador**. Preguntas: (a) ¿auditoría = **perímetro con histórico de ejecuciones** (una fila que evoluciona) o **fila nueva por ejecución**?; (b) mientras re-audita, ¿badge "En curso" con el informe previo accesible por histórico?; (c) al cancelar la re-ejecución, **vuelve a *Revisada*** (informe previo intacto), sin borrador; (d) al terminar, informe con comparación **Corregidos/Persisten/Nuevos** → *Pendiente de revisión* → *Revisada*. *(Propuesta provisional: perímetro con histórico; cancelar ramifica según haya o no ejecución previa completada.)* Engancha con §5.3.
16. **¿Qué tipos de check admiten falso positivo?** **No decidido.** Hipótesis del prototipo: solo los checks de **juicio** (discrepancias, ortografía, SEO semántico, designación administrativa) muestran el botón de falso positivo; las **ausencias objetivas** (sin descripción/designación, atributos vacíos, imágenes < mínimo, longitud) no lo muestran (son hechos verificables, no un error de juicio del motor). Confirmar con cliente/motor si **todos** los tipos deben poder marcarse como FP o solo un subconjunto, y con qué criterio. Afecta a **qué filas de la matriz muestran la acción** y a qué disparadores permiten el **bloque**. *(Principio de base: la IA no prescribe el FP; lo detecta el humano.)*
17. **Datos del falso positivo en bloque:** el bloque asume que el motor expone el **disparador** (término/patrón que origina la alerta = clave de agrupación) y la jerarquía **Familia → Modelo (arquetipo de la guía) → Referencia** + el **seller** por referencia, además de **cómo calcula la equivalencia** (exacta vs IA). Confirmar que esos datos existen/son accesibles (BQ/motor); sin ellos el bloque no es fiable.

---

## 6. Fases

**MVP — solo 1P**
- Entrada: acceso directo **1P (BigQuery)**, **perímetro por un solo tipo** (**modelo / gama / proveedor-seller unificado / categoría web (PLP) / listado**). *El PDF (pág. 37) describe la ingesta como (A) URL de PLP y (B) listado de URLs de PDPs; el "listado" del prototipo es CSV de SKUs → **formato a alinear**.*
- Detección: **designación + descripción** (+ **ficha/atributos** si hay dato) + **cumplimiento de guía**.
- **Health Score base** (criterios auditables, misma escala que pág. 39).
- Informe interno con **impacto por tipo de error** + **matriz TIP/seller**, con marca 1P/3P y severidad básica.
- **Dos exports separados por audiencia** (seller: PDF acotado + CSV/XLSX; interno TIP/ecommerce).
- **Revisión de falsos positivos** (En curso → Pendiente de revisión → Revisada) con **loop** al motor.
- **Configuración del motor** (prompt + .md + vocabulario), versionada, por sección/tipología.
- **Histórico de cambios desde el día 1** (para alimentar la fase 3 y el evolutivo).
- **Re-auditoría bajo demanda** sobre la auditoría previa (mismo perímetro) — **no periódica**: se lanza cuando saben que **se han corregido los datos**. Compara contra la ejecución anterior y muestra **Corregidos / Persisten / Nuevos**. *(La automática/programada es fase posterior.)*

**Fase 2**
- **3P** (+ crawling solo si no hay dato directo), **contenido premium** (Contentful, vía API/headless; reto = formato heterogéneo, ~90k refs), **imágenes** (color/medidas), **Health Score completo** (+imágenes/reviews), **evolutivo / comparación de periodos** consolidado, **alerta de cambio de datos → re-auditoría**, comparativa/rankings por sección, pack agregado por seller.

**Fase 3**
- **Analítica Data-to-Revenue** (integración con Digital Analytics, correlación de tramos del Health Score con KPIs), **re-auditoría automática**.
- *(Categorización en navegación queda fuera — la trabaja ADEO.)*
