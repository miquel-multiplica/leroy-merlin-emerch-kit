# Cockpit — lo que salió de la primera sesión

> **Sesión del 7 de octubre de 2026**, con Pedro Benavente, Susana González y Javier Hernán por
> parte de Leroy, y Enrique, Gerardo, Jordi y Miquel por la nuestra.
>
> **No era la inmersión.** Era una toma de contacto: los tres llegaban sin conocer el proyecto.
> La sesión de verdad es **el 14 de octubre**, la que convoca Fernando —*«evolución Q4 guías y
> merch + analítica»*—.
>
> Escrito a partir de la transcripción completa.

---

## Llegaron sin contexto, y conviene tenerlo presente

- **Pedro:** *«a mí me contó Fernando un poco muy por encima… de pasillo»*, y más tarde *«a mí me
  ha caído como una bomba»*.
- **Javier:** *«es la primera vez que lo veo»*.
- **Susana:** *«a mí todavía me parece ciencia ficción»*.

Nada de lo que dijeron es una validación del alcance. Es una primera reacción.

## El giro: Javier reencuadra el producto y Pedro le compra entero

Es lo más importante de la parte formal, y coincide con lo que ya decía el deck anterior
—*«no otro cuadro de mando más»*— pero dicho por quien lo va a usar.

> **Javier:** *«Ya tenemos el dashboard. No crear un dashboard bonito, unificado dentro de un
> cockpit, sino ser capaces de, con esos datos, poder dar algún tipo de activador.»*
>
> *«Lo aterrizaría más a cómo se le puede dar una vuelta a toda la cantidad de información que hay
> en un dashboard y convertirla aquí en esos accionables.»*

**Pedro:** *«totalmente de acuerdo con Javi»*.

Javier añadió también que, tal como lo veía, para final de año le parecía **«demasiado
inalcanzable»**.

### Y Pedro da el argumento del producto, sin que se lo pidan

> *«Es imposible pedir a un eMerch que de forma diaria, o incluso muchas veces semanal, revise
> todos esos reports que hay ahí. Y lo que necesitamos precisamente es eso: **alertas alimentadas
> por esos dashboards**.»*

**Esto redefine el problema.** La propuesta dice que las métricas están repartidas en herramientas
que hay que abrir una a una. Pedro dice algo distinto y más fuerte: **los dashboards ya existen;
lo que no hay es tiempo ni criterio para leerlos.**

## Los tres tableros

Pedro los nombra como *«de uso core del colectivo eMerch… el corazón del oficio»*:

1. **Search & Publication** — el «setup publication». Dice que tiene tantos apartados que *«limita
   la labor de investigación del eMerch»*.
2. **Calidad de catálogo**
3. **eMerch** — conversión, ATC y vendibilidad

*«En base a esos tres, que son los que nos den los insights para alimentar esto, centramos el
tiro.»* Gerardo le pidió que los lleve el día 14.

**Sustituyen a «BigQuery» como punto de partida**, y son mucho mejor punto de partida: tres
artefactos concretos, con dueño, que ya se usan.

## Cuatro candidatos a alerta

Los tres primeros los dieron ellos sin que nadie preguntara. El cuarto sale de lo que pasó
después de la reunión.

1. **Caída de surtido en PLP** (Pedro) — *«si has bajado más del 50% de productos en tus PLPs»*,
   cruzado con si la venta de esa familia está bajando.
2. **Atributos faltantes concentrados** (Pedro) — *«una familia tiene un porcentaje de referencias
   importantes a las que les faltan un montón de los atributos que se han seleccionado»*.
3. **Dos métricas cayendo a la vez** (Javier) — *«si vemos que el ATC está bajando y que la GV
   Quality también está bajando, que ellos puedan ver esos dos datos conjuntos y decir: perfecto,
   tengo que enfocarme en atributos»*. Es **correlación entre métricas**, que en el deck estaba en
   fase 3.
4. **Discrepancia entre fuentes para la misma referencia** — ver abajo.

## Jira: la petición no está validada por nadie

- **Pedro:** *«lo del Jira me ha dejado despistado por completo… nosotros lo utilizamos, pero lo
  utilizamos para lo que lo utilizamos»*. El proceso de creación de guías sí está ahí; operaciones
  comerciales *«entiendo que también»*; del resto, *«no tengo la info»*.
- **Gerardo lo reconoce:** *«si uno revisa hoy la propuesta, en la propuesta no figura Jira.
  Comentario de Fer para que lo evaluemos en la inmersión»*.
- **Pedro había entendido otra cosa**: que esto iría dentro de **un Power BI que ya tienen** con
  métricas de negocio e-commerce.

**Y Susana hizo la pregunta exacta** que teníamos identificada como la raya de leer vs escribir:

> *«¿Esto está pensado para que los accionables después vuelquen en las herramientas nuestras… o
> esto te va a avisar de lo que tienes que hacer y luego tú te tienes que ir al Jira a hacerlo?»*

Quedó abierta. **No conviene ir el día 14 defendiendo Jira**: las tres personas que lo usarían no
reconocen la petición.

## Lo más valioso: los 30 minutos de después

Al acabar la parte formal, Pedro, Javier y Susana se quedaron a resolver un caso real. **Es la
demostración en vivo del problema que el cockpit dice resolver**, y probablemente ni ellos se
dieron cuenta.

**El caso:** una referencia de piscinas. En STEP un atributo aparece relleno desde el **10 de
abril**; en las tablas de Javier figura como *missing* hasta el **27 de agosto**. Media hora sin
llegar a una respuesta.

Lo que salió por el camino:

- **El dato no tiene una fuente, tiene cinco.** Javier describe la cascada que usa: *«lo primero es
  el JSON, se prioriza JSON. Si no está en la herramienta de Multiplica se mira el Drive de Susana
  de 2026. Si no está en el de 2026 ni en la herramienta, se mira 2025»*. Más STEP y más Opus.
  **Esa cascada vive en su cabeza**, no está escrita en ningún sitio.
- **Hay desincronización reconocida.** Pedro: *«cómo le decimos al final que la info de Opus no
  está sincronizada con este, y que le puede penalizar»*. Javier, sobre Opus: *«es que ahí ya es
  una caja negra»*.
- **Una referencia pasó de 13 atributos a 104 de un día para otro**, el 24 de agosto, cambiando
  también designación y descripción. Susana: *«104. Imposible»*. No lo resolvieron.
- **La recarga es mensual, no diaria.** *«La siguiente recarga, que será el 5 de noviembre.»* El
  plazo de subida era el 4 de octubre y una familia entró el día 5: no cuenta hasta noviembre.

### Por qué esto importa

**Rompe un principio firmado.** La propuesta dice *«cada KPI tiene una fórmula, un dueño y una
fuente»* y *«ante una misma pregunta, el sistema devuelve el mismo resultado»*. Pero hoy el mismo
atributo está relleno o vacío según a quién preguntes y cuándo se recargó. **El determinismo no es
una decisión de diseño nuestra: es una precondición que hoy no se cumple**, y no está entre los
cinco supuestos.

**El agente hereda el problema entero.** Si le preguntas *«¿tiene atributos esta referencia?»*, su
respuesta será correcta o no según qué tabla lea. Eso convierte el *«cita de dónde sale cada
respuesta»* de requisito elegante en **la función principal**: lo que aporta no es la respuesta,
es de dónde la saca y de cuándo es.

**Y afecta al diseño de las alertas.** Con recarga mensual, una alerta de atributos puede llegar
con hasta un mes de retraso. O se acepta y se dice en pantalla, o hay que ir a otra fuente.

## Qué haría antes del 14

1. **Pedir a Pedro los tres tableros ya**, no el día 14. Son el mapa de fuentes real y ahorran
   media sesión.
2. **Llevar los cuatro candidatos a alerta escritos**, para que la sesión sea elegir y no inventar.
3. **Levantar el determinismo como supuesto**, con el caso de las piscinas delante. No como
   objeción: como *«esto que acabáis de vivir es lo que el cockpit tiene que resolver, y
   necesitamos saber cuál es la fuente buena para cada cosa»*.

Y una que no haría: **insistir con Jira hasta hablar con Fernando.**

## Otros apuntes

- **Un tablero único para todos** en este MVP (Gerardo). La configuración por persona es fase 2.
- **Para las sesiones de usuario**, Pedro apuntó que en animación comercial *«tenía que estar
  Almudena y David, o incluso algún representante de ALV»*.
