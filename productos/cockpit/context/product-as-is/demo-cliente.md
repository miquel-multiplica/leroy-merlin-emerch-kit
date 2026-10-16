# El demo del cliente

> **Es la visión a largo plazo, no el producto actual.** No existe un «producto as-is» del
> Cockpit: lo único que hay es este demo, y lo que enseña es a dónde quiere llegar el cliente
> algún día, no lo que hay hoy ni lo que se va a construir.

## Qué es

Un HTML navegable que **hizo el cliente**. Nosotros lo retocamos para que se entendiera
internamente y pudieran hacer venta interna del proyecto — sin saber todavía mucho de lo que
resolvía.

## Para qué sirve, y para qué no

**Sirve** para entender la ambición y el lenguaje con el que la cuentan.

**No sirve** como base de código, de estructura ni de alcance. El diseño se hace de nuevo sobre
los estilos de las Guías eMerch, que es lo que dice la propuesta.

## Qué enseña, y cuánto de eso está comprometido

| El demo enseña | Lo firmado |
|---|---|
| ~20 métricas repartidas en seis pestañas | **3 KPIs** |
| 9 alertas en tres niveles | **3 alertas** |
| 8 agentes con selector | **1 agente** |
| Selector de cinco mundos | transversal, sin selector de mundo |
| Bandeja de acciones y decisiones como pantalla de inicio | excluida en la propuesta |

**La distancia es el dato.** Cuando alguien diga «como en el demo», esta tabla es la respuesta.

## Dos cosas técnicas, por si se vuelve a enseñar

- **El chat no funciona.** Llama a `api.anthropic.com` desde el navegador sin clave: da error.
  Las conversaciones del lateral están escritas a mano en el código.
- **No lleva `noindex`.**

## El mapeo de Figma

El canvas *Mapeo Cockpit* (29 pantallas, con diagrama de flujo) se pidió como **material de
taller**, para poder recorrer con Leroy cada pieza y entender de dónde sale cada dato. **No es
alcance** ni especificación de pantallas.
