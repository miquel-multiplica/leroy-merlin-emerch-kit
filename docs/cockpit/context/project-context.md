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
