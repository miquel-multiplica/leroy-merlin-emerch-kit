# Guías — separar "Completada" de "Publicada/visible": preguntas para devs

> **Contexto.** Hoy, cuando una guía tiene los **datos mínimos**, pasa a **Completada** y **se hace visible para todos automáticamente**. El cliente quiere que **cada creador controle si su guía es visible para el resto**. Antes de proponer diseño, hay que **contrastar con los devs** cómo funciona hoy y qué implicaría separar esos dos conceptos. Este doc es para esa conversación técnica; las preguntas de *negocio* (el porqué, quién aprueba) van después con el cliente (bloque al final).

## El cambio en una frase
Separar **un concepto que hoy hace dos trabajos** en **dos ejes independientes**:
- **Completitud** — `Pendiente → Completada` (automático, ¿están los datos mínimos?).
- **Visibilidad/Publicación** — `Privada → Publicada` (manual, ¿la ven/consumen los demás?).

Efecto: reaparece de facto un "borrador", pero como **Completada · Privada** (completa pero solo del creador), no como incompleta.

---

## A · Cómo funciona HOY (confirmar con devs)
1. **Estados reales**: ¿el estado de guía es solo `Pendiente`/`Completada`? ¿"visible para todos" es **implícito** (toda Completada es visible) o hay ya algún **flag/campo** de visibilidad?
2. **Herencia de PRODUCT.md**: la doc menciona `DRAFT` / `PUBLISHED` / `ARCHIVED` y `GuideModel` `PENDING`/`COMPLETED`. ¿Eso **existe en el modelo de datos** (aunque deprecado en UI) o ya no? ¿Se podría **reutilizar** un campo existente para la visibilidad?
3. **Cómo se calcula "Completada"**: ¿es cuando **todos los GuideModel** están `COMPLETED`? ¿Es **automático e irreversible**, o puede volver a Pendiente si se edita/quita un mínimo?

## B · Consumo por otros módulos (confirmar con devs)
4. **Descripciones**: ¿qué guías ve/usa hoy — **todas las Completadas** o hay filtro? (el wireframe dice "a partir de tus guías publicadas" → ¿es literal o copy antiguo?).
5. **Validaciones** ("Empezar validación" sale en las Completadas): ¿contra qué guía audita y **cómo la resuelve por modelo**?
6. **Unicidad por modelo**: ¿un modelo puede estar en **varias guías**? Si sí, ¿cómo se decide **cuál es la oficial** que consumen Descripciones/Validaciones?

## C · El cambio propuesto (viabilidad)
7. **Nuevo eje de visibilidad** independiente de Completada: ¿**coste**? ¿Hace falta **migración** (todas las guías actuales → `Publicada`) para no romper lo existente?
8. **Alcance de la visibilidad**: ¿`Privada` = **solo el creador** (+ admins), o hay **scoping por sección/rol**? ¿El modelo de permisos (`ADMIN`/`EDITOR`/`USER`) lo soporta ya?
9. **Descargas del dueño**: ¿podemos permitir que el **creador descargue PDF y atributos de su guía aunque esté Privada** (permiso a nivel de dueño), y que **los demás** solo puedan si está Publicada?

## D · Implicaciones a levantar (validar viabilidad/impacto)
10. **Despublicar algo en uso**: si se despublica una guía con la que ya se **generaron descripciones** o hay **auditorías** (en curso o hechas), ¿qué pasa con lo ya generado? ¿Se congela, se marca obsoleto, se bloquea?
11. **Versionado**: si se puede publicar → editar → republicar, ¿**republicar = versión nueva**? ¿Hace falta versionar la guía (como hicimos con el motor de validaciones)? ¿Qué versión ve cada consumidor?
12. **Puertas de acción**: hoy **"Empezar validación"** y **"Generar PDF"** aparecen en Completada. Con el nuevo eje, ¿deberían **exigir Publicada** (validación oficial) o valen en Privada (prueba del creador)?
13. **Notificación**: al publicar, ¿se **avisa** a los consumidores (validadores/editores)?
14. **Reversibilidad de estado**: ¿publicar bloquea la edición? ¿O se edita en caliente y se refleja al instante (sin versión)?

## E · Nota de UX (para cuando se prototipe, no bloqueante)
- **Publicación = estado con toggle** (chip "Publicada 👁 / Privada"), **no** un botón que compita con las acciones.
- **Una acción primaria contextual** + resto en menú, para no saturar la fila:
  - *Completada · Privada* → primaria **"Publicar"**; **"Descargar ▾"** (PDF · Atributos); validación/resto en kebab.
  - *Completada · Publicada* → primaria **"Empezar validación"**; **"Descargar ▾"**; "Despublicar" en el chip/kebab.
- Agrupar las dos descargas (PDF · Atributos) bajo **"Descargar ▾"** evita dos botones sueltos.

---

## Para el cliente (después de contrastar con devs)
- **El porqué / la necesidad real**: ¿es un **paso de aprobación** antes de que la guía sea la "oficial", o control de **work-in-progress** (afinar SEO/ejemplos aunque los mínimos estén)? Esto decide si "Publicar" es **visibilidad** o **aprobación**.
- **Gobernanza**: ¿quién puede **publicar/despublicar** — el creador, el responsable de catálogo/sección, admins? ¿Se puede despublicar la guía de otro?
