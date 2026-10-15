# Bench — dashboards con agente

> **Qué es esto.** Lo que hacen otros productos que mezclan un panel de datos con un agente
> conversacional. **Solo observación**: aquí no se decide nada ni se dice qué encaja con lo
> nuestro — eso va en `definition/`.
>
> Fuente: board de Figma *«UX · Inmersión | POC Cockpit»*, sección **Inspiration**.
> Primera pasada el 2026-10-15; ampliado el 2026-10-16 con el recorrido de *«Profundizar con
> Copilot»*. Documento vivo: se enriquece según aparezcan referencias.

---

## Las tres referencias

| | Qué es | Por qué interesa |
|---|---|---|
| **Shopify** *(Sidekick)* | Backoffice de e-commerce | El agente **es** la home, no un añadido |
| **Connectif** *(Copilot)* | Automatización de marketing | El agente se invoca **desde el dato** y responde con gráficos |
| **Jira** *(Rovo)* | Gestión del trabajo | Paraguas con **las herramientas en el menú** y un agente transversal |

---

## A · Dónde vive el agente

- **Panel lateral que empuja.** El contenido se estrecha y recoloca — en Shopify las tarjetas
  pasan de tres columnas a dos. *(Shopify, Rovo)*
- **Panel flotante superpuesto**, que tapa parte del panel de datos. *(Connectif)*
- **Expansión a pantalla completa**, con un icono que alterna entre lateral y completo.
  *(Shopify)*
- **El menú se colapsa a un rail de iconos** cuando el agente se abre, para dejarle sitio.
  *(Shopify)*

## B · Cómo se invoca

- **Botón en la cabecera** — *«Preguntar a Rovo»*. *(Rovo)*
- **Botón flotante abajo a la derecha**, en paralelo al de cabecera. *(Rovo)*
- **El input del agente es la propia home**, bajo un saludo. *(Shopify, Connectif)*
- **El botón escribe la pregunta por ti.** Al pulsar *«Profundizar con Copilot»* no se abre un
  chat vacío: se abre una conversación **ya empezada**, con el mensaje del usuario redactado y
  acotado al bloque desde el que se pulsó — *«Quiero profundizar en el resumen de mi negocio de
  los últimos 30 días»*. *(Connectif)*
- **Desde el dato**: un botón *«Profundizar con Copilot»* junto a *«Ver dashboard»*, en la tarjeta
  de la métrica. *(Connectif)*
- **Chip de arranque** que propone la primera conversación — *«Hablar sobre mis datos»*.
  *(Connectif)*

## C · Qué se ve antes de preguntar

- **Saludo con el nombre y tres prompts sugeridos** — *«¿En qué puedo ayudarte, Miquel?»* seguido
  de tres preguntas pulsables. *(Rovo)*
- **Saludo sin panel de datos**: solo la frase y el input. *(Shopify)*
- **Saludo con pregunta abierta y chips de intención** — *«Hola Miquel, ¿qué quieres hacer hoy?»*,
  input *«Describe lo que quieres crear»*, y chips *«Analiza…»* · *«Reducir el churn»*.
  *(Connectif)*
- **Iconos de las fuentes conectadas bajo el input**, que enseñan de qué puede hablar antes de
  preguntarle. *(Rovo)*
- **Aviso de IA en el pie**, siempre visible — *«Utiliza IA. Verifica tus resultados»* ·
  *«Copilot puede cometer errores. Por favor, revisa las respuestas»*. *(Rovo, Connectif)*

## D · Cómo responde

- **Enuncia el plan antes de ejecutarlo** — *«Voy a revisar cómo va tu tienda y los pedidos
  pendientes para recomendarte qué priorizar hoy»*. *(Shopify, Connectif)*
- **Muestra los pasos dados, con check** — *«✓ Se consultaron los pedidos abiertos sin preparar y
  su cantidad total»*. *(Shopify)*
- **Estado de progreso mientras trabaja** — *«Pensando…»*, *«Analizando los datos de la
  tienda…»*, *«Trabajando en tu tienda…»*. *(los tres)*
- **Genera gráficos dentro de la conversación**, con título, rango de fechas y descripción del
  cálculo. *(Connectif)*
- **Cierra con una conclusión en lenguaje natural que incluye la cifra y la causa** — *«La tienda
  ha facturado un 28,66 % menos que en los 30 días anteriores. La caída corresponde a menos
  compras y una menor contribución atribuida al email»*. *(Connectif)*
- **Cada gráfico lleva en el subtítulo los parámetros del cálculo** — rango, comparación, zona
  horaria y filtro aplicado: *«16 ago 2026 – 14 oct 2026 · Periodo actual y 30 días anteriores;
  Europe/Madrid»*, *«Solo compras atribuidas a Connectif»*. *(Connectif)*
- **Los gráficos son interactivos** dentro de la conversación, con tooltip al pasar por encima.
  *(Connectif)*
- **La conclusión va en negrita y el matiz detrás, en redonda** — *«**La facturación cae un
  28,66 %, principalmente por el descenso de compras, no por el ticket medio.** Dentro de
  Connectif, email concentra la mayor pérdida…»*. *(Connectif)*
- **Las respuestas largas van numeradas y con titular propio** — *«2. La contribución de Connectif
  baja, especialmente en email»*. *(Connectif)*
- **Tablas dentro de la conversación**, con la comparación de periodos y la variación por fila.
  *(Connectif)*
- **El texto cita los gráficos que él mismo acaba de generar** — *«El gráfico «Importe atribuido a
  Connectif por canal» muestra dónde se concentra la diferencia»*. *(Connectif)*

## E · La home

- **Franja de KPIs en la cabecera**, con variación y sparkline por métrica. *(Shopify)*
- **Selector de periodo y de ámbito** sobre la franja — *«Últimos 30 días · Todos los canales»*.
  *(Shopify)*
- **Feed «Para ti» con pestañas** — Recomendado · Mis asignaciones (9) · Con trabajo reciente ·
  Visto. Agrupado por *Ayer* y *En la última semana*. *(Rovo)*
- **Tarjetas de oportunidad con su acción**, cada una con un botón concreto — *«Deja que Inbox
  gestione los chats por ti»* → *«Activar agente»*. *(Shopify)*
- **Espacios o secciones recomendadas** como tarjetas arriba. *(Rovo)*

## F · El menú y las herramientas

- **Las aplicaciones conectadas viven en el menú lateral**, bajo las secciones propias: Jenkins,
  Lista de verificación, Project portfolio, QMetry, Tempo, Zeplin, Cursor… *(Rovo)*
- **Agrupadas por naturaleza** — *Tus aplicaciones*, *Recommended for your team*, *Apps*,
  *Canales de ventas*. *(Rovo, Shopify)*
- **Buscador con atajo de teclado arriba del todo** — ⌘K. *(Shopify)*
- **Recientes al final del menú**, con los últimos espacios abiertos. *(Rovo)*
- **Un canal llamado «Agéntico»** como una entrada más del menú. *(Shopify)*

## G · Conversaciones

- **Historial agrupado por antigüedad** — Hoy · Ayer · Más antiguos. *(Shopify)*
- **Título generado de la propia pregunta** — *«dime que tengo que hacer hoy»*. *(Shopify)*
- **«Nuevo chat»** y lista de conversaciones dentro del panel. *(Connectif)*
- **Selector de agente en la cabecera del panel**, con desplegable. *(Rovo)*

## H · Consumo y límites, a la vista

- **Contador de créditos en la cabecera del panel** — *«1937 créditos restantes · 96,85 %»*.
  *(Connectif)*
- **El plan y su consumo, en el propio panel de datos** — *«Tu plan de este mes»*, *28 / 2000*.
  *(Connectif)*

## I · El campo de entrada

- **`@` menciona y `/` ejecuta acciones** — *«Pregunta, @menciona o usa / para llevar a cabo
  acciones»*. *(Rovo)*
- **Adjuntar y dictar** junto al botón de enviar. *(Shopify, Rovo)*
- **Selector de modo** — *«Automático»*. *(Rovo)*

---

---

## Un recorrido completo: «Profundizar con Copilot» *(Connectif)*

Lo que ocurre entre pulsar el botón de una métrica y tener la respuesta. Es el recorrido más
desarrollado de las tres referencias y merece verse entero.

**1 · El botón redacta la pregunta.** No abre un chat vacío: abre una conversación con el mensaje
del usuario ya escrito y acotado al bloque de origen.

**2 · El agente enuncia el plan.** *«Voy a revisar los últimos 30 días completos y compararlos con
los 30 anteriores para identificar qué ha cambiado en ventas y en la contribución de Connectif.»*

**3 · Genera los gráficos encadenados**, cada uno con sus parámetros de cálculo en el subtítulo.

**4 · Da la conclusión en negrita** y el matiz en redonda, debajo.

**5 · Estructura el informe largo** en apartados numerados con titular, intercalando tabla y
texto.

**6 · Explica la aritmética entre guiones** — *«Su peso sobre el importe total de compras baja del
10,89 % al 10,09 % —importe atribuido dividido entre importe total, en cada periodo—»*.

**7 · Y se autolimita, explícitamente.** Esta es la parte más llamativa:

> *«Email explica aritméticamente el 85,84 % del descenso atribuido: 422.228,51 € de los
> 491.835,01 € perdidos. **Esto no demuestra causalidad ni ventas incrementales**.»*

> *«…pero su actividad histórica no permite saber si actualmente están activos o cuál era su
> calendario.»*

> *«Con cantidades de compras similares, el importe es muy diferente: la composición de las
> reservas merece atención antes de interpretar toda la caída como menor demanda.»*

Dice el dato y, pegado, **qué no se puede concluir de él**: distingue correlación de causalidad,
declara lo que no puede saber, y avisa de cuándo un dato merece desconfianza antes de
interpretarlo.

**8 · Y el consumo se mueve a la vista.** El contador de créditos pasa de 63/2000 a 108/2000
durante la conversación.

**Un detalle del estado real:** en uno de los gráficos el tooltip muestra *«SMS · € null»*. El
valor vacío llega hasta la interfaz sin limpiar.

## Lo que piden las tres sin excepción

Tres cosas aparecen en las tres referencias, y conviene anotarlo:

1. **El agente dice lo que va a hacer antes de hacerlo.**
2. **El agente enseña que está trabajando**, con estado visible.
3. **Hay un aviso permanente de que puede equivocarse.**
