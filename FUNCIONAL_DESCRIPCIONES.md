# Módulo Descripciones — Documentación funcional

> Documento de traspaso pensado para **leerse de corrido** y entender cómo funciona el módulo, sin necesidad de consultar la especificación técnica.
> Refleja la funcionalidad **actual** del prototipo. **No** aplica los recortes propuestos en `PROPUESTA_RECORTES_DESCRIPCIONES.md` (aún sin confirmar): aquí todo se describe como vigente.
> Para el detalle exacto y verificable (requisitos y escenarios), ver la especificación en `openspec/specs/descripciones/spec.md`.

---

## 1. Qué es y para qué sirve

El módulo **Descripciones** permite **generar de forma masiva** las descripciones de producto de un modelo, cruzando tres insumos:

1. Una **guía de estilo publicada** (plantillas de designación/descripción, atributos y ejemplos definidos en el módulo de Guías).
2. Los **valores reales de atributos por SKU**, subidos mediante un **CSV**. Es el puente manual hasta que exista integración directa con el catálogo / Mirakl.
3. Un **prompt configurable** (tono, longitud, palabras prohibidas e instrucciones).

El resultado de cada ejecución es una **orden**, cuyo output es un **CSV listo para importar en Mirakl**.

En una frase: *generación masiva de descripciones de producto para un modelo, cruzando su guía de estilo, los atributos por SKU (vía CSV) y un prompt configurable, con salida en un CSV listo para Mirakl.*

---

## 2. Quién lo usa

Lo usan los equipos de contenido y catálogo, que ya trabajan con las guías de estilo publicadas. El acceso general a la aplicación se rige por roles (Administrador General, e-Merch, TIP, 3P Marketplace); el detalle de permisos por rol dentro de Descripciones está **pendiente de definir** y no se cubre en este documento.

---

## 3. Conceptos clave

- **Orden**: una ejecución de generación masiva. Es la unidad central del módulo; tiene un estado, un modelo, una guía, un alcance de SKUs y un resultado descargable.
- **Alcance de SKUs**: sobre qué SKUs del modelo se genera (todos, los de un CSV, o solo los que fallaron antes).
- **Borrador**: una configuración de orden guardada a medias (o recuperada tras cancelar), lista para retomarse.
- **Reintento y orden raíz**: cuando se vuelve a generar sobre los SKUs que fallaron, el reintento se **encadena siempre a la orden original ("raíz")**, para evitar cadenas indefinidas de reintentos.
- **Guía publicada**: la referencia editorial del modelo. El módulo la consume, no la edita (eso es el módulo de Guías).

---

## 4. Flujo principal (paso a paso)

El punto de partida es el **listado de órdenes**. Desde ahí se pulsa **"Nueva orden"** y se recorre el asistente:

1. **Elegir guía** — se muestra la lista de guías publicadas con buscador. Se selecciona una.
2. **Elegir modelo** — se muestra la lista de modelos de esa guía. Se selecciona uno. (Se puede volver a cambiar de guía con "Cambiar".)
3. **Definir el alcance de SKUs** — se elige sobre qué SKUs se genera:
   - **Todos los SKU's del modelo**: el sistema consulta el catálogo y muestra cuántos SKUs ha detectado.
   - **Subir selección (CSV)**: se sube un CSV; el sistema valida que esos SKUs pertenecen al modelo antes de aceptarlo.
   - **Solo los que fallaron**: disponible solo cuando se parte de una orden anterior; acota a los SKUs que quedaron sin generar.
4. **Configurar y previsualizar** — se ajustan tono, longitud, blacklist de palabras y prompt; y se genera una **vista previa** de ejemplos con puntuación de calidad. La generación masiva **solo se habilita tras ejecutar al menos una vista previa**.
5. **Generar** — se lanza la generación, que muestra el progreso. Se puede seguir en primer plano o **dejarla en segundo plano** y volver al listado.
6. **Descargar** — al terminar, se descarga el **CSV** resultante (o se consulta el detalle de la orden).

En cualquier momento del asistente se puede **salir guardando un borrador** para retomarlo después.

---

## 5. Estados de una orden y ciclo de vida

Una orden pasa por estos estados (con su indicador visual):

| Estado | Qué significa |
|---|---|
| **En curso** | La generación se está ejecutando; muestra el porcentaje de progreso. |
| **Pausado** | Una orden en curso que se ha detenido temporalmente, conservando el progreso. |
| **Error** | La generación falló globalmente a mitad de proceso (p. ej. se perdió la conexión con el catálogo). Conserva lo ya generado. |
| **Lista** | Completada y **aún no descargada** (se marca como pendiente de descarga). |
| **Descargada** | Completada y ya descargada al menos una vez. |
| **Expirada** | Completada cuyo CSV ha caducado; ya no se puede descargar ni ver su detalle. |
| **Borrador** | Configuración guardada a medias o recuperada tras cancelar. |

**Transiciones principales:**

- **En curso → Pausado → En curso**: con "Pausar" / "Reanudar".
- **En curso o Pausado → Borrador**: al **cancelar**, la orden **no** queda como "cancelada"; se **convierte en borrador**, conservando su configuración (guía/modelo/prompt/alcance), no el progreso.
- **En curso → Lista**: al terminar la generación.
- **Lista → Descargada**: al descargar el CSV.
- **Error → En curso**: con "Reintentar" (regenera los SKUs pendientes del fallo).
- **Lista/Descargada → Expirada**: cuando caduca el CSV pasado el plazo de retención.

---

## 6. Pantallas principales

### Listado de órdenes (hub)
Es la pantalla de inicio del módulo y el centro de todo. Ofrece:

- **Pestañas**: "Todo", "En curso", "Finalizadas", "Borradores" y "Expiradas", con sus contadores. La pestaña "Finalizadas" muestra un **punto rojo** cuando hay órdenes sin descargar.
- **Buscador** por modelo, guía o código de orden.
- **Ordenación** por columnas (Orden, Modelo, SKUs, Estado, Fecha, Creador), con orden por defecto por fecha descendente.
- **Acciones por fila según el estado**: Pausar/Reanudar/Cancelar (en curso), Descargar CSV + Nueva orden (completada), Reintentar (error), Nueva orden (expirada), y un menú con "Ver detalle" y "Eliminar orden".
- **Indicador de descarga**: las completadas sin descargar muestran el badge "Lista" y una marca de "sin descargar"; al descargar pasan a "Descargada".
- **Referencia a la guía**: junto al nombre del modelo, un desplegable muestra la guía, su última actualización y un enlace "Ver guía".
- **Marca de reintento**: las órdenes que son reintento muestran junto a su código la orden raíz de la que derivan.
- **Listado de borradores** propio, con opción de "Continuar" o eliminar.

### Detalle de orden
Muestra el progreso y resultado de una orden, adaptándose a su estado:

- **En curso**: SKUs procesados, porcentaje, descripciones generadas y sin generar; botones Pausar / Cancelar.
- **Completada**: totales de procesados, generadas y sin generar; botones Nueva orden / Descargar CSV.
- **Error**: desglose de **cuatro cifras** (procesados antes del fallo, generados, sin generar por falta de atributos, y pendientes por el fallo) + un mensaje explicando el motivo; botón Reintentar.
- **Borrador**: informa de que se canceló y guardó como borrador; botón Continuar.

Debajo, una **tabla de SKUs sin descripción generada**, con su producto, seller y **atributos que faltan** en el catálogo. La tabla es ordenable, se puede **filtrar por seller** y **descargar un informe** del subconjunto filtrado.

### Selección de SKUs
Donde se acota el alcance (todos / CSV / solo fallidos). Al subir un CSV, valida que los SKUs pertenezcan al modelo y rechaza los que no. Cuando se parte de una orden anterior, muestra un **banner de reutilización** avisando de que la configuración proviene de esa orden y puede editarse.

### Configuración y vista previa
- **Configuración**: tono (informativo, persuasivo, técnico, cercano, premium), longitud (mín./máx. de caracteres), **blacklist** de palabras prohibidas (chips eliminables) y un **prompt** de instrucciones adicionales.
- **Vista previa**: genera ejemplos con una **puntuación de calidad** por ejemplo. Los ejemplos con **puntuación baja** muestran **sugerencias concretas de mejora**. Hasta no ejecutar la vista previa, no se puede lanzar la generación masiva.

### Generación
Ocurre en una ventana de progreso: muestra el avance, permite **verlo en detalle** o **continuar en segundo plano**, y al completar ofrece **Descargar CSV**, **Ver detalles** o salir. Cancelar durante la generación pide confirmación.

---

## 7. Reglas de negocio y casos especiales

- **Vista previa obligatoria**: no se puede generar en masa sin haber previsualizado antes.
- **Validación del CSV de SKUs**: solo se aceptan CSVs cuyos SKUs pertenezcan al modelo; si hay SKUs ajenos, se informa y no se deja continuar.
- **Reutilizar una orden anterior**: desde una orden completada o expirada se puede lanzar una nueva reutilizando su guía/modelo/prompt, con la opción de generar **solo los SKUs que fallaron**.
- **Reintentos encadenados**: un reintento siempre se vincula a la **orden raíz** original.
- **SKUs sin descripción**: un SKU no se genera cuando **faltan atributos obligatorios en el catálogo**; el detalle de la orden lista esos SKUs y qué atributos faltan.
- **Fallo global (Error)**: si la generación se corta a mitad (p. ej. caída de conexión con el catálogo), las descripciones ya generadas **no se pierden**; el reintento completa el resto.
- **Expiración**: las órdenes cuyo CSV caduca dejan de ser descargables y consultables; solo permiten crear una nueva orden a partir de ellas.

---

## 8. Relación con otros módulos

- **Consume** guías publicadas del módulo de **Guías** (no las edita).
- **Produce** un CSV pensado para **importar en Mirakl**.
- El **CSV de atributos por SKU** es el **puente manual** actual; en el futuro se prevé integración directa con el catálogo de Leroy Merlin / Mirakl.

---

## 9. Referencias

- **Especificación técnica (detalle exacto, requisitos y escenarios):** `openspec/specs/descripciones/spec.md`
- **Análisis de posibles recortes (sin decisiones tomadas):** `PROPUESTA_RECORTES_DESCRIPCIONES.md`
- **Contexto de producto del sistema completo (Guías de Estilo):** `PRODUCT.md`
