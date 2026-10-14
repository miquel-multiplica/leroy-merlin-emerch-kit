# Cockpit — estado del módulo

> Contexto entre sesiones. Qué está decidido, qué está abierto y por qué.
>
> **Estado a 7 de octubre de 2026.** El proyecto arrancó el 1 de octubre y la **inmersión empieza
> hoy**, con más de una sesión prevista.

---

## Dónde estamos

Hay propuesta firmada y nada construido. La inmersión es la que cierra el alcance real: qué
mundo, qué familias, qué tres KPIs, qué tres alertas y de qué fuentes salen.

**El alcance firmado** está volcado en [`PROPUESTA_COCKPIT.md`](./PROPUESTA_COCKPIT.md) y manda
sobre cualquier otro material.

## Lo decidido

- **Un solo agente.** El demo enseña ocho especialistas; no es el alcance.
- **Un solo rol**, el Responsable de eMerchandising. El demo también se recortó por ahí.
- **El demo no se reutiliza.** Lo hizo el cliente y nosotros lo retocamos para que se entendiera
  internamente y pudieran hacer venta interna del proyecto — sin saber todavía mucho de lo que
  resolvía. Sirve para entender la ambición, **no** como base de código, estructura ni alcance.
- **Diseño nuevo sobre los estilos que ya tenemos**, reutilizando la línea de las Guías eMerch,
  como dice la propuesta.
- **El mapeo de Figma es material de taller.** El canvas *Mapeo Cockpit* (29 pantallas) se pidió
  para poder recorrer con Leroy cada pieza y entender de dónde sale cada dato. No es alcance.

## Lo grande que está abierto

Cuatro cosas de la propuesta que conviene cerrar con quien la vendió, antes de diseñar.

### 1 · «3 KPIs», pero el puesto tiene cuatro

La ficha del rol lista cuatro indicadores —CV del mundo, CR familia-mercado, ATC familia-mercado y
% de GV vendibles— y la propuesta compromete tres. O se cae uno, o «3 KPIs» quería decir otra
cosa. Afecta a lo primero que se diseña.

### 2 · «3 tipos de alertas»: ¿tipos o niveles?

No es lo mismo **tres tipos** —tres reglas distintas, cada una con su condición, su dueño y su
mantenimiento— que **tres niveles** de una misma regla con umbrales. La propuesta dice «avisos con
su impacto y el siguiente paso sugerido», que suena a tipos, y eso multiplica por tres el trabajo
de definición en la inmersión.

### 3 · Qué ve el eMerchant al abrir el cockpit

La propuesta excluye la **accionabilidad** y la **bandeja de acciones y decisiones**: el cockpit
informa y responde. Pero una bandeja es justo lo que haría a alguien abrirlo cada mañana, y el
criterio de éxito firmado es *«que se use y se discuta»*. Hay que resolver qué ocupa ese sitio.

**Media respuesta la da el deck anterior**: el cliente pidió *«no otro cuadro de mando más»*, y la
pieza que lo evita son *«alertas que llegan solas, con impacto y siguiente paso»*. O sea que el
peso de la pantalla no está en los KPIs sino en las alertas — lo contrario de lo que sugiere el
demo. Queda por resolver qué hace esa pantalla **cuando no hay ninguna alerta**.

También hay una tensión menor que conviene mirar: la propuesta dice que el cockpit *«no propone
flujos accionables»*, y a la vez que cada alerta lleva *«el siguiente paso sugerido»*. Se
reconcilian —sugerir en texto no es montar un flujo— pero hay que decidir dónde está la raya.

### 4 · Los cinco supuestos están escritos como si ya se cumplieran

Y ninguna de sus alternativas es cosmética: o recortan alcance o degradan el producto. Las dos que
más pesan:

- **Datos** — *«las tablas necesarias están identificadas en BigQuery»*. Si no lo están, entran
  solo las áreas con fuente: se ajusta el alcance, no el plazo.
- **Negocio** — *«hay un dueño por KPI y un responsable de las reglas de alerta»*. Sin esa figura,
  la propia propuesta dice que se entregan **umbrales fijos, no alertas con criterio**. Es una
  degradación del producto escrita en el contrato.

Las otras tres —documentación, legal y seguridad— están en
[`PROPUESTA_COCKPIT.md`](./PROPUESTA_COCKPIT.md). La de **seguridad** (criterios de IT por escrito
sobre qué puede consultar un agente, y proveedor de modelo aprobado) es la que menos controlamos
y la que puede parar el proyecto entero.

### 5 · Dónde vive el cockpit, y qué pasa cuando deje de ser de calidad

**Preguntado en el kickoff del 14 de octubre y no resuelto.**

Hoy tenemos tres herramientas —Guías, Descripciones y Validaciones— y las tres son de **calidad
de catálogo**. El cockpit de fase 1 también, así que encaja de forma natural a su lado o dentro.

**Pero el cockpit no se queda ahí.** En cuanto entren otros perfiles —opecoms y animación, CV y
negocio— esos usuarios **no usan ninguna de las tres**. Y entonces la arquitectura que hoy parece
obvia deja de serlo.

Hay una contradicción que conviene nombrar:

- En la sesión se pidió *«integrar data **dentro de la app de guías** a modo de cockpit»*.
- Y el deck dice *«el cockpit **orquesta**, no reemplaza fuentes y/o aplicativos»*.

**Orquestar no es estar dentro.** Son dos arquitecturas distintas y solo se nota al dibujar la
navegación.

**Qué hay que preguntar:**

- ¿El cockpit vive **dentro** de la app de guías, o la app de guías es **una de las cosas a las
  que el cockpit lleva**?
- Cuando entre el eMerch de opecoms, ¿abre la misma aplicación? ¿Entra por su **rol** o por su
  **herramienta**?
- ¿Las tres herramientas actuales conservan entrada propia, o se accede solo desde el cockpit?
- ¿Hay una capa común de identidad y rol, o cada herramienta mantiene la suya?

**Mientras no se responda**, lo barato es **no cerrar ninguna de las dos puertas**: mantener el
cockpit al mismo nivel que las tres herramientas, no enterrado en la navegación de una de ellas.
Construir dentro hoy y tener que sacarlo en 2027 obliga a rehacer navegación y, probablemente,
identidad y permisos. Dejar la costura puesta no cuesta casi nada.

## El deck de kickoff promete más que el contrato

Revisado el **14 de octubre**, el día de su sesión. Corrige bien varias cosas del deck
anterior —el reto ya es *«disponibilizar un MVP de eMerchant Cockpit»* y no la generación de
agentes; un solo agente; fuentes decididas; y una slide de infraestructura (GCP en entorno
Multiplica, aislado de producción, migración solo cuando esté validado) que responde al supuesto
de Seguridad sin que lo pregunten—.

**Pero arrastra cuatro líneas del deck de 87.710 € que el recorte dejó fuera:**

| El deck de kickoff dice | La propuesta firmada dice |
|---|---|
| «La home y las áreas de responsabilidad… **tope 12 widgets**» | **3 KPIs máximo** |
| «**Acciones propuestas por área**, con su siguiente paso» | Accionabilidad fuera de alcance |
| «**Bandeja de acciones y decisiones: consulta con JIRA**» | Bandeja explícitamente excluida |
| «**Prueba de inferencia acotada**: la ficha se propone desde las fuentes» | No aparece |

La primera es la que más pesa: **12 widgets frente a 3 KPIs es cuatro veces el alcance**, y es el
número que se queda anclado en quien lo ve proyectado.

**Descuadre menor:** el calendario dice «12 semanas» y la tabla llega a S16 y Mes 4. El proyecto
va del 1 de octubre al 31 de diciembre — **13 semanas**.

## Jira no es conocimiento, son tareas

El deck parecía contradecirse: nombra Jira en *«qué haremos»*, no lo dibuja en la arquitectura, y
mantiene que las fuentes de conocimiento son dos y ninguna es Jira. **No hay contradicción si se
separan las dos naturalezas:**

| | Alimenta a | Cómo |
|---|---|---|
| **Conocimiento** — BigQuery + guías eMerch | el **agente** | vistas parametrizadas, con trazabilidad de cada respuesta |
| **Operación** — Jira | la **bandeja** | lectura directa de un tablero, sin pasar por el agente |

Y de ahí salen cuatro consecuencias:

- **El agente no lee Jira.** Responde sobre datos y guías; los pendientes los enseña la bandeja.
  Mantiene limpio el alcance del agente y la slide de «dos fuentes» se queda correcta.
- **No necesita vista parametrizada.** Un tablero se lee y se pinta: es lo más barato de la lista,
  aunque suene a lo contrario.
- **No entra en la trazabilidad**, que aplica a las respuestas del agente.
- **Pero tiene otro ritmo.** BigQuery va a diario, las guías se recargan **una vez al mes** y las
  tareas cambian cada hora. Una bandeja desfasada es peor que un KPI desfasado: un número viejo se
  nota, una tarea ya hecha que sigue ahí te hace perder el tiempo y dejas de fiarte de la pantalla.

**Y la bandeja vuelve a entrar de forma legítima:** se excluyó porque alguien tendría que mantener
estado y no hay backoffice. Si los items vienen de Jira, **el backoffice es Jira**.

Lo que sigue sin saber nadie —ni Pedro, ni Susana, ni Javier— es **de qué tablero y con qué
filtro**. Es la pregunta para Fernando.

## Una observación para la inmersión

El ámbito son «las familias que ya tienen guía eMerch». Conviene saber **cuántas son de verdad**
antes de prometer KPIs de mundo: si son una minoría del catálogo, un indicador agregado del mundo
no se puede calcular sobre ellas sin explicar muy bien qué está midiendo.

## Dónde está cada cosa

- [`PROPUESTA_COCKPIT.md`](./PROPUESTA_COCKPIT.md) — el alcance firmado.
- [`context/`](./context/) — contexto funcional: producto, roles, reglas, datos, flujos y specs.
- Demo HTML del cliente y mapeo de Figma — materiales de entrada, no viven en el repo.
