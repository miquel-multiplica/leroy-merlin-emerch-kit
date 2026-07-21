# Descripciones — Generación masiva de descripciones de producto

> Especificación en formato **OpenSpec** (Purpose + Requirements con `SHALL` + Scenarios en `WHEN/THEN`).
> Refleja el **estado actual del prototipo** (`wireframe_descripciones.html`) **sin aplicar los recortes** propuestos en `PROPUESTA_RECORTES_DESCRIPCIONES.md` — todas las funcionalidades se documentan como vigentes.
> Los **gaps de experiencia** detectados durante la redacción están al final, en un apéndice **no normativo**.

## Purpose

El módulo **Descripciones** permite a los equipos de contenido de Leroy Merlin generar de forma **masiva** las descripciones de producto de un modelo, cruzando tres insumos:

1. Una **guía de estilo publicada** (plantillas de designación/descripción, atributos, ejemplos).
2. Los **valores reales de atributos por SKU** (subidos vía CSV — puente manual hasta que exista integración directa con catálogo/Mirakl).
3. Un **prompt personalizable** (tono, longitud, blacklist, instrucciones).

El resultado de una ejecución es una **orden** cuyo output es un **CSV listo para importar en Mirakl**. El módulo cubre el ciclo completo: iniciar una orden, acotar el alcance de SKUs, configurar y previsualizar, lanzar la generación, seguir su progreso, consultar el detalle y descargar el resultado, además de reintentar los SKUs fallidos y reutilizar la configuración de órdenes anteriores.

**Fuera del alcance de esta spec** (viven en el mismo prototipo pero pertenecen a otras capacidades): la construcción de guías (`s10` Guías eMerch, `s-guia-desc*`), la gestión de usuarios (`s-users`, invitación, activación) y la validación de CSV de cliente contra la guía.

---

## Requirements

### Requirement: Hub de órdenes como punto de entrada

El sistema SHALL presentar el listado de órdenes de generación de descripciones como pantalla principal del módulo, desde la cual se crean nuevas órdenes y se consulta el estado de las existentes.

#### Scenario: Entrada con órdenes existentes
- WHEN el usuario abre el módulo Descripciones
- THEN se muestra la tabla de órdenes con la pestaña "Todo" activa
- AND se ofrece el botón "Nueva orden"

#### Scenario: Entrada sin ninguna orden
- WHEN el usuario abre el módulo y no existe ninguna orden
- THEN se muestra un estado vacío con el texto "No tienes ninguna orden de generación de descripciones" y una llamada a "Crea tu primera orden"

### Requirement: Pestañas y contadores del listado

El sistema SHALL segmentar las órdenes en las pestañas "Todo", "En curso", "Finalizadas", "Borradores" y "Expiradas", cada una mostrando solo las órdenes de su estado y, cuando aplique, un contador.

#### Scenario: Contador de órdenes en curso
- WHEN existen órdenes activas
- THEN la pestaña "En curso" muestra un contador con el número de órdenes de ese grupo

#### Scenario: Aviso de descargas pendientes en Finalizadas
- WHEN existe al menos una orden completada que aún no se ha descargado
- THEN la pestaña "Finalizadas" muestra un punto rojo con el título "Hay órdenes sin descargar"

#### Scenario: Contadores de borradores y expiradas
- WHEN existen borradores u órdenes expiradas
- THEN las pestañas "Borradores" y "Expiradas" muestran su recuento respectivo

### Requirement: Búsqueda de órdenes

El sistema SHALL permitir filtrar el listado por texto libre sobre el nombre del modelo, la guía y el código de orden.

#### Scenario: Búsqueda con coincidencias
- WHEN el usuario escribe en el buscador ("Buscar por modelo, guía o código")
- THEN la tabla muestra solo las órdenes cuyo modelo, guía o código coincidan
- AND se ofrece un control para limpiar la búsqueda

### Requirement: Ordenación por columnas del listado

El sistema SHALL permitir ordenar el listado de órdenes por las columnas Orden, Modelo, SKUs, Estado, Fecha y Creador, mostrando la dirección de orden activa, con orden por defecto por Fecha descendente.

#### Scenario: Ordenar por una columna
- WHEN el usuario pulsa la cabecera de una columna ordenable
- THEN la tabla se reordena por esa columna
- AND la cabecera muestra un indicador de dirección (ascendente/descendente)

#### Scenario: Orden por estado
- WHEN el usuario ordena por la columna Estado
- THEN las órdenes se ordenan por prioridad de estado (error, en curso, completada, expirada)

### Requirement: Acciones por fila según estado

El sistema SHALL ofrecer en cada fila las acciones pertinentes a su estado, además de un menú con "Ver detalle" (cuando el detalle esté disponible) y "Eliminar orden".

#### Scenario: Acciones de una orden en curso
- WHEN la orden está en curso
- THEN la fila ofrece "Pausar"
- AND al pausar se ofrecen "Reanudar" y "Cancelar"

#### Scenario: Acciones de una orden completada
- WHEN la orden está completada
- THEN la fila ofrece "Descargar CSV" y "Nueva orden"

#### Scenario: Acciones de una orden en error
- WHEN la orden está en error
- THEN la fila ofrece "Reintentar"

#### Scenario: Acciones de una orden expirada
- WHEN la orden está expirada
- THEN la fila ofrece únicamente "Nueva orden"
- AND la opción "Ver detalle" no está disponible

### Requirement: Indicador de descarga pendiente

El sistema SHALL distinguir visualmente las órdenes completadas descargadas de las no descargadas, y marcar las no descargadas como pendientes de descarga.

#### Scenario: Orden completada sin descargar
- WHEN una orden está completada y aún no se ha descargado
- THEN muestra el badge "Lista" y un indicador de "Sin descargar"

#### Scenario: Orden completada ya descargada
- WHEN se descarga el CSV de una orden completada
- THEN su badge pasa a "Descargada" y desaparece el indicador de descarga pendiente

### Requirement: Popover de guía en la columna Modelo

El sistema SHALL permitir consultar la guía asociada a una orden desde la columna Modelo, sin ocupar una columna fija, mediante un chevron que abre un popover.

#### Scenario: Consultar la guía desde el listado
- WHEN el usuario despliega el chevron junto al nombre del modelo
- THEN se muestra el nombre de la guía, su última actualización y un enlace "Ver guía"
- AND "Ver guía" abre la vista de guía en modo solo lectura

### Requirement: Distinción visual de reintentos con orden raíz

El sistema SHALL identificar las órdenes que son reintento de otra, mostrando junto a su código una referencia a la orden raíz de la que derivan.

#### Scenario: Orden que es reintento
- WHEN una orden es reintento de otra
- THEN junto a su código aparece un icono con un tooltip que indica la orden raíz (ej. "Reintento de la orden #1020: generada solo para los SKU's que fallaron.")

### Requirement: Listado de borradores

El sistema SHALL mantener los borradores en un listado propio con su modelo, número de SKUs, última edición y guía, permitiendo continuarlos o eliminarlos.

#### Scenario: Continuar un borrador
- WHEN el usuario pulsa "Continuar" en un borrador
- THEN se reanuda el funnel de configuración con la configuración guardada del borrador

#### Scenario: Sin borradores
- WHEN no existen borradores
- THEN se muestra el estado vacío "No tienes borradores guardados"

---

### Requirement: Estados y ciclo de vida de una orden

El sistema SHALL modelar cada orden con uno de los estados: **En curso**, **Error**, **Completada** (subdividida en "Lista" y "Descargada"), **Expirada**, además de los estados operativos **Pausado** (transitorio de En curso) y **Borrador**.

#### Scenario: Badge según estado
- WHEN se muestra una orden en cualquier listado o en su detalle
- THEN su badge refleja su estado con un texto y color consistentes (En curso, Error, Lista, Descargada, Expirada, Pausado, Borrador)

### Requirement: Cancelar una generación la convierte en borrador

El sistema SHALL permitir cancelar una orden en curso o pausada, recuperándola después como **borrador** con su configuración (guía/modelo/prompt/alcance) — no con el progreso ya generado.

#### Scenario: Cancelar una orden en curso
- WHEN el usuario cancela una orden en curso y confirma la cancelación
- THEN la orden desaparece del listado de órdenes activas
- AND aparece en el listado de borradores conservando su configuración
- AND el progreso ya generado no se conserva

### Requirement: Pausar y reanudar la generación

El sistema SHALL permitir pausar una generación en curso y reanudarla más adelante desde el mismo punto, sin repetir el trabajo ya realizado.

#### Scenario: Pausar y reanudar
- WHEN el usuario pausa una orden en curso
- THEN la orden pasa a estado Pausado conservando su porcentaje de progreso
- AND se ofrece "Reanudar" para continuar y "Cancelar" para convertirla en borrador

### Requirement: Expiración de órdenes completadas

El sistema SHALL marcar como **Expiradas** las órdenes completadas cuyo CSV ha caducado, dejándolas no accionables salvo para crear una nueva orden a partir de ellas.

#### Scenario: Orden caducada
- WHEN el CSV de una orden completada supera su plazo de retención
- THEN la orden pasa a estado Expirada, con su fecha de expiración visible
- AND deja de ofrecer descarga y detalle, ofreciendo solo "Nueva orden"

---

### Requirement: Iniciar una orden seleccionando guía y modelo

El sistema SHALL iniciar el funnel de una nueva orden pidiendo primero elegir una guía publicada y después un modelo de esa guía.

#### Scenario: Selección de guía
- WHEN el usuario pulsa "Nueva orden"
- THEN se muestra la lista de guías publicadas con un buscador
- AND al seleccionar una guía se avanza a la selección de modelo

#### Scenario: Selección de modelo
- WHEN el usuario selecciona un modelo de la guía
- THEN se avanza al paso de selección de alcance de SKUs
- AND se ofrece un enlace "Cambiar" para volver a elegir guía

### Requirement: Selección del alcance de SKUs

El sistema SHALL permitir acotar sobre qué SKUs del modelo se generará: **todos los SKUs del modelo**, **solo los SKUs de un CSV subido**, o **solo los SKUs que fallaron** en una orden anterior (esta última opción solo al reutilizar una orden).

#### Scenario: Todos los SKUs del modelo
- WHEN el usuario elige "Todos los SKU's del modelo"
- THEN el sistema consulta el catálogo y muestra el número de SKUs detectados
- AND habilita continuar a la configuración

#### Scenario: Subir selección por CSV
- WHEN el usuario elige "Subir selección (CSV)" y sube un fichero
- THEN el sistema valida que los SKUs pertenecen al modelo antes de aceptarlo
- AND muestra el número de SKUs detectados del CSV

#### Scenario: Solo los SKUs fallidos
- WHEN se reutiliza una orden anterior con SKUs fallidos y el usuario elige "Solo los SKU's que quedaron sin generar"
- THEN el alcance queda fijado al número de SKUs que fallaron en la orden anterior

### Requirement: Validación del CSV de SKUs

El sistema SHALL validar el CSV de selección de SKUs y rechazarlo cuando contenga SKUs que no pertenecen al modelo, informando del motivo.

#### Scenario: CSV con SKUs de otro modelo
- WHEN el CSV subido contiene SKUs que no corresponden al modelo
- THEN se muestra un error indicando cuántos SKUs no corresponden y que deben revisarse
- AND no se permite continuar hasta subir un CSV válido

#### Scenario: CSV válido
- WHEN el CSV subido solo contiene SKUs del modelo
- THEN se acepta y se muestra el número de SKUs detectados, con opción de "Subir otro CSV"

### Requirement: Reutilizar la configuración de una orden anterior

El sistema SHALL permitir crear una nueva orden a partir de una orden ya finalizada, reutilizando su guía/modelo/prompt e informando de ello, con la opción de acotar el alcance a los SKUs que fallaron.

#### Scenario: Nueva orden a partir de existente
- WHEN el usuario pulsa "Nueva orden" sobre una orden completada o expirada
- THEN se abre el funnel con la configuración reutilizada
- AND se muestra un banner que informa de que la configuración proviene de la orden anterior y puede editarse
- AND se ofrece la opción de generar solo para los SKUs que fallaron

### Requirement: Configuración del prompt de generación

El sistema SHALL permitir configurar la generación mediante tono, longitud (mínimo/máximo de caracteres), una blacklist de palabras prohibidas y un prompt de instrucciones adicionales.

#### Scenario: Configurar tono y longitud
- WHEN el usuario está en la configuración
- THEN puede elegir un tono entre las opciones disponibles (Informativo y neutro, Persuasivo y comercial, Técnico y preciso, Cercano y conversacional, Premium y aspiracional)
- AND puede fijar la longitud mínima y máxima en caracteres

#### Scenario: Añadir palabras a la blacklist
- WHEN el usuario introduce una o varias palabras (separadas por `;` o `,`) y las añade
- THEN cada palabra se muestra como una etiqueta eliminable que no deberá aparecer en las descripciones generadas

### Requirement: Vista previa con puntuación de calidad

El sistema SHALL permitir generar una vista previa de ejemplos antes de la generación masiva, mostrando una puntuación de calidad por ejemplo y sugerencias de mejora para los ejemplos con puntuación baja; la generación masiva SHALL habilitarse solo tras ejecutar al menos una vista previa.

#### Scenario: Generar vista previa
- WHEN el usuario pulsa "Generar vista previa"
- THEN se muestran ejemplos de descripción con su puntuación de calidad
- AND el botón cambia a "Regenerar vista previa"
- AND se habilita el botón "Generar descripciones"

#### Scenario: Sugerencias en ejemplos de baja puntuación
- WHEN un ejemplo de la vista previa tiene una puntuación baja
- THEN se muestran sugerencias concretas de mejora asociadas a ese ejemplo

#### Scenario: Generación bloqueada sin vista previa
- WHEN el usuario no ha ejecutado ninguna vista previa
- THEN el botón "Generar descripciones" permanece deshabilitado

### Requirement: Cambiar guía o modelo desde el wizard

El sistema SHALL permitir corregir la guía o el modelo elegidos sin abandonar el funnel.

#### Scenario: Cambiar guía y/o modelo
- WHEN el usuario abre "Cambiar guía o modelo" desde el wizard
- THEN puede elegir cambiar guía y modelo, cambiar solo el modelo, o descartar y seguir donde estaba

### Requirement: Guardar borrador al salir del wizard

El sistema SHALL ofrecer guardar el trabajo en curso como borrador al intentar salir del funnel.

#### Scenario: Salir con cambios sin generar
- WHEN el usuario intenta salir del wizard
- THEN se ofrece "Guardar borrador" o "Descartar cambios"
- AND al guardar, el borrador aparece en el listado de borradores para retomarlo después

---

### Requirement: Ejecutar la generación masiva con seguimiento de progreso

El sistema SHALL ejecutar la generación en una orden con progreso observable (SKUs procesados, porcentaje, descripciones generadas y sin generar), permitiendo seguirla en primer plano o dejarla en segundo plano.

#### Scenario: Lanzar la generación
- WHEN el usuario pulsa "Generar descripciones" tras la vista previa
- THEN se inicia la generación mostrando el progreso (contador y barra)
- AND se ofrece "Ver proceso en detalle" y "Cerrar y continuar en segundo plano"

#### Scenario: Continuar en segundo plano
- WHEN el usuario elige continuar en segundo plano
- THEN vuelve al listado de órdenes, donde la orden aparece "En curso"

#### Scenario: Generación completada
- WHEN la generación termina
- THEN se ofrece "Descargar CSV", "Ver detalles" y "Salir"

### Requirement: Confirmar la cancelación durante la generación

El sistema SHALL pedir confirmación antes de cancelar una generación en curso.

#### Scenario: Confirmación de cancelación
- WHEN el usuario cancela una generación en curso
- THEN se muestra una confirmación ("¿Cancelar la generación?") con "No, continuar generando" y "Sí, cancelar generación"

---

### Requirement: Detalle de orden según su estado

El sistema SHALL mostrar el detalle de una orden con un bloque de progreso acorde a su estado (en curso, completada, error, borrador), además de la tabla de SKUs sin descripción.

#### Scenario: Detalle en curso
- WHEN se abre el detalle de una orden en curso
- THEN se muestran SKUs procesados, porcentaje y el desglose de descripciones generadas y sin generar
- AND se ofrecen "Pausar" y "Cancelar"

#### Scenario: Detalle completado
- WHEN se abre el detalle de una orden completada
- THEN se muestran el total de SKUs procesados, generadas y sin generar
- AND se ofrecen "Nueva orden" y "Descargar CSV"

#### Scenario: Detalle de borrador
- WHEN se abre el detalle de una orden guardada como borrador
- THEN se informa de que se canceló y se guardó como borrador
- AND se ofrece "Continuar"

### Requirement: Desglose completo del estado Error

El sistema SHALL, para una orden que falla globalmente a mitad de proceso, mostrar el desglose de cuatro cifras — SKUs procesados antes del fallo, de esos cuántos se generaron, cuántos fallaron por falta de atributos y cuántos quedaron pendientes por el fallo — junto a una explicación del motivo, conservando las descripciones ya generadas.

#### Scenario: Orden en error por fallo global
- WHEN una orden falla globalmente (p. ej. se pierde la conexión con el catálogo a mitad de proceso)
- THEN el detalle muestra las cuatro cifras (procesados, generados, sin generar por atributos, pendientes por el fallo)
- AND muestra un banner explicando que no se pudo continuar y que las descripciones ya generadas no se han perdido
- AND ofrece "Reintentar"

### Requirement: Tabla de SKUs sin descripción con filtro por seller y export

El sistema SHALL listar, en el detalle de una orden, los SKUs para los que no se generó descripción, con su producto, seller y atributos faltantes, permitiendo ordenar, filtrar por seller y descargar un informe filtrado.

#### Scenario: Consultar SKUs sin descripción
- WHEN se abre el detalle de una orden con SKUs fallidos
- THEN se listan los SKUs afectados indicando el seller y los atributos que faltan en el catálogo
- AND la tabla es ordenable por sus columnas

#### Scenario: Filtrar por seller y exportar
- WHEN el usuario selecciona un seller en el filtro
- THEN la tabla muestra solo los SKUs de ese seller
- AND "Descargar informe" exporta el subconjunto filtrado

### Requirement: Reintentar desde el detalle acotando a los pendientes

El sistema SHALL permitir reintentar una orden en error desde su detalle, regenerando únicamente los SKUs pendientes del fallo y encadenando el reintento a la orden raíz original.

#### Scenario: Reintentar una orden en error
- WHEN el usuario pulsa "Reintentar" en el detalle de una orden en error
- THEN se reanuda la generación sobre los SKUs pendientes del fallo
- AND el reintento queda encadenado a la orden raíz original (evitando cadenas indefinidas de reintentos)

---

## Apéndice — Gaps de experiencia detectados (NO normativo)

> Estos puntos surgieron al contrastar la spec con el prototipo. **No forman parte de los requisitos**: son decisiones de producto pendientes o incoherencias del prototipo que conviene resolver antes de construir. No confundir con los *recortes* (que son candidatos a eliminar funcionalidad); aquí se asume que todo se mantiene.

### A. Gaps de comportamiento (requieren decisión de producto)

1. **Estado "Cancelada" inexistente.** Cancelar convierte la orden en **Borrador** y la saca del listado; no hay un estado "Cancelada" persistente. El copy de los modales habla de "cancelar la generación", lo que puede leerse como un estado final. → Confirmar que el resultado esperado es siempre borrador, y alinear el copy.
2. **Cancelar durante el wizard no cancela nada.** Si aún no se ha lanzado la generación, "Sí, cancelar generación" solo cierra el modal y vuelve a configuración, pese a que el texto advierte de que "las descripciones generadas se perderán". → Definir el comportamiento y el copy para el caso "aún no hay nada que cancelar".
3. **"Reintentar" desde el listado regenera TODO, no solo los fallidos.** Desde la tabla, reintentar abre la generación completa; solo el detalle acota a los pendientes. Conceptualmente un reintento debería regenerar solo los SKUs que fallaron. → Unificar la semántica de reintento entre listado y detalle.
4. **Órdenes en Error mezcladas en "En curso".** No tienen pestaña propia; el filtro "En curso" las incluye, y el contador visible no cuadra con el cálculo real. → Decidir si Error merece su propia pestaña/contador y corregir el recuento.
5. **Expiradas sin explicación ni salida de valor.** Las filas expiradas no son clicables, no tienen "Ver detalle" ni descarga, y no se explica por qué. → Definir qué información conserva una orden expirada y cómo se comunica al usuario (p. ej. tooltip "Caducó el · CSV ya no disponible").
6. **Tabla de SKUs sin descripción visible también sin errores.** El detalle muestra la tabla de fallidos incluso en órdenes completadas sin errores. → Ocultarla o mostrar un estado "sin fallos" cuando `errores = 0`.
7. **Semántica de "Descargada" con varios usuarios.** No está definido si "Descargada" significa descargada por *cualquier* usuario o por el *usuario actual*. → Decidir el alcance del indicador (afecta al coste; ver punto 8 del doc de recortes).

### B. Incoherencias de datos del prototipo (a resolver al conectar datos reales)

8. **Contexto de guía/modelo hardcodeado.** La guía elegida no se propaga; a partir de `s2` todo muestra siempre "Aplique de pared industrial · MOD_000000 · Lámparas de techo" independientemente de la selección. Los metadatos de la guía (sección/quarter) difieren entre pantallas.
9. **Detalle de orden hardcodeado.** El detalle muestra siempre el mismo título ("#1001"), meta y cifras, sin variar según la orden abierta.
10. **Órdenes raíz inexistentes.** Los tooltips de reintento apuntan a órdenes raíz (#1020, #1021) que no existen en el listado (que solo llega a #1018).

### C. Andamiaje de prototipo (demo-only, no son requisitos)

11. Botones de demostración: "Toggle vacío/lleno" (listado), "Vista activación cuenta" (usuarios) y los "Volver a…" de las pantallas de onboarding.
12. Acciones simuladas con `alert(...)`: "Descargar CSV", "Descargar informe".
13. Pantallas de flujo `s7`/`s8` declaradas pero inexistentes (la generación ocurre en un modal), y `edit-modal` referenciado pero inexistente. Referencias muertas del prototipo, sin impacto funcional.
14. Columna "Guía" contemplada como ordenable pero sin cabecera propia (la guía vive en el popover de "Modelo").
