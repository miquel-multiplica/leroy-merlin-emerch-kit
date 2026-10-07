# Project Context — Cockpit

## Qué es

El **eMerchant Cockpit**: un único sitio donde el Responsable de eMerchandising ve los
indicadores de su puesto y puede preguntar, en lenguaje natural, sobre los datos y la
documentación de su ámbito.

Sustituye el abrir una herramienta detrás de otra para componer la foto de su mundo, y el
preguntar por correo o de palabra lo que está escrito en la guía eMerch.

**Nombre público:** ⬜ TODO — el documento alterna «eMerchant Cockpit» y «Cockpit».

**Lo que NO es, y conviene decirlo pronto:** no es una bandeja de tareas ni un gestor de
acciones. La propuesta excluye explícitamente la accionabilidad y la bandeja de decisiones: el
cockpit **informa y responde**. Qué ve entonces el eMerchant al abrirlo es una de las preguntas
abiertas (ver `../ESTADO_MODULO_COCKPIT.md`).

## Por qué existe el proyecto

> **Procedencia.** Esto no está en la propuesta firmada: sale del deck anterior
> *«Cockpit y generación de agentes — propuesta por fases»* (v1.0, septiembre 2026, 87.710 €),
> que **quedó obsoleto** cuando el alcance se recortó a lo que hoy está firmado. Su detalle de
> fases y arquitectura ya no aplica. Se recoge aquí solo el encuadre del porqué, que la propuesta
> firmada no llegó a escribir.

**El objetivo no es un cockpit de e-commerce.** Es un modelo transversal a marketing, y empieza
por eMerchandising **porque es donde hay prototipo y datos** — no porque sea el rol más
importante. El cockpit es el vehículo; lo que se quiere demostrar es que el modelo se sostiene.

Un cockpit, definido en una línea: **el puesto de trabajo digital de un rol — su misión, sus
métricas, sus agentes.**

Y el cuello de botella está nombrado: *«tres agentes montados de nueve previstos. El cuello de
botella no es la IA, es la documentación.»* Por eso el ámbito son las familias que **ya** tienen
guía eMerch, y por eso el principio de no generar documentación nueva.

### Lo que pidió el cliente

Siete necesidades, en sus palabras. Son el mejor enunciado del motivo que tenemos:

| Necesidad | Cómo se resuelve |
|---|---|
| **No otro cuadro de mando más** | Alertas que llegan solas, con impacto y siguiente paso. |
| No un proyecto por cockpit, sino un sistema que los genere | Configuración declarativa por usuario. *(El generador es fase 2.)* |
| Que no dependa de meses de documentación | La ficha se propone desde las fuentes, la persona corrige. *(Fuera de lo firmado.)* |
| Que IT no vea un superagente | Agentes acotados sobre vistas parametrizadas. |
| Que se pueda medir el retorno | Trazabilidad de cada respuesta desde el día uno. |
| Que el agente aprenda de los documentos que ya existen | Ingesta del corpus en el formato en que está hoy. |
| Que sea el colaborador quien enseñe al agente, no el proveedor | En esta fase se define **con él**, durante la inmersión. |

La primera es la más útil para diseñar: **el cliente dice explícitamente que no quiere otro
cuadro de mando.** Es la mitad de la respuesta a qué ve el eMerchant al abrirlo.

### El objetivo de esta fase, en cinco palabras

**Dejar de buscar el dato.** Y la prueba de que ha funcionado, también del deck: *el eMerchant lo
usa a diario y lo desafía — corrige, descarta con motivo y pide lo que falta, y todo queda
registrado.* Es la misma frase que la propuesta firmada convirtió en *«que se use y se discuta»*.

### Dos encuadres más que conviene no perder

- **El cockpit orquesta, no reemplaza fuentes ni aplicativos.**
- **Se apoya en dos iniciativas que ya existen:** las guías eMerch como base documental y la
  analítica digital como fuente de métricas.

## Problema actual

- Las métricas del puesto están repartidas en varias herramientas que hay que abrir una a una.
- El conocimiento del puesto vive en personas, correos y documentos. No está en un sitio consultable.
- No hay una definición única de cada indicador: la misma pregunta puede dar respuestas distintas
  según quién la calcule.

## Objetivo

1. Reunir en una vista los indicadores del rol, cada uno con **una** fórmula, **un** dueño y
   **una** fuente.
2. Avisar de lo que merece atención, con su impacto y un siguiente paso sugerido.
3. Permitir preguntar sobre datos y documentación del ámbito, con la respuesta **citando de dónde sale**.
4. Dejar registrado qué se pidió y no existía, para decidir la fase siguiente con datos.

## Usuarios

| Rol | Descripción |
|---|---|
| Responsable de eMerchandising de Mundo | Único rol de esta fase. Lidera la estrategia eMerch de su mundo: navegación, animación comercial, calidad de catálogo y analítica. |

Detalle en [`user-roles.md`](./user-roles.md).

## Scope fase 1

Lo comprometido, literal de la propuesta:

- **Un rol y un ámbito.** El ámbito concreto —qué mundo y qué familias— **se define en la
  inmersión**, acotado a las familias que ya tienen guía eMerch.
- **3 KPIs**, con fórmula, dueño y fuente. Cuáles, ⬜ TODO (inmersión).
- **3 tipos de alertas**, con impacto y siguiente paso. Cuáles, ⬜ TODO (inmersión).
- **Un agente conversacional**, uno solo, que cita sus fuentes.
- Sin verse: catálogo de métricas y trazabilidad.

**La regla de corte:** *entra lo que tiene fuente*. Si un indicador no tiene de dónde alimentarse
o una funcionalidad depende de información que hoy se mantiene a mano, no entra.

**Queda fuera** (lista completa y motivos en [`../PROPUESTA_COCKPIT.md`](../PROPUESTA_COCKPIT.md)):
accionabilidad, bandeja de acciones y decisiones, generación de agentes, configuración por el
colaborador, ingesta de cualquier formato, documentación que no exista hoy, notificaciones fuera
del cockpit y versión móvil.

## Materiales de partida, y qué vale cada uno

- **La propuesta firmada** — [`../PROPUESTA_COCKPIT.md`](../PROPUESTA_COCKPIT.md). Manda sobre todo lo demás.
- **El demo HTML** — lo hizo **el cliente**; nosotros lo retocamos para que se entendiera
  internamente y pudieran hacer venta interna. Sirve **solo para entender la ambición**: no se
  reutiliza su código, su estructura ni sus decisiones de alcance. El diseño se hace de nuevo
  sobre los estilos de las Guías eMerch.
- **El mapeo de pantallas en Figma** (*Mapeo Cockpit*, 29 pantallas) — es **material de taller**,
  pedido para poder hacer la inmersión con Leroy y entender cada pieza y de dónde sale cada dato.
  No es una especificación de alcance.
