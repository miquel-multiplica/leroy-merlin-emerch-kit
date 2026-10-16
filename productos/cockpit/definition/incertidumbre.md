# Cockpit — mapa de incertidumbre

> **Qué es esto.** Lo que no sabemos todavía, con qué lo resolvería y en qué estado está. **No
> intenta cuadrar nada**: la mayoría de estas preguntas se contestan con dato de la inmersión, no
> razonando. Las que aquí ponen «abierta» se quedan abiertas a propósito.
>
> Se actualiza cuando algo se cierra, diciendo **con qué** se cerró. Lo que se cierre pasa a
> [`decisiones-y-pendientes.md`](./decisiones-y-pendientes.md).
>
> Abierto el 2026-10-16, antes de la semana de inmersión.

**Estados:** 🔴 bloquea diseñar · 🟡 condiciona pero se puede avanzar · ⚪️ se decide más tarde a
propósito

---

## 1 · Qué ve el eMerch al abrir

| | |
|---|---|
| **Qué no sabemos** | Si la home lleva métricas, alertas, pendientes, el agente, o en qué proporción |
| **Por qué importa** | Es la primera pantalla; de ahí cuelga todo lo demás |
| **Qué lo resolvería** | Las cuatro entrevistas: qué abren cada mañana y por qué eso |
| **Estado** | 🔴 |

**Lo que ya sabemos y no basta para cerrarlo:**

- El contrato compromete **3 KPIs, 3 alertas y 1 agente**.
- El cliente pidió **«no otro cuadro de mando más»**.
- El acta del kickoff apunta a tres preguntas al abrir: *qué ha cambiado · qué priorizar · qué
  acción puedo hacer*.
- El bench: las tres referencias dan el peso visual al agente y reducen las métricas al mínimo
  —Shopify las tiene, pero **pequeñas y arriba, con sparkline**—.

**Sub-preguntas que NO hay que cerrar ahora** *(⚪️)*:

- ¿La caja del agente va en la home, flotando abajo, o las dos cosas?
- ¿Franja de métricas sí o no, y de qué tamaño?
- **Qué ocupa el centro un martes que no hay ninguna alerta.**

## 2 · Cuáles son los tres KPIs

| | |
|---|---|
| **Qué no sabemos** | Cuáles, y quién responde de cada definición |
| **Por qué importa** | Sin dueño no hay definición acordada, y es la mitad del contrato |
| **Qué lo resolvería** | Las entrevistas dan los candidatos; el taller del 23 los cierra con dueño |
| **Estado** | 🔴 |

La ficha del puesto lista **cuatro** —CV del mundo, CR y ATC familia-mercado, % de GV vendibles— y
el contrato compromete tres. Pero con el alcance en calidad de catálogo es probable que los
candidatos reales sean otros: **GV Quality, ATT Imp.5, cobertura de guías**.

## 3 · Cuáles son las tres alertas, y si son tipos o niveles

| | |
|---|---|
| **Qué no sabemos** | Qué dispara cada una, y si «3 tipos» son tres reglas o tres niveles de una |
| **Por qué importa** | Tres reglas distintas es el triple de definición y de mantenimiento |
| **Qué lo resolvería** | El orden que salga de las entrevistas sobre los seis candidatos |
| **Estado** | 🔴 |

Seis candidatos sobre la mesa, cuatro de ellos dichos por el cliente:

1. Una familia concentra referencias con atributos obligatorios sin informar *(Pedro)*
2. Una PLP pierde más de la mitad de sus productos *(Pedro)*
3. El add-to-cart y la GV Quality caen a la vez en la misma familia *(Javier)*
4. Dos fuentes dicen cosas distintas de la misma referencia *(de los 30 min finales del 7 oct)*
5. Una familia vende y no tiene guía *(Pedro)*
6. Las imágenes que pide la guía no son las que el cliente clica *(Pedro)*

**Y falta el dueño de las reglas.** Sin él, la propuesta dice que se entregan *«umbrales fijos, no
alertas con criterio»*.

## 4 · El determinismo, que hoy no se cumple

| | |
|---|---|
| **Qué no sabemos** | Cuál es la fuente buena de cada cosa cuando dos dicen distinto |
| **Por qué importa** | La regla de oro dice *«ante la misma pregunta, el mismo resultado»*, y hoy no es cierto |
| **Qué lo resolvería** | La sesión con Javier Hernán |
| **Estado** | 🔴 |

El 7 de octubre, tres personas tardaron media hora en saber si un atributo estaba relleno. La
cascada que se usa —JSON, Drive 2026, Drive 2025, más STEP y Opus— **vive en la cabeza de Javier**
y no está escrita. Y la recarga de guías es **mensual**, no diaria.

**No está entre los cinco supuestos de la propuesta**, y debería.

## 5 · Qué es un «action item» y de dónde sale

| | |
|---|---|
| **Qué no sabemos** | Qué tablero, qué filtro, y si se lee o también se escribe |
| **Por qué importa** | De ello depende que la bandeja entre o no, y con qué coste |
| **Qué lo resolvería** | Fernando, que es el único que lo pidió |
| **Estado** | 🟡 |

Lo pidió Fer y **nadie de los que lo usarían reconoce la petición**. Pedro: *«no usamos Jira»*. Y
Susana hizo la pregunta que lo decide: *«¿vuelca en nuestras herramientas, o solo me avisa y me
voy al Jira a hacerlo?»*.

Leer no rompe nada de lo firmado. **Escribir sí** —*«no ejecuta nada en los sistemas de origen»*—
y además obliga a autenticar por usuario para que el ticket no lo cierre un bot.

## 6 · Dónde vive el cockpit en la navegación

| | |
|---|---|
| **Qué no sabemos** | Si es el paraguas o una sección más, cuando entren otros roles |
| **Por qué importa** | Rehacer navegación e identidad después es caro |
| **Qué lo resolvería** | Decisión de producto con Leroy; internamente ya hay postura |
| **Estado** | 🟡 |

**Internamente ya se acordó** en la sesión con Jordi del 15 de octubre: *el cockpit es la madre, y
Guías, Descripciones y Validaciones son aplicaciones que viven dentro*. Falta contrastarlo con
Leroy — y cuando se preguntó en el kickoff, no se entendió la pregunta.

Mientras tanto, **no cerrar ninguna de las dos puertas**: el cockpit al mismo nivel que las tres
herramientas, no enterrado en la navegación de una.

## 7 · Cuántas familias tienen guía, y cuáles

| | |
|---|---|
| **Qué no sabemos** | El número, y si son familias que al eMerch le importan |
| **Por qué importa** | Es el ámbito entero: *«las familias que ya tienen guía eMerch»* |
| **Qué lo resolvería** | Una lista. Se pidió en el kickoff |
| **Estado** | 🔴 |

Si son una minoría del catálogo, un indicador agregado no se puede calcular sobre ellas sin
explicar muy bien qué mide. Y si las que tienen guía no son las que pesan, el agente sabrá mucho
de lo que nadie le pregunta.

## 8 · Si el impacto para el proveedor se puede estimar

| | |
|---|---|
| **Qué no sabemos** | Si hay forma defendible de decir «si arreglas esto, ganas aquello» |
| **Por qué importa** | Es la POC 2 y el incentivo entero del proveedor |
| **Qué lo resolvería** | Javier, y una decisión de método |
| **Estado** | 🟡 |

Pedro ya avisó del riesgo: *«los KPIs pueden ser buenos y no haberse trabajado nada de calidad»*.
Y tiene razón — **de una foto transversal no sale causalidad**.

Lo que sí es defendible sin estadística: **la encontrabilidad**. Sin el atributo, el producto no
aparece cuando el cliente filtra por él. Eso es mecánico, no correlacional.

Connectif resuelve esto de una forma copiable: da el dato y, pegado, **qué no se puede concluir de
él** — *«esto no demuestra causalidad ni ventas incrementales»*.

## 9 · Dónde está la raya entre sugerir y accionar

| | |
|---|---|
| **Qué no sabemos** | Qué cuenta como *«siguiente paso sugerido»* y qué ya es un flujo accionable |
| **Por qué importa** | El contrato excluye lo segundo y compromete lo primero, sin definir la frontera |
| **Qué lo resolvería** | Decisión de producto, con las entrevistas delante |
| **Estado** | 🟡 |

La propuesta dice a la vez que el cockpit *«no propone flujos accionables»* y que cada alerta
lleva *«el siguiente paso sugerido»*. **Sugerir un siguiente paso es proponer una acción.** No hay
contradicción si se define la raya; no definirla es lo que deja entrar todo.

## 10 · Quién es el usuario, además del eMerch

| | |
|---|---|
| **Qué no sabemos** | Si el TIP necesita lo mismo filtrado o algo distinto |
| **Por qué importa** | Si es un filtro, es casi gratis; si es otra pantalla, es otro proyecto |
| **Qué lo resolvería** | Preguntar si el TIP trabaja transversal o por ámbito |
| **Estado** | 🟡 |

En Validaciones el reparto ya está hecho: **congruencia al TIP, completitud al seller**. Y en el
prototipo los hallazgos se reparten casi al 50%. Con el alcance en calidad de catálogo, **puede que
el TIP reciba más alertas que el eMerch**.

---

## Lo que NO es incertidumbre

Para no reabrir lo cerrado:

- El alcance es **calidad de catálogo**.
- Es **transversal**, no por mundo. Un mundo es una agrupación de categorías de producto.
- Es **un** agente.
- Va **integrado con Guías**.
- **El agente no calcula**: los números salen de consultas validadas; él interpreta y explica.
- Los **«12 widgets»** del deck son vaguedad deliberada para poder recortar módulos sin
  renegociar. No es una promesa de doce cosas.
