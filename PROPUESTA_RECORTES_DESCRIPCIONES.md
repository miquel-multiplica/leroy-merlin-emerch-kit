# Propuesta de recortes — Descripciones (eMerch Toolkit)

> **Estado del documento:** versión revisada (julio 2026), ampliada con los puntos 7–9 tras revisión completa del prototipo. Sin decisiones tomadas — pendiente de revisión por el equipo de desarrollo.

## Contexto

Este documento recoge un análisis de funcionalidades del módulo de Descripciones que son candidatas a simplificarse o eliminarse para reducir horas de desarrollo. Para cada punto se describe qué es, qué implica técnicamente construirlo, y cómo quedaría la experiencia si se recorta — sin ninguna decisión tomada. El objetivo es que el equipo de desarrollo lo revise y valore viabilidad y coste real antes de decidir nada.

---

## 1. Pausar / Reanudar la generación

**Qué es:** poder detener una generación en curso y continuarla más adelante desde el mismo punto, sin repetir trabajo ya hecho.

**Qué implica técnicamente:** que el proceso sea un job con estado persistente y reanudable — registrar SKU a SKU qué está hecho y qué no, poder detener el job sin perderlo, y reanudarlo sin duplicar llamadas a la IA ya realizadas.

**Nota de contexto:** el módulo mantiene la funcionalidad de "Cancelar → convertir en borrador" (una orden cancelada se recupera después con su configuración, no con el progreso ya generado). Convendría que desarrollo confirme si esa funcionalidad, tal como está planteada, comparte o no infraestructura con lo que exigiría Pausar/Reanudar — a falta de esa confirmación, no se puede dar por hecho que recortar esto ahorre horas de forma aislada.

**Si se recorta:** durante una generación en curso, la única acción disponible sería Cancelar.

---

## 2. Reutilizar configuración de una orden anterior ("Nueva orden a partir de existente" + "Solo los que fallaron")

**Qué es:** desde una orden ya finalizada, poder lanzar una nueva orden que reutiliza su guía/modelo/prompt, con la opción añadida de generar solo para los SKU's que fallaron la vez anterior.

**Qué implica:** vincular una orden nueva a la configuración y/o a los SKU's fallidos de la anterior, y encadenar reintentos siempre a la orden raíz original (para evitar cadenas de reintentos indefinidas).

**Dos grados de recorte posibles:**

- **Grado 1:** eliminar la funcionalidad por completo — toda orden nueva se configura desde cero. Elimina también la necesidad de vincular reintentos a una orden raíz.
- **Grado 2:** mantener la reutilización de guía/modelo/prompt, pero eliminar solo la opción "Solo los que fallaron". *Punto a valorar:* el desglose de qué SKU's fallaron por falta de atributos sería parte del output de cualquier generación completa, independientemente de si se ofrece o no esta opción — con lo que el ahorro real de este grado podría ser menor de lo que parece a simple vista. Convendría que desarrollo lo confirme.

**Nota — vínculo visible a la orden raíz:** si se mantiene esta funcionalidad (en cualquiera de los dos grados), el backend ya necesita saber cuál es la orden raíz de un reintento para poder encadenar correctamente. Mostrar esa referencia en el tooltip del icono de reintento (ej. "Reintento de la orden #1020") es coste prácticamente nulo sobre eso — ya está construido en el prototipo como referencia visual de cómo se vería.

---

## 3. Sugerencias de IA en la vista previa (para ejemplos con puntuación baja)

**Qué es:** cuando un ejemplo generado en la vista previa tiene una puntuación de calidad baja, se muestran sugerencias concretas de mejora.

**Qué implica:** una llamada adicional a IA para generar las sugerencias en el momento de mostrarlas; a confirmar con desarrollo si esto puede resolverse sin persistencia (generándose al vuelo cada vez) o si exige algún tipo de almacenamiento o lógica adicional.

**Si se recorta:** la vista previa mostraría únicamente la puntuación de cada ejemplo, sin sugerencias de mejora asociadas.

---

## 4. Desglose completo del estado Error (4 cifras)

**Qué es:** cuando una orden falla globalmente (por ejemplo, se cae la conexión con el catálogo a mitad de proceso), el detalle muestra 4 cifras distintas: SKU's procesados antes de la caída, de esos cuántos se generaron bien, cuántos fallaron por falta de atributos, y cuántos ni se llegaron a intentar.

**Qué implica técnicamente:** que el backend guarde el progreso de forma incremental durante la generación, de manera que ese desglose sobreviva a un fallo catastrófico a mitad de proceso.

**Si se recorta:** una orden en Error mostraría solo un estado genérico con botón "Reintentar", sin desglose numérico.

---

## 5. Filtro por seller + exportar informe filtrado (detalle de orden)

**Qué es:** en la tabla de SKU's sin descripción generada (dentro del detalle de una orden), poder filtrar por seller y descargar un informe ya filtrado.

**Qué implica:** relacionar cada SKU con su seller y permitir exportar un subconjunto filtrado, en vez de un único informe fijo con todos los SKU's.

**Si se recorta:** se mantendría solo el informe completo, sin filtro por seller.

---

## 6. Chevron + popover de guía en el listado de órdenes

**Qué es:** al pasar el ratón sobre el nombre de un modelo en la tabla de Descripciones, aparece una flecha pequeña que abre un popover con el nombre de la guía, su última actualización y un enlace "Ver guía" — sustituye a una columna "Guía" fija en la tabla.

**Qué implica:** es una interacción de interfaz sin dependencia de datos o lógica de backend nueva más allá de lo que ya existe para mostrar el nombre de la guía.

**Si se recorta:** se podría volver a una columna "Guía" fija y simple en la tabla, sin la interacción de hover/popover.

---

## 7. Estado "Expirada" + pestaña Expiradas

**Qué es:** las órdenes completadas cuyo CSV ha caducado pasan a un estado "Expirada": fila atenuada y no clicable, badge propio, fecha de expiración visible y una pestaña dedicada en el listado con contador.

**Qué implica técnicamente:** un TTL sobre los ficheros generados, un job (o comprobación) que marque órdenes como expiradas al vencer el plazo, un estado adicional en el ciclo de vida de la orden, y su exposición en UI (pestaña, badge, fila deshabilitada). *Punto a valorar:* la limpieza de ficheros antiguos probablemente exista de todas formas por motivos de almacenamiento — el coste recortable es la exposición del concepto como estado visible, no la limpieza en sí. Convendría que desarrollo lo confirme.

**Si se recorta:** las órdenes antiguas simplemente desaparecen del listado pasado el plazo de retención (o permanecen como completadas sin botón de descarga disponible). No hay estado "Expirada" ni pestaña dedicada.

---

## 8. Indicador "descargada / sin descargar"

**Qué es:** las órdenes completadas distinguen entre "Lista" (sin descargar, badge verde + punto rojo en la fila) y "Descargada" (badge neutro). Además, la pestaña Finalizadas muestra un punto rojo cuando hay órdenes pendientes de descargar.

**Qué implica técnicamente:** persistir el evento de descarga por orden. Con varios usuarios, además, definir la semántica: ¿descargada por cualquier usuario, o por el usuario actual? La segunda opción multiplica el coste (tracking por usuario).

**Si se recorta:** un único estado "Completada" con el botón Descargar siempre disponible. Se pierde el recordatorio visual de "te falta descargar esto".

---

## 9. Ordenación por columnas en las tablas

**Qué es:** en el listado de órdenes, las 6 columnas (orden, modelo, SKUs, estado, fecha, creador) son ordenables con indicador de dirección; en la tabla de SKU's fallidos del detalle de orden, las 4 columnas también.

**Qué implica técnicamente:** con tablas paginadas, la ordenación debe resolverse en el servidor — cada columna ordenable es un criterio más que la API debe soportar e indexar.

**Si se recorta:** orden fijo por fecha descendente (que ya es el comportamiento por defecto), sin cabeceras clicables. Opcionalmente podría conservarse la ordenación en una única columna si desarrollo confirma que el coste marginal es bajo.

---

## Resumen para revisión

| # | Funcionalidad | Depende de |
|---|---------------|------------|
| 1 | Pausar / Reanudar | Checkpoint de progreso por SKU — a confirmar solape con Borradores |
| 2 | Reutilizar orden anterior | Vínculo a orden raíz + datos de SKU's fallidos |
| 3 | Sugerencias de IA en vista previa | Llamada IA adicional — a confirmar si requiere persistencia |
| 4 | Desglose completo de Error (4 cifras) | Registro incremental de progreso |
| 5 | Filtro por seller + export | Relación SKU–seller |
| 6 | Chevron + popover de guía | Ninguna (interfaz pura) |
| 7 | Estado Expirada + pestaña | TTL de ficheros + job de expiración — a confirmar si la limpieza existe igualmente |
| 8 | Indicador descargada / sin descargar | Persistir evento de descarga (¿global o por usuario?) |
| 9 | Ordenación por columnas | Sorting server-side por criterio en tablas paginadas |

---

## Historial de revisiones

- **v3:** añadidos los puntos 7 (estado Expirada), 8 (indicador de descarga) y 9 (ordenación por columnas) tras revisión completa del prototipo HTML.
- **v2:** el antiguo punto 6 "Vínculo visible a la orden raíz" se eliminó como candidato independiente y pasó a ser nota de contexto dentro del punto 2 — nunca fue una propuesta de recorte real (ya está implementado y confirmado como coste casi nulo). El chevron + popover pasó a ser el punto 6.
