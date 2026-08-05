# Estado del módulo Descripciones — eMerch Toolkit

> **Propósito de este documento:** persistir el contexto de trabajo entre conversaciones. Actualizar al final de cada sesión relevante. Las marcas `[COMPLETAR]` son huecos que Miquel debe rellenar o que se rellenarán con el HTML actualizado.
>
> **Última actualización:** 2026-07-20

---

## 1. Qué es el módulo

El módulo de **Descripciones** del eMerch Toolkit permite la generación masiva de descripciones de producto cruzando:

1. La configuración de una **guía de estilo** publicada (plantillas de designación/descripción, atributos, ejemplos).
2. Los **valores reales de atributos por SKU** (subidos vía CSV — puente manual hasta que exista integración directa con catálogo/Mirakl).
3. Un **prompt personalizable**.

El output es un **CSV listo para importar en Mirakl**.

Flujo de pantallas del módulo: selección de guía → selección de modelo → subida/validación de CSV → configuración de prompt → vista previa → generación masiva → descarga.

## 2. Conceptos clave del módulo

- **Orden:** una ejecución de generación masiva. Tiene estados (en curso, completada, error, borrador, cancelada). `[COMPLETAR: lista exacta de estados y sus nombres en UI]`
- **Cancelar → convertir en borrador:** una orden cancelada se recupera después con su *configuración* (guía/modelo/prompt), no con el progreso ya generado. Funcionalidad confirmada como parte del alcance.
- **Orden raíz / reintentos:** un reintento se encadena siempre a la orden raíz original (evita cadenas indefinidas). El tooltip del icono de reintento muestra la orden madre (ej. "Reintento de la orden #1020") — construido en el prototipo como referencia visual.
- **Vista previa con puntuación:** los ejemplos generados en vista previa tienen una puntuación de calidad. Las sugerencias de mejora asociadas a puntuaciones bajas son candidata a recorte (ver documento de recortes, punto 3).
- **Detalle de orden:** incluye tabla de SKU's sin descripción generada, con desglose de fallos por falta de atributos.
- **Chevron + popover de guía:** en la tabla de órdenes, hover sobre el nombre del modelo abre popover con nombre de guía, última actualización y enlace "Ver guía" (sustituye a columna "Guía" fija).

## 3. Estado actual del trabajo

- **Documento de recortes:** finalizado y corregido → ver `PROPUESTA_RECORTES_DESCRIPCIONES.md`. Pendiente de revisión por desarrollo. Sin decisiones tomadas.
- **Prototipo HTML del módulo:** `[COMPLETAR: nombre del fichero .html vigente y fecha de la última versión — pendiente de subir a esta conversación]`
- **Pantallas construidas en el prototipo:** `[COMPLETAR: listado de pantallas/estados que ya existen en el HTML]`
- **Pendientes inmediatos tras el documento de recortes:** `[COMPLETAR]`

## 4. Decisiones tomadas (no reabrir sin motivo)

- El vínculo visible a la orden raíz **no** es candidato a recorte independiente: está implementado, es coste casi nulo, y depende del punto 2 del documento de recortes.
- `[COMPLETAR: otras decisiones cerradas de esta fase del módulo Descripciones]`

## 5. Convenciones de trabajo (aplican siempre)

- **Design system Leroy Merlin** (`DESIGN_LEROY_GUIAS_ESTILOS.md`): verde #188803 como único acento interactivo, radius 8px universal en controles (16px solo modales, pill solo badges), floating labels en todos los inputs, dos raíles fijos (header 64px + action bar), sin gradientes, sin sombras en botones, weight 600 para énfasis UI (nunca 500), validación en ámbar #c65200.
- **Iconos:** exclusivamente de la librería del skill wireframe-figma (`icon-svgs.md` / `icons.json`). Cero emojis. Regeneración completa del bloque `const ICONS={...}`, nunca parcial.
- **Ediciones del HTML:** vía script Python con `assert` en cada string match → encadenado bash con `set -e` (la copia al fichero final solo si la validación pasa). `node --check` para sintaxis JS, jsdom para smoke tests de runtime tras cada cambio de JavaScript.
- **Estilo de trabajo:** tácticos antes que estructurales; análisis/propuesta antes de implementar; validar comprensión de la estructura UX antes de construir; ejecución literal de las instrucciones (no abreviar textos, no mover elementos, no duplicar IDs).
- **Jerarquía visual sutil:** diferenciación por tonos de fondo y bordes, no por color ni negritas.
- **Idioma:** todo en español.

## 6. Threads abiertos

- Confirmaciones pendientes de desarrollo sobre el documento de recortes (solape Pausar/Reanudar con Borradores; persistencia de sugerencias IA; ahorro real del Grado 2 del punto 2).
- Integración directa con catálogo de Leroy Merlin / Mirakl (futuro; CSV es el puente actual para MVP/demo).
- `[COMPLETAR: otros threads abiertos]`

## 7. Protocolo anti-pérdida de contexto

Para no volver a chocar con el límite de conversación:

1. Al cerrar cada sesión de trabajo, actualizar este MD (sección 3 y 4 sobre todo) y guardarlo en local + subirlo al proyecto.
2. Documentos entregables (propuestas, análisis) siempre a fichero MD propio, nunca solo en el chat.
3. El HTML del prototipo se sube al inicio de cada conversación nueva; este MD da el contexto que el HTML no lleva.
4. Conversaciones largas de iteración sobre el prototipo: preferir sesiones cortas por bloque de cambios, cerrando cada una con actualización de este documento.
