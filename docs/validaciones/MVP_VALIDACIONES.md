# Validaciones — alcance MVP

> **Estado:** propuesta en discusión, **sin decisiones cerradas con cliente**. Recoge los
> recortes planteados para una primera versión y el razonamiento de cada uno.
>
> El alcance completo está descrito en `ESTADO_MODULO_VALIDACIONES.md`; este documento solo
> dice **qué se deja fuera del MVP y por qué**.
>
> **Última actualización:** 2026-09-28

---

## El criterio

Lo que mejor defiende este MVP no es que abarate: es que **desbloquea**. Hoy el módulo no se
puede construir porque faltan la escala de criticidad y el modelo de datos del motor. Varios de
estos recortes hacen que esas respuestas dejen de ser bloqueantes.

Lo que queda después de todos ellos: *lanzas una auditoría sobre un proveedor, ves sus errores,
descartas los que no lo son, y descargas lo que hay que corregir en casa y lo que le toca a él.*
Es coherente y se sostiene solo — pero es **fase 1 de un sistema de validación, no el sistema**.
La diferencia está en el bucle de aprendizaje.

Todo esto es revisable cuando lleguen las respuestas del cliente (`PREGUNTAS_CLIENTE.md`).

---

## 1 · Fuera: gestión del motor en la herramienta

**Qué se cae.** El panel de administración "Reglas del motor de validaciones": árbol
General ▸ Familia ▸ Modelo, editor de prompt por nivel, versionado con publicar, nombrar,
historial y restaurar, y seguimiento de cambios sin publicar.

**Por qué.** Es lo más caro del módulo por lo menos que devuelve a corto plazo, y el modelo de
reglas **está a punto de rehacerse**: desarrollo quiere separar lo determinista de lo
interpretado. Construir hoy un editor para una estructura que va a cambiar es construir sobre
arena.

**Qué se pierde.** Menos de lo que parece: el bucle ya era manual. Quedó descartada la conexión
directa, así que el informe de falsos positivos iba a ser un archivo que alguien aplica a mano.
Sin panel lo aplica un desarrollador en vez de un administrador. Con dos personas tocándolo y
baja frecuencia, es asumible.

**Bonus.** Deja de bloquear la pregunta **B3** (qué nivel hay por encima del modelo), que existe
solo para organizar el árbol de ese panel.

## 2 · Fuera: la mitad de los tipos de perímetro

**Qué se queda.** **Proveedor/seller** y **gama**.

**Qué se cae.** Categoría web (PLP) y listado.

**Por qué proveedor no es negociable.** No por volumen: **toda la salida del módulo es el
informe por proveedor**. Si no puedes acotar por proveedor, auditas un perímetro mixto y luego
tienes que elegir el proveedor en el modal de exportación — que es exactamente el apaño que
tiene hoy el prototipo. Acotando por proveedor el MVP cierra el círculo: auditas a un proveedor
y le mandas lo suyo.

**Por qué gama.** Es la unidad de trabajo del validador de gama, que es el usuario principal.

**Por qué se caen esas dos.** La PLP necesita el mapeo categoría↔referencias, que sigue sin
confirmar, y arrastra el problema de que una misma PLP mezcla modelos. El listado es la vía de
escape, no el flujo principal.

**Lo que hay que decir al presentarlo.** Estos dos son precisamente los que el brief original
del cliente describía como forma de entrada (URL de PLP y listado de URLs). La vía de entrada
por selectores ya está acordada, pero conviene decir explícitamente que **la entrada pegando
URLs se deja para fase 2**, en vez de que lo descubran ellos.

**Bonus.** Deja de bloquear la pregunta **B11** (categorías web que mezclan modelos).

## 3 · Fuera: borradores

**Qué se cae.** El estado Borrador, su pestaña, el "Continuar" que reabre el funnel con el
perímetro preseleccionado, y el guardado a medias desde el funnel.

**Por qué.** El funnel de Validaciones es corto: tipo de perímetro, valor, lanzar. Guardar a
medias un formulario de dos pasos aporta poco. Es una diferencia real con Descripciones, donde
la configuración es pesada (prompt, tono, blacklist, alcance, vista previa) y ahí un borrador sí
vale.

**Lo que hay que decidir con esto.** Qué pasa al **cancelar una auditoría en curso**, que es la
otra vía por la que hoy nacen borradores. Propuesta: **la fila desaparece**. Si se mete un
estado "Cancelada" se ha cambiado un estado por otro sin ahorrar nada.

## 4 · Fuera: distribución de errores e impacto por categoría

**Qué se cae.** Las dos pestañas de analítica del informe.

**Por qué.** Son de solo lectura: no cambian lo que nadie hace. El trabajo ocurre en el listado
y en los exports. Y priorizar ya está cubierto por dos vías que se quedan: **los filtros del
listado** (sección, severidad, tipo, owner) y **el CSV**, que se pivota en Excel. Se pierde la
forma de un vistazo, no la información.

El de **impacto por categoría** además depende de datos de negocio que no tenemos — ya estaba
marcado como dependiente de Función 4 —, así que hoy son cifras inventadas en pantalla.

**Lo que se pierde.** El informe queda como Health Score + listado + exports. Es más pobre para
enseñar y el módulo se lee más como un generador de informes que como un sistema de
diagnóstico. Es un coste de relato, no de uso.

## 5 · Simplificado, no eliminado: falsos positivos

**El principio: se simplifica la mecánica, no el dato.**

Quitar el motivo no simplifica el flujo — **elimina la razón por la que existe**. El informe de
falsos positivos se convertiría en un log de borrados: nadie mejora el motor desde *«estas 340
estaban mal»*, necesita *«porque «izq» es una abreviatura aceptada»*. Y como el retrabajo del
motor va a tardar, los primeros meses de uso real son precisamente los que más materia prima
generan.

Y el **marcado en bloque** es el que no se puede quitar. La aritmética: una auditoría del orden
de 18.000 alertas con un 5% de falsos positivos son ~900 clics de uno en uno. Nadie hace eso. Lo
que pasa es que el usuario deja de marcarlos, y entonces **los informes salen al proveedor con
alertas que sabemos que están mal** — lo que destruye la confianza con el proveedor, que es lo
único que el módulo tiene que conseguir.

### Qué se captura por cada descarte

| Campo | Quién lo pone |
|---|---|
| Motivo (valor de lista cerrada) | El usuario, un clic |
| Disparador que levantó la alerta | El sistema |
| Identificador de bloque, si se marcó junto a otras | El sistema |
| Usuario y fecha | El sistema |

Fuera: comentario libre, multi-selección de motivos, ámbito editable referencia a referencia.

### La interacción

**Marcar.** Botón *Falso positivo* en la fila → **panel pequeño** (no la modal grande a dos
columnas: el diagnóstico y la corrección ya están en la fila que estás mirando), con:

1. **Motivo** — lista cerrada, selección única, un clic.
2. **Alcance** — solo si el mismo disparador afectó a más referencias, **una línea**:
   `☐ Aplicar también a las otras 47 referencias donde saltó «izq»` · *ver cuáles*
   **Desmarcada por defecto**: marcar en bloque es la acción de más impacto y debe ser
   deliberada.
3. Botón primario: *Descartar* / *Descartar 48*.

Si no hay coincidencias, la línea de alcance no aparece y el panel es motivo + botón.

**Restaurar.** Desde la pestaña de falsos positivos:
- Marcado suelto → restaura directo, sin preguntar.
- Marcado en bloque → confirmación de una línea con dos botones: *Solo esta* / *Las 48*.

**Editar.** Fuera. Si te equivocaste de motivo, restauras y vuelves a marcar: dos clics, y
elimina un flujo entero (modo edición, paso de ámbito, atrás, guardar cambios).

**Toast con Deshacer.** Se queda. Es barato y es la red de seguridad de un marcado en bloque de
48 referencias.

### Qué se cae respecto al flujo actual

- La modal de 1100px a dos columnas con el resumen del hallazgo.
- El paso 2 de *Revisar coincidencias*: árbol Familia→Modelo→Referencia, expandir/colapsar,
  filtro por seller, contadores y *Seleccionar todas*.
- El catálogo de motivos distinto por tipo de error.
- El comentario libre.
- El modo edición y el paso de ámbito de tres opciones.
- El indicador *Paso 1 de 2*.

### El riesgo que se asume, y cómo se mitiga

Con el checkbox, el usuario decide **sin ver** a qué referencias aplica. Es exactamente el
compromiso a ciegas que se rechazó en su día y que motivó el paso 2 obligatorio. Dos mitigaciones
de coste casi nulo:

1. **La línea nombra el disparador y el número**, no un genérico. Lo que comparten esas
   referencias es el disparador; si el motivo elegido aplica al disparador, aplica a todas.
2. **El enlace *ver cuáles* filtra el listado por ese disparador**, en vez de abrir una pantalla
   nueva. Reutiliza el filtro que ya existe y resuelve el *«no veo el resto»* sin construir el
   árbol.

### Lista de motivos propuesta

Una sola, cerrada y corta, igual para todos los tipos de error:

- Término válido en nuestro vocabulario
- Marca o nombre propio
- Valores equivalentes
- No aplica a esta categoría
- Otro

*Otro* no lleva campo de texto. Su frecuencia es en sí misma una señal: si se dispara, es que la
lista se queda corta y hay que ampliarla.

---

## Comparativa — qué hay hoy y qué queda

> El prototipo con los recortes aplicados está en `wireframe_validaciones_recortado.html`.
> El completo sigue en `wireframe_validaciones.html`.

### Gestión del motor

| Hoy | MVP |
|---|---|
| Pantalla propia "Reglas del motor", desde la cabecera del hub | **No existe** |
| Árbol navegable General ▸ Familia ▸ Modelo, con buscador | — |
| Editor de prompt por nodo, con aviso de cambios sin guardar | — |
| Editor de vocabulario por nodo | — |
| Versionado: publicar, nombrar, historial, restaurar | — |
| Indicador de cambios sin publicar | — |
| Las reglas las configura un administrador desde la herramienta | Las configura desarrollo, fuera de la herramienta |

**Delta: una pantalla entera con seis bloques, a cero.** Es el recorte más grande en volumen.

### Tipos de perímetro

| Hoy — 6 tipos | MVP — 2 tipos |
|---|---|
| Gama · *todas las referencias de una gama* | ✅ **Gama** |
| Proveedor / Seller · *catálogo de un proveedor o seller* | ✅ **Proveedor / Seller** |
| Modelo · *un modelo concreto* | ❌ |
| Categoría web · *pegar la URL de su PLP* | ❌ |
| Sección · *referencias de una sección de tienda física* | ❌ |
| Listado CSV · *subir un fichero con las referencias* | ❌ |

**Delta: de 6 a 2.** Cada tipo es una consulta y un paso de selección distintos, así que el
ahorro es directo y proporcional.

*Modelo es discutible: es el más barato de los cuatro que se cortan porque su selector ya existe
para otras cosas. Pero no aporta al circuito proveedor → informe.*

### Borradores

| Hoy | MVP |
|---|---|
| Estado Borrador en el modelo de datos | **No existe** |
| Pestaña "Borradores" en el hub, con contador | — |
| "Guardar y salir" desde el funnel, a medio configurar | — |
| Acción "Continuar" que reabre el funnel con el perímetro | — |
| Acción "Eliminar borrador" | — |
| Cancelar una auditoría en curso → se convierte en borrador | Cancelar → **la fila desaparece** |
| Guarda en `openInforme` para que un borrador no abra ficha | — |

**Delta: un estado, una pestaña, tres acciones y una regla de navegación.**

### Distribución e impacto

| Hoy | MVP |
|---|---|
| Dos pestañas en el informe: *Distribución de errores* e *Impacto por categoría* | **Ninguna** |
| Distribución: barras por tipo de error agrupadas en Designación, Descripción, Ficha técnica y Multimedia, con leyenda de severidad | — |
| Impacto: desglose por categoría con cifras de negocio | — |
| Informe: cifras + Health Score + **2 pestañas** + listado + exports | Informe: cifras + Health Score + listado + exports |

**Delta: dos vistas de solo lectura.** Priorizar sigue siendo posible con los filtros del listado
y con el CSV.

### Falsos positivos — marcar

| Hoy | MVP |
|---|---|
| Modal de 1100 px a dos columnas: resumen del hallazgo a la izquierda, pasos a la derecha | **Panel pequeño** sobre la fila |
| **Paso 1 · Motivo**: catálogo distinto según el tipo de error, multi-selección, comentario libre y caja informativa de coincidencias | **Motivo**: lista cerrada igual para todos, selección única, sin comentario |
| **Paso 2 · Revisar coincidencias** (obligatorio si las hay): árbol Familia→Modelo→Referencia, bloques desplegables, filtro por seller, "Seleccionar todas", contadores, enlaces a PDP | **Una línea**: `☐ Aplicar también a las otras 47 donde saltó «izq»` + enlace *ver cuáles* que filtra el listado |
| Indicador "Paso 1 de 2" y botón "← Atrás" | Sin pasos |

### Falsos positivos — editar

| Hoy | MVP |
|---|---|
| Paso de ámbito con 3 opciones: solo esta · las N del bloque · elegir cuáles | **No existe** |
| "Elegir cuáles" reabre la pantalla de selección cargada con el bloque | — |
| Modo edición del modal, con "Guardar cambios" y navegación de vuelta | — |
| Regla: editar una sola la desvincula del bloque | — |
| | Para cambiar un motivo: **restaurar y volver a marcar** |

### Falsos positivos — restaurar

| Hoy | MVP |
|---|---|
| Mismo paso de ámbito con las 3 opciones | Confirmación de una línea: *Solo esta* / *Las 48* |
| Suelta: restaura directo | Igual |
| Toast con Deshacer | Igual, se queda |

**Delta del flujo completo: de 3 pantallas a 1 panel y 1 confirmación.** Y el dato guardado pasa
de *motivos múltiples + comentario + ámbito editable* a *un motivo + disparador*.

### Re-auditar

| Hoy | MVP |
|---|---|
| Botón en la cabecera del informe | ❌ |
| Opción en el menú de la fila del hub | ❌ |
| Abre el funnel con tipo y valor de perímetro preseleccionados | Se lanza una auditoría nueva desde cero, reintroduciendo el perímetro |

**Delta: dos puntos de entrada y la lógica de preselección.** La capacidad de re-auditar no se
pierde.

### El recuento

El MVP se lleva por delante:

- **1 pantalla completa** (panel del motor, con seis bloques dentro)
- **4 de 6 tipos de perímetro**
- **1 estado** con su pestaña y tres acciones
- **2 pestañas de analítica** en el informe
- **2 de las 3 pantallas** del flujo de falsos positivos, más el modo edición
- **1 atajo** (re-auditar)

Lo que queda en pie: funnel de dos tipos → auditoría → informe con cifras, Health Score y
listado → descartar falsos positivos con motivo y en bloque → reabrir revisión si hace falta →
exportar PDF e informes.

---

## Candidatos adicionales

### Re-auditar

**Qué es.** Botón en el informe y en el menú del hub que abre el funnel de nueva auditoría con
el mismo perímetro ya preseleccionado.

**Por qué es candidato.** Es un **atajo, no una capacidad**: "Nueva auditoría" hace lo mismo con
dos clics más, y el perímetro que hay que reintroducir son dos campos. La re-auditoría como
concepto no se pierde — lo que se pierde es el preseleccionado.

---

## Lo que NO se recorta, y por qué

### El PDF del proveedor

Es el artefacto más caro que hay construido y da pereza mantenerlo en alcance, pero **es lo
único que sale de la empresa**. Sin él, el MVP es una herramienta interna de listar errores.

### Reabrir revisión — **se mantiene (decidido 2026-09-28)**

Se valoró cortarla: es una transición de estado hacia atrás (Revisada → Pendiente de revisión)
con su modal de confirmación y sus consecuencias sobre los exports ya descargados.

**Se queda porque es la única vía de corrección si alguien marca como revisada una auditoría por
error.** Sin ella ese error es irreversible y la única salida sería relanzar la auditoría
entera. El coste de construirla es menor que el de dejar un callejón sin salida en el flujo
principal.

### El Health Score — decisión pendiente, y es el mayor riesgo del MVP

No estaba en la lista de recortes y debería estar en la conversación. Está en el hub, en el
informe y en el PDF, y **depende por completo de que Leroy dé el valor por referencia** — cinco
preguntas abiertas con Javier Hernán, sin respuesta.

Si ese dato no llega, el MVP tiene tres pantallas con un número que no se puede calcular. Hay
que decidir ahora entre dos opciones, no asumir que el dato llegará:

- **Fuera del MVP**, dejando solo recuentos.
- **Dentro con degradación explícita**: si no hay dato, no se pinta.

### La escala de criticidad

No es recortable: el módulo necesita severidad para ordenar y para decidir qué llega al
proveedor. El MVP sale con nuestra propuesta de tres niveles como defecto, que es lo que ya
hace, y se ajusta cuando el cliente la cierre.
