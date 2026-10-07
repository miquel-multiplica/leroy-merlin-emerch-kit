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

## Una observación para la inmersión

El ámbito son «las familias que ya tienen guía eMerch». Conviene saber **cuántas son de verdad**
antes de prometer KPIs de mundo: si son una minoría del catálogo, un indicador agregado del mundo
no se puede calcular sobre ellas sin explicar muy bien qué está midiendo.

## Dónde está cada cosa

- [`PROPUESTA_COCKPIT.md`](./PROPUESTA_COCKPIT.md) — el alcance firmado.
- [`context/`](./context/) — contexto funcional: producto, roles, reglas, datos, flujos y specs.
- Demo HTML del cliente y mapeo de Figma — materiales de entrada, no viven en el repo.
