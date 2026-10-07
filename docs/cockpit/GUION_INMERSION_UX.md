# Cockpit — guion de la inmersión

> **Qué es esto.** El material de las sesiones de inmersión. La propuesta ya dice **qué hay que
> acordar** —familias, fuentes, KPIs, alertas, dueños—; esto es **lo que hay que descubrir antes**
> para poder acordarlo bien.
>
> Va en dos partes, y no son el mismo día:
> **(1)** la sesión con stakeholders, que es la de hoy, y
> **(2)** las sesiones con los eMerchants, que son las siguientes.
>
> Escrito el 2026-10-07.

---

# Parte 1 · Sesión con stakeholders

**Hoy no se entrevista a ningún usuario.** El objetivo es doble: cerrar lo que se pueda del
alcance, y sobre todo **dejar montadas las sesiones con los eMerchants**, que es lo que
desbloquea todo lo demás.

## 1 · Repasar los inputs de la propuesta

Abrir el documento de formalización y marcar, pieza por pieza, qué se cierra en la inmersión.

- **Los techos están confirmados:** 3 KPIs máximo, 3 alertas máximo, 1 agente. Son techos, no
  objetivos — arrancar con menos es mejor señal.
- **Un KPI son tres cosas: fórmula, dueño y fuente.** Cortarlo por familia o por mes no crea uno
  nuevo; cambiar cualquiera de las tres, sí.
- **Las fuentes están decididas:** BigQuery y guías eMerch. Falta **cuántas guías**, y es un
  entregable de esta fase.
- **El ámbito:** qué mundo, y cuántas familias tienen guía eMerch hoy, con número.
- **Las dos piezas invisibles:** catálogo de métricas y trazabilidad. Nadie las pide y las dos
  tienen trabajo.
- **Los cinco supuestos:** cuáles están cerrados de verdad. Datos y Negocio recortan alcance;
  Seguridad puede parar el proyecto.

→ **Salida:** qué se cierra hoy y qué queda para las siguientes sesiones.

## 2 · Mapear el demo contra el alcance

Pasar cada bloque del demo por la regla de la propuesta —**entra lo que tiene fuente**—, que ahora
se puede aplicar literal porque las fuentes están decididas:

> **¿Sale de BigQuery, de una guía eMerch o de Jira? Si no sale de ahí, no entra.**

**Aviso sobre «Catálogo y familias».** Parece la candidata natural porque se apoya en las guías,
pero el pipeline por etapas —paso 3 de 6, bloqueador, próxima acción— **no tiene fuente**: se
mantendría a mano. De esa pestaña solo tienen fuente GV Quality y ATT, que vienen de analítica.

→ **Salida:** el demo anotado, y la lista de lo que parecía entrar y no entra.

## 3 · Los Action Items de Jira — lo que un stakeholder sí puede contestar

Lo pidió Fernando y quedó para revisarse aquí. La complejidad no está en la integración, está en
qué se quiere hacer con ellos. Hoy, la parte que no necesita al usuario:

- **¿Qué es un «action item»?** ¿Cualquier ticket, o un subconjunto con alguna marca?
- ¿Qué proyectos y tableros? ¿Hay campo de mundo o de familia para poder filtrar el ámbito?
- ¿Quién los crea? ¿Los abre el eMerchant o se los abren?
- ¿Quién puede dar acceso de lectura, y cuándo?

### Esto reabre la bandeja, y de forma legítima

La bandeja de acciones se excluyó por un motivo escrito: *«requiere que alguien mantenga estado y
bloqueadores, y en este alcance no hay backoffice»*. **Si los items vienen de Jira, el backoffice
es Jira** — nadie mantiene nada de nuestro lado, y el motivo de la exclusión desaparece.

Y de paso contesta qué ve el eMerchant al abrirlo: alertas + lo que tiene pendiente + sus KPIs ya
es una razón para abrirlo cada mañana, sin construir backoffice.

### Una raya que conviene decidir

- **Leer** los items y mostrarlos → informa. No rompe nada de lo firmado.
- **Escribir** el check de completado de vuelta a Jira → la propuesta dice que el cockpit *«no
  ejecuta nada en los sistemas de origen»*. Es una excepción, pequeña pero excepción.

Escribir no es caro por lo técnico sino por la identidad: para que el ticket lo cierre **la
persona** y no un bot, hace falta autenticación por usuario.

→ **Acción:** pedir lectura de un tablero y una exportación de ~50 tickets para ver la forma real.

## 4 · Montar las sesiones con los eMerchants

**El bloque más importante de hoy.** Sin fechas no hay inmersión, y es lo único que depende
enteramente de ellos.

- **¿A cuántos entrevistamos?** Con uno no hay contraste; a partir de tres se repite. Dos o tres.
- **¿A quiénes?** Pedir explícitamente que no sean solo los entusiastas. Alguien que use poco las
  herramientas de hoy cuenta más que quien ya las domina.
- **Individuales, no en grupo.** En grupo el de más peso marca el tono y el resto asiente.
- **El manager, mejor fuera de la sala.** Cambia lo que la persona cuenta sobre lo que no mira,
  no entiende o no se cree.
- **¿Podemos verle trabajar?** Media hora mirando su pantalla vale más que una hora de preguntas.
  Si cabe, pedirlo.
- **Formato:** 60 minutos, y si se puede grabar.
- **Quién hace la presentación.** Que la pida el stakeholder, no nosotros en frío.

Y las figuras que hay que poner nombre hoy:

| Quién | Para qué |
|---|---|
| El eMerchant piloto (2-3) | Necesidades del rol y día a día |
| Su manager | Valida los KPIs y las alertas |
| Dueño de cada KPI | Sin esta figura no hay definición acordada |
| Responsable de las reglas de alerta | Sin él: umbrales fijos, no alertas con criterio |
| Alguien de datos | BigQuery: qué tablas y quién da acceso |
| IT / seguridad | Criterios por escrito y proveedor de modelo aprobado |
| Quien mantiene las guías eMerch | En qué formato están de verdad |

→ **Salida:** calendario de las siguientes sesiones, con nombres y fechas.

## 5 · Qué datos existen y cómo se disponibilizan

Por cada KPI candidato: **tabla, quién da el acceso, cada cuánto se actualiza**. Y lo mismo para
la guía eMerch —formato real, no el que suponemos— y para Jira.

→ **Salida:** mapa de fuentes con responsable y fecha de acceso por cada una.

## Las tres cosas que conviene disparar hoy

1. **Pedir los accesos** — BigQuery con datos reales, un tablero de Jira, las guías. Son las que
   tienen plazo de otro, no nuestro.
2. **Pedir los criterios de IT por escrito** y el proveedor de modelo aprobado.
3. **Poner nombre** al dueño de KPI y al responsable de las reglas de alerta.

## Qué hay que salir sabiendo hoy

- [ ] **Fechas y nombres** de las sesiones con los eMerchants.
- [ ] **Cuántas guías eMerch** hay y de qué familias, con lista.
- [ ] **Qué es un action item**, y si se lee o también se escribe.
- [ ] **Quién da cada acceso**, y cuándo.
- [ ] **Dueño de KPI y responsable de alertas**, con nombre.
- [ ] El demo anotado: qué entra, qué no y qué no se sabe.

---

# Parte 2 · Sesiones con el eMerchant

**No es la sesión de hoy.** Esto se usa cuando las sesiones que se monten en el bloque 4 tengan
fecha.

## Cómo conducirlas

**Preguntar por la semana pasada, no por el día típico.** El día típico es una construcción: sale
ordenado, completo y falso. El lunes pasado sale desordenado y verdadero.

**No enseñar el demo al principio.** Existe, lo conocen y ancla todo: a partir de ahí solo
hablarán de lo que ya vieron. Si hay que enseñarlo, al final y como pregunta: *«¿qué de esto no
usarías?»*.

**No preguntar qué quieren.** Preguntar qué hacen, qué miran, qué preguntan y qué les falló. Lo
que quieren es un resumen de lo que creen que se puede pedir.

**Perseguir el ejemplo concreto.** Cuando digan «siempre miro el CV», la siguiente es *«¿cuándo
fue la última vez y qué hiciste después?»*. Si no hay última vez, no lo miran.

## Encuadre (2 minutos)

No venimos a enseñar nada ni a validar una idea. Venimos a entender el puesto. Nada de lo que se
diga aquí compromete a que esté en la herramienta.

## El puesto y el día

- Cuéntame el lunes pasado. ¿Qué hiciste, por orden?
- ¿Qué fue lo primero que abriste al sentarte? ¿Y después?
- De una semana tuya, ¿cuánto es mirar cómo van las cosas y cuánto es hacer cosas?
- ¿Qué te interrumpe? ¿Quién te escribe y para qué?
- ¿Cuánto tiempo pasas al día buscando un dato que sabes que existe?

## Qué mira de verdad  → *los 3 KPIs*

- ¿Qué herramientas abriste la semana pasada? ¿Cuántas veces cada una?
- Si solo pudieras ver **un número** cada mañana, ¿cuál?
- **¿Qué número miras porque te lo piden, y no porque te sirva?**
- ¿Qué número has mirado este mes que te haya hecho **cambiar algo**? ¿Qué cambiaste?
- ¿Cuál de los indicadores de tu ficha no miras nunca? ¿Por qué?
- Cuando ves un número que no te gusta, ¿qué es lo siguiente que haces?

## De dónde salen y quién los discute  → *fórmula, dueño, fuente*

- ¿Has visto alguna vez dos cifras distintas del mismo indicador? ¿Qué pasó?
- Si tu manager te pregunta de dónde sale ese número, ¿a quién preguntas tú?
- ¿Hay algún número que no te acabes de creer?
- ¿Quién decide cómo se calcula? ¿Lo has acordado con alguien o viene dado?

## Qué se le escapa  → *las 3 alertas*

- La última vez que algo se te pasó y lo descubriste tarde: ¿qué fue y cómo te enteraste?
- ¿Quién te avisa hoy de los problemas? ¿Por qué canal?
- **¿Qué aviso has recibido este mes que no te servía de nada?**
- Si mañana a las ocho te llegara un aviso, ¿de qué querrías que fuera para que valiera la pena?
- Cuando te avisan de algo, ¿qué necesitas saber para decidir si es grave?

## Qué pregunta y a quién  → *el agente*

- ¿Qué le has preguntado esta semana a alguien del equipo?
- ¿Qué te preguntan a ti? ¿Siempre lo mismo?
- ¿Qué buscas en la guía eMerch? ¿Cuánto tardas en encontrarlo?
- Si pudieras preguntarle a algo que supiera de tu mundo, ¿qué le preguntarías primero?
- **¿Qué respuesta te haría desconfiar?** ¿Qué necesitarías ver para fiarte?
- ¿Y si te dijera «no lo sé»? ¿Mejor o peor que inventarse algo?

## Sus pendientes  → *los action items*

- ¿Dónde está lo que tienes que hacer? ¿Lo llevas tú o te lo llevan?
- Cuando abres un pendiente, **¿qué necesitas tener delante para decidir si lo haces ahora?**
- ¿Cuáles de tus pendientes tienen que ver con un número que miras?
- **¿Qué pendiente se te queda parado porque te falta un dato?**
- Lo que pides y no existe, ¿dónde acaba?

## El sitio en su rutina  → *qué ve al abrir el cockpit*

- ¿Qué abres cada mañana sin que nadie te lo pida? ¿Por qué justo eso?
- ¿Qué tendría que tener algo para que lo abrieras tú solo, no porque te lo manden?
- ¿En qué momento del día lo usarías? ¿Desde el escritorio, en una reunión?
- ¿Lo usarías delante de otra persona? ¿De quién?

## Su ámbito  → *qué mundo y qué familias*

- ¿De qué familias respondes? ¿Cuáles te quitan más tiempo?
- ¿Cuáles tienen guía eMerch hoy?
- ¿Trabajas solo sobre tu mundo, o te comparan con otros?

## Cierre

- **Si dentro de tres meses esto existiera y funcionara, ¿qué habrías dejado de hacer?**
- ¿Qué te haría no volver a abrirlo?
- ¿A quién más deberíamos preguntar?

## Qué hay que salir sabiendo de estas sesiones

- [ ] **Tres candidatos a KPI**, cada uno con quién responde de su definición y de qué fuente sale.
- [ ] **Qué número mira cada día** y cuál mira solo porque se lo piden — no deben pesar igual.
- [ ] **Tres situaciones reales** que merecerían aviso, contadas como ocurrieron.
- [ ] **Las cinco preguntas que más repite**, que es lo primero que el agente tiene que responder.
- [ ] **Qué le haría abrirlo por su cuenta**, sin lista de tareas.
- [ ] **Lo que pidió y hoy no existe** — por contrato se registra: es la base de la fase siguiente.
