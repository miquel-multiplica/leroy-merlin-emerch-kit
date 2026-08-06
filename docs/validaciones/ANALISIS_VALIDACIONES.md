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

**Datos:** acceso directo (BigQuery) es **casi requisito**; el crawling queda casi descartado por eficiencia. **1P: hay acceso directo. 3P: puede que no** → 3P (y crawling) para más adelante. Los filtros de perímetro (incluida la PLP) solo acotan **qué** referencias entran; el **dato siempre se lee de la fuente directa**, no de la página.

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
**En curso → Pendiente de revisión → Revisado → Enviada** (+ Error). *(No hay concepto de "borrador" — eso en Descripciones significa otra cosa.)* El **perímetro** persiste entre ejecuciones para el siguiente ciclo.

---

### 1 · Hub de auditorías
La entidad que persiste es el **perímetro/auditoría recurrente**; cada pasada es una **ejecución (snapshot)** con su Health Score y sus alertas.
- **Columnas:** perímetro auditado (con **chip de tipo** dentro de la celda: Modelo / Gama / Proveedor-Seller / Categoría web (PLP) / Listado) · nº de referencias · **Health Score (base)** · **tendencia** (↑/↓ vs. ejecución anterior) · nº de alertas · última ejecución · creador · estado.
- **Pestañas:** **Todo · En curso · Por revisar · Con seguimiento (activas)**.
- **Filtros:** sección/tipología, 1P/3P (este último aplica cuando entre 3P).
- **Acciones por fila:** Ver informe · Exportar · **Re-auditar** · eliminar.
- **Entradas:** botón "Nueva auditoría" **y** desde Guías ("Empezar validación" de esa guía/categoría).

> **Punto de diseño a afinar:** cómo se articula el hub como **loop recurrente** (perímetros con histórico de ejecuciones + comparación de periodos / evolutivo). Es de lo más valioso del módulo y conecta con su evolutivo semanal; lo dejamos abierto.

### 2 · Nueva auditoría
Muy ligera (no es un funnel largo). En el prototipo: **modal de tipos (botones anchos, sin radios: clicas y avanzas) → wizard de 2 columnas**. Izquierda: **tipo de perímetro** (card con icono + lápiz para cambiarlo) y su **selector**. Derecha: **"Tu selección"** — cabecera con el perímetro + **SKUs detectados** + caja azul **"¿Qué se validará?"**.

- **Perímetro = un solo tipo por auditoría** (no se combinan varios filtros). Tipos:
  - **Modelo · Gama · Proveedor/Seller** (**un único selector unificado**, con **tag 1P/3P**) → buscador + lista; al elegir, la selección se confirma en la card derecha (para cambiarla, **lápiz en su esquina superior derecha** → resetea y vuelve al listado).
  - **Categoría web** → se pega la **URL de la PLP** y se pulsa **Consultar** (validación de formato tolerante: admite sin `http`, con o sin `www`).
  - **Listado** → subir un listado de referencias con **nombre propio editable** (se edita inline en la card derecha). **Sí está en el PDF del cliente** (pág. 37, "Cómo alimentamos la herramienta" → *B · Listado de URLs de PDPs*, 1P y 3P), pero **con matiz de formato**: el cliente lo describe como **lista de URLs de PDPs**, y el prototipo lo montó como **CSV de SKUs** (por analogía con Descripciones). **Formato a alinear.**
- Todos resuelven a una lista de referencias cuyos **datos se leen de BigQuery (1P)**. Si el perímetro va ligado a una guía/categoría, se activan los checks de **cumplimiento**.
- **Empezar auditoría de calidad** → progreso **por lotes** (barra **global**, no reinicia por lote) → *Pendiente de revisión*. La ventana es **cancelable** (con confirmación) y permite **continuar en segundo plano** (la auditoría aparece como *En curso* en el hub). **Sin pausa.**

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

---

## 6. Fases

**MVP — solo 1P**
- Entrada: acceso directo **1P (BigQuery)**, **perímetro por un solo tipo** (**modelo / gama / proveedor-seller unificado / categoría web (PLP) / listado**). *El PDF (pág. 37) describe la ingesta como (A) URL de PLP y (B) listado de URLs de PDPs; el "listado" del prototipo es CSV de SKUs → **formato a alinear**.*
- Detección: **designación + descripción** (+ **ficha/atributos** si hay dato) + **cumplimiento de guía**.
- **Health Score base** (criterios auditables, misma escala que pág. 39).
- Informe interno con **impacto por tipo de error** + **matriz TIP/seller**, con marca 1P/3P y severidad básica.
- **Dos exports separados por audiencia** (seller: PDF acotado + CSV/XLSX; interno TIP/ecommerce).
- **Revisión de falsos positivos** (En curso → Pendiente de revisión → Revisado → Enviada) con **loop** al motor.
- **Configuración del motor** (prompt + .md + vocabulario), versionada, por sección/tipología.
- **Histórico de cambios desde el día 1** (para alimentar la fase 3 y el evolutivo).
- **Re-auditoría manual** sobre la auditoría previa (mismo perímetro: seller / categoría web / modelo). *(La automática es fase posterior.)*

**Fase 2**
- **3P** (+ crawling solo si no hay dato directo), **contenido premium** (Contentful, vía API/headless; reto = formato heterogéneo, ~90k refs), **imágenes** (color/medidas), **Health Score completo** (+imágenes/reviews), **evolutivo / comparación de periodos** consolidado, **alerta de cambio de datos → re-auditoría**, comparativa/rankings por sección, pack agregado por seller.

**Fase 3**
- **Analítica Data-to-Revenue** (integración con Digital Analytics, correlación de tramos del Health Score con KPIs), **re-auditoría automática**.
- *(Categorización en navegación queda fuera — la trabaja ADEO.)*
