# Cockpit — la propuesta firmada

> **Qué es esto.** El volcado de *«Formalización - eMerchant Cockpit»* (septiembre 2026), el
> documento que define la colaboración. Es **la referencia de alcance**: cuando el demo, el
> mapeo de Figma o una conversación digan otra cosa, manda este documento.
>
> Original en Drive: `Formalizacion - eMerchant Cookpit.docx`.
> Volcado el 2026-10-07. No se edita para cambiar el alcance — si el alcance cambia, se anota
> en `ESTADO_MODULO_COCKPIT.md` con la fecha y quién lo decidió.

---

## Lo que se compromete

Una primera versión del eMerchant Cockpit, **acotada y en producción antes del cierre de 2026**.

El criterio que lo ordena: avanzar por pasos, demostrar valor con un caso concreto antes de
construir el sistema completo. Por eso el alcance se ciñe a **un rol, un ámbito y la
documentación que ya existe**.

### El punto de partida

- Las métricas de un eMerchant están repartidas en varias herramientas que hay que abrir una a una.
- El conocimiento del puesto vive en personas, correos y documentos. No está en un sitio consultable.
- Existe un prototipo que marca lineamientos de estructura y lenguaje visual.
- Existe la guía eMerch, que da una base documental sobre la que un agente puede responder.

La oportunidad es unir las dos últimas: la estructura ya diseñada y una base documental que ya existe.

## Alcance

> **La regla que lo ordena todo: entra lo que tiene fuente.** Si un indicador no tiene de dónde
> alimentarse, o una funcionalidad depende de información que hoy se mantiene a mano, no forma
> parte de esta fase. Aplicarla desde el principio es lo que permite comprometer tres meses.

### Lo que se construye

Cinco piezas: tres visibles para el colaborador y dos que lo sostienen.

- **Un cockpit para un rol y un ámbito** — Responsable de eMerchandising, acotado a las familias
  que ya tienen guía eMerch.
- **3 KPIs** — los indicadores del rol, con fórmula, dueño y fuente definidos una sola vez.
- **3 tipos de alertas** — avisos con su impacto y el siguiente paso sugerido.
- **Un agente conversacional** — responde sobre los datos y la documentación del ámbito acordado,
  sobre la vista parametrizada, y **cita de dónde sale cada respuesta**.
- Y sin verse: **el catálogo de métricas** y **la trazabilidad**.

### Lo que se posterga

| Queda fuera | Motivo |
|---|---|
| Acciones y accionabilidad | El cockpit informa y responde. No propone flujos accionables ni ejecuta nada en los sistemas de origen. |
| Bandeja de acciones y decisiones | Requiere que alguien mantenga estado y bloqueadores, y en este alcance no hay backoffice. |
| Generación de agentes en autonomía | No forma parte de este encargo. |
| Configuración por el colaborador | Los KPIs y las alertas se definen con el eMerchant durante la inmersión. |
| Ingesta de cualquier formato | Se trabaja con los formatos que se acuerden en la inmersión, no con cualquier documento sin preparar. |
| Documentación que no exista hoy | El cockpit trabaja con lo que ya está escrito y acordado; no genera documentación nueva. |
| Notificaciones fuera del cockpit y versión móvil | Fuera de alcance. |

## Plan de trabajo

**1 · Inmersión y definición** — antes de construir nada, con entregable propio: el alcance
refinado y cerrado. Se acuerda qué familias entran y con qué documentación; qué fuentes de
conocimiento se consumen, en qué formato y quién da el acceso; qué KPIs y qué alertas componen el
cockpit y quién es dueño de cada uno; qué tablero esperan los colaboradores; y qué se mide para
saber si ha funcionado.

**2 · Diseño de la solución** — reutilización de la línea de diseño de las Guías eMerch, diseño de
pantallas e interacciones, prototipo navegable.

**3 · Construcción** — preparación de las vistas y la base documental sobre las fuentes acordadas;
implementación de KPIs, alertas y agente; definición y validación del comportamiento del agente
con las personas del área; pruebas y ajustes.

**Calendario:** tres meses, del **1 de octubre al 31 de diciembre de 2026**.

## Principios

- La IA **no consulta las fuentes libremente**: trabaja sobre vistas y documentos preparados para ese fin.
- Ante una misma pregunta, el sistema devuelve el mismo resultado.
- Cada KPI tiene **una fórmula, un dueño y una fuente**, definidos una sola vez.
- Si el agente no puede responder, lo dice. **No inventa.**
- El cockpit **no toma la decisión** por el colaborador, ni en esta fase ni en las siguientes.
- Todo queda registrado: lo que el sistema propone y lo que la persona decide.

## Qué se mide

> El éxito no es que el cockpit se use, sino que **se use y se discuta**.

- El eMerchant lo abre por su cuenta y lo incorpora a su día a día.
- Corrige respuestas, descarta propuestas con motivo y pide lo que falta.
- Cada KPI tiene una definición acordada, y una sola.
- Queda registrado qué pidieron los colaboradores y no existía: es la base para decidir qué hacer después.

## Equipo

Dedicación compartida a lo largo de los tres meses, con distinto peso según la fase: diseño y
definición funcional al inicio, desarrollo y QA en la segunda mitad.

| Perfil | Responsabilidad |
|---|---|
| Project Manager | Coordinación, seguimiento y relación con los equipos de Leroy Merlin. |
| Strategy Consultant | Facilita workshops, identifica oportunidades, define KPIs de éxito, evalúa escalabilidad y viabilidad técnica. |
| Diseño UX/UI | Inmersión con los eMerchants, definición funcional del tablero e interfaz. |
| AI Solution Architect | Arquitectura de agentes y stack; estrategia de prompts, RAG, modelos y orquestación. |
| Full stack Engineer | Cockpit, vistas, integraciones y desarrollo del agente. |
| QA | Pruebas funcionales y validación del comportamiento del agente. |

## Supuestos y dependencias

El plazo y el alcance se sostienen sobre estas condiciones. **Cada una lleva su alternativa, y
ninguna es cosmética**: o recortan alcance o degradan el producto.

| Ámbito | Supuesto | Si no se cumple |
|---|---|---|
| **Datos** | Las tablas necesarias están identificadas en BigQuery y hay entorno con datos reales. | Entran solo las áreas que tengan fuente. Se ajusta el alcance, no el plazo. |
| **Documentación** | Existe guía eMerch para las familias elegidas, en formato digital legible. | El alcance se acota a las familias que sí la tengan. |
| **Legal** | Si se incorporan fuentes con datos personales, existe base legal para tratarlas. | Se arranca solo con fuentes de trabajo, sin comunicación personal. |
| **Seguridad** | Criterios de IT por escrito sobre qué puede consultar un agente, y proveedor de modelo aprobado. | Se diseña sobre el criterio más restrictivo y se revisa después. |
| **Negocio** | Hay un dueño por KPI, un responsable de las reglas de alerta y eMerchants disponibles para la inmersión. | **Sin dueño de las alertas se entregan umbrales fijos, no alertas con criterio.** |

## Inversión

**63.000 €** (IVA no incluido), repartidos en tres mensualidades de **21.000 €** de octubre a
diciembre, facturadas la primera semana de cada mes. No incluye IVA, dietas y desplazamientos, ni
compra de imágenes, vídeos o derechos de terceros.
