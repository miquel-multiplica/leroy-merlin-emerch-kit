# Cockpit — guion de inmersión (lado UX)

> **Qué es esto.** El guion de las sesiones con el Responsable de eMerchandising. La propuesta ya
> dice **qué hay que acordar** —familias, fuentes, KPIs, alertas, dueños—; esto es **lo que hay
> que descubrir antes** para poder acordarlo bien.
>
> No es un cuestionario a recitar. Son las preguntas que abren; el trabajo está en lo que venga
> después de cada una.
>
> Escrito el 2026-10-07, para la primera sesión.

---

## Cómo conducirlo

**Preguntar por la semana pasada, no por el día típico.** El día típico es una construcción: sale
ordenado, completo y falso. El lunes pasado sale desordenado y verdadero.

**No enseñar el demo al principio.** Existe, lo conocen y ancla todo: a partir de ahí solo
hablarán de lo que ya vieron. Si hay que enseñarlo, al final, y como pregunta: *«¿qué de esto no
usarías?»*.

**No preguntar qué quieren.** Preguntar qué hacen, qué miran, qué preguntan y qué les falló. Lo
que quieren es un resumen de lo que creen que se puede pedir.

**Perseguir el ejemplo concreto.** Cuando digan «siempre miro el CV», la pregunta siguiente es
*«¿cuándo fue la última vez y qué hiciste después?»*. Si no hay última vez, no lo miran.

---

---

# Agenda de la primera sesión (7 de octubre)

Cinco bloques. El guion de abajo es para las sesiones **con el eMerchant**; esto es para la
sesión de arranque, que es otra cosa: poner el alcance sobre la mesa y disparar accesos.

## 1 · Repasar los inputs de la propuesta

Abrir el Word y marcar, pieza por pieza, qué se cierra en la inmersión. Lo que hay que tocar:

- **«3 KPIs»** — ¿tres definiciones con sus cortes, o tres números? (Un corte es la misma fórmula
  con otro filtro; si cambia la fórmula, el dueño o la fuente, es un KPI nuevo.)
- **«3 tipos de alerta»** — ¿tres reglas distintas o tres niveles de una?
- **El ámbito** — qué mundo, y cuántas familias tienen guía eMerch **hoy**, con número.
- **Las dos piezas invisibles** — catálogo de métricas y trazabilidad. Nadie las pide y las dos
  tienen trabajo.
- **Los cinco supuestos** — cuáles están cerrados de verdad. Datos y Negocio recortan alcance;
  Seguridad puede parar el proyecto.

→ **Salida:** qué se cierra hoy y qué queda para las siguientes sesiones.

## 2 · Mapear el demo contra el alcance

Pasar cada bloque del demo por la regla de la propuesta —**entra lo que tiene fuente**— y
marcarlo: entra / no entra / no se sabe.

**Ojo con «Catálogo y familias».** Parece la candidata natural porque se apoya en las guías, pero
el deck anterior la excluye explícitamente: *«widget de seguimiento de familias: las etapas y el
avance se mantienen a mano»*. El pipeline por etapas **no tiene fuente**. Lo que sí la tiene de
esa pestaña son GV Quality y ATT, que vienen de analítica.

→ **Salida:** el demo anotado, y la lista de lo que parecía entrar y no entra.

## 3 · Cómo entran las cosas: Jira y las fuentes del día a día

El bloque que más nos falta entender. Qué preguntar:

- ¿Qué le llega al eMerchant por Jira y qué no? ¿Quién abre los tickets?
- ¿Los lee, o se entera por otro lado y el ticket es el registro a posteriori?
- ¿Qué tableros y proyectos? ¿Hay campo de mundo o de familia para poder filtrar su ámbito?
- Lo que se pide y hoy no existe, **¿dónde se anota?** Por contrato hay que registrarlo.

**Por qué Jira y no otra cosa:** el deck lo pone como *«rastro del colaborador»* junto a correo y
Drive, pero en la mitigación legal dice que si no hay base legal *«arrancamos solo con Jira, Drive
de equipo y calendario»*. Es la fuente menos problemática de las tres, y la única del grupo que se
puede pedir hoy sin abrir un tema legal.

→ **Salida:** si Jira es bandeja de entrada, backlog o las dos, y si sirve para filtrar por ámbito.
→ **Acción:** pedir lectura de un tablero y una exportación de ~50 tickets para ver la forma real.

## 4 · Con quién hay que hablar

Cerrar **nombres y fechas hoy**, no «ya lo vemos»:

| Quién | Para qué |
|---|---|
| El eMerchant piloto | Necesidades del rol y día a día. Es el guion de abajo. |
| Su manager | Valida los KPIs y las alertas. |
| Dueño de cada KPI | Sin esta figura no hay definición acordada. |
| Responsable de las reglas de alerta | Sin él: umbrales fijos, no alertas con criterio. |
| Alguien de datos | BigQuery: qué tablas y quién da acceso. |
| IT / seguridad | Criterios por escrito y proveedor de modelo aprobado. |
| Quien mantiene las guías eMerch | En qué formato están de verdad. |

→ **Salida:** calendario de las siguientes sesiones.

## 5 · Qué datos existen y cómo se disponibilizan

Por cada KPI candidato: **tabla, quién da el acceso, cada cuánto se actualiza**. Y lo mismo para
la guía eMerch (formato real, no el que suponemos) y para Jira.

→ **Salida:** mapa de fuentes con responsable y fecha de acceso por cada una.

## Las tres cosas que conviene disparar hoy

1. **Pedir los accesos** — BigQuery con datos reales, un tablero de Jira, las guías. Son las que
   tienen plazo de otro, no nuestro.
2. **Pedir los criterios de IT por escrito** y el proveedor de modelo aprobado.
3. **Poner nombre** al dueño de KPI y al responsable de alertas.

---

# Guion de las sesiones con el eMerchant

## 0 · Encuadre (2 minutos)

No venimos a enseñar nada ni a validar una idea. Venimos a entender el puesto. Nada de lo que se
diga aquí compromete a que esté en la herramienta.

## 1 · El puesto y el día

- Cuéntame el lunes pasado. ¿Qué hiciste, por orden?
- ¿Qué fue lo primero que abriste al sentarte? ¿Y después?
- De una semana tuya, ¿cuánto es mirar cómo van las cosas y cuánto es hacer cosas?
- ¿Qué te interrumpe? ¿Quién te escribe y para qué?
- ¿Qué parte de tu trabajo crees que nadie de fuera entiende?

## 2 · Qué mira de verdad  → *los 3 KPIs*

- ¿Qué herramientas abriste la semana pasada? ¿Cuántas veces cada una?
- Si solo pudieras ver **un número** cada mañana, ¿cuál?
- **¿Qué número miras porque te lo piden, y no porque te sirva?**
- ¿Qué número has mirado este mes que te haya hecho **cambiar algo**? ¿Qué cambiaste?
- ¿Cuál de los indicadores de tu ficha no miras nunca? ¿Por qué?
- Cuando ves un número que no te gusta, ¿qué es lo siguiente que haces?
- ¿Cada cuánto lo miras? ¿Diario, semanal, cuando te preguntan?

## 3 · De dónde salen y quién los discute  → *fórmula, dueño, fuente*

- ¿Has visto alguna vez dos cifras distintas del mismo indicador? ¿Qué pasó?
- Si tu manager te pregunta de dónde sale ese número, ¿a quién preguntas tú?
- ¿Hay algún número que no te acabes de creer?
- ¿Quién decide cómo se calcula? ¿Lo has acordado con alguien o viene dado?
- ¿Alguna vez has tenido que rehacer un cálculo a mano porque la herramienta no lo daba así?

## 4 · Qué se le escapa  → *los 3 tipos de alerta*

- La última vez que algo se te pasó y lo descubriste tarde: ¿qué fue y cómo te enteraste?
- ¿Quién te avisa hoy de los problemas? ¿Por qué canal?
- **¿Qué aviso has recibido este mes que no te servía de nada?**
- Si mañana a las ocho te llegara un aviso, ¿de qué querría que fuera para que valiera la pena?
- Cuando te avisan de algo, ¿qué necesitas saber para decidir si es grave?
- ¿Qué tendría que pasar para que dejaras de hacer caso a los avisos?

## 5 · Qué pregunta y a quién  → *el agente*

- ¿Qué le has preguntado esta semana a alguien del equipo?
- ¿Qué te preguntan a ti? ¿Siempre lo mismo?
- ¿Qué buscas en la guía eMerch? ¿Cuánto tardas en encontrarlo?
- ¿Hay algo que preguntas siempre y siempre te cuesta?
- Si pudieras preguntarle a algo que supiera de tu mundo, ¿qué le preguntarías primero?
- **¿Qué respuesta te haría desconfiar?** ¿Qué necesitarías ver para fiarte?
- ¿Y si te dijera «no lo sé»? ¿Mejor o peor que inventarse algo?

## 6 · El sitio en su rutina  → *qué ve al abrir el cockpit*

Esta parte alimenta la pregunta que tenemos abierta: si no hay bandeja de tareas, qué hace que
alguien abra esto por su cuenta.

- ¿Qué abres cada mañana sin que nadie te lo pida? ¿Por qué justo eso?
- ¿Qué tendría que tener algo para que lo abrieras tú solo, no porque te lo manden?
- **Si esto existiera y no tuviera una lista de tareas, ¿para qué lo abrirías?**
- ¿En qué momento del día lo usarías? ¿Desde el escritorio, en una reunión, de camino?
- ¿Lo usarías delante de otra persona? ¿De quién?

## 7 · Su ámbito  → *qué mundo y qué familias*

- ¿De qué familias respondes? ¿Cuáles te quitan más tiempo?
- ¿Cuáles tienen guía eMerch hoy?
- ¿Trabajas solo sobre tu mundo, o te comparan con otros?
- ¿Hay alguna familia que mirarías cada día si pudieras?

## 8 · Cierre

- **Si dentro de tres meses esto existiera y funcionara, ¿qué habrías dejado de hacer?**
- ¿Qué te haría no volver a abrirlo?
- ¿A quién más deberíamos preguntar?

---

## Qué hay que salir sabiendo

Al cerrar las sesiones tenemos que poder escribir, sin inventar:

- [ ] **Tres candidatos a KPI**, cada uno con quién responde de su definición y de qué fuente sale.
- [ ] **Qué número mira cada día** y cuál mira solo porque se lo piden — no es lo mismo y no deben
      pesar igual.
- [ ] **Tres situaciones reales** que merecerían aviso, contadas como ocurrieron.
- [ ] **Las cinco preguntas que más repite**, que es lo primero que el agente tiene que saber responder.
- [ ] **Qué le haría abrirlo por su cuenta**, sin lista de tareas.
- [ ] **Qué familias de su ámbito tienen guía** hoy, con número.
- [ ] **Lo que pidió y hoy no existe** — por contrato esto se registra: es la base de la fase siguiente.

## Lo que no haría en estas sesiones

- Enseñar el demo al principio.
- Preguntar «¿qué KPIs quieres?» — la respuesta será la lista de su ficha, que ya tenemos.
- Prometer. Todo lo que se diga aquí entra solo si tiene fuente.
- Cerrar el ámbito en la primera sesión. Hay más de una; la primera es para entender.
