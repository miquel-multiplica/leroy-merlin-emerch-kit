# Bench — dashboards con agente

> **Qué es esto.** Lo que hacen otros productos que mezclan un panel de datos con un agente
> conversacional. **Solo observación**: aquí no se decide nada ni se dice qué encaja con lo
> nuestro — eso va en `definition/`.
>
> Fuente: board de Figma *«UX · Inmersión | POC Cockpit»*, sección **Inspiration**.
> Primera pasada el 2026-10-15. Documento vivo: se enriquece según aparezcan referencias.

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
- **El input del agente es la propia home**, bajo un saludo. *(Shopify)*
- **Desde el dato**: un botón *«Profundizar con Copilot»* junto a *«Ver dashboard»*, en la tarjeta
  de la métrica. *(Connectif)*
- **Chip de arranque** que propone la primera conversación — *«Hablar sobre mis datos»*.
  *(Connectif)*

## C · Qué se ve antes de preguntar

- **Saludo con el nombre y tres prompts sugeridos** — *«¿En qué puedo ayudarte, Miquel?»* seguido
  de tres preguntas pulsables. *(Rovo)*
- **Saludo sin panel de datos**: solo la frase y el input. *(Shopify)*
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

## Lo que piden las tres sin excepción

Tres cosas aparecen en las tres referencias, y conviene anotarlo:

1. **El agente dice lo que va a hacer antes de hacerlo.**
2. **El agente enseña que está trabajando**, con estado visible.
3. **Hay un aviso permanente de que puede equivocarse.**
