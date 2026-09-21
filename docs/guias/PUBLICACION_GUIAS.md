# Guías — publicar o no publicar

> El cliente quiere **una capa más** sobre las guías de estilo: que además de estar *completadas*, puedan estar **publicadas o no**. Este doc recoge lo que confirmó desarrollo y las preguntas que quedan para el cliente.

## Cómo funciona hoy

Confirmado con desarrollo el **2026-09-21**:

- **No hay ninguna capa de visibilidad.** Todo el mundo ve todas las guías, completadas y pendientes.
- **El antiguo `DRAFT`/`PUBLISHED` ya no existe** como tal: `draft` pasó a significar *pendiente* y `published`, *completada*. No hay campo reaprovechable — el eje habría que crearlo de cero.
- **La matriz de roles está implementada** (ver [PRODUCT.md](../PRODUCT.md)). Crear guías solo pueden Administrador General y e-Merch.
- **Solo existe una versión de cada guía.**

## Respuestas de desarrollo

| Pregunta | Respuesta |
|---|---|
| ¿"Visible para todos" es implícito o hay un flag? | Todo el mundo ve todo, completado y pendiente |
| ¿La matriz de roles está implementada? | Sí |
| ¿e-Merch/TIP ven las completadas ajenas? | Sí, y las no completadas también |
| ¿Existe aún `DRAFT`/`PUBLISHED` en BD? | `draft` es pendiente y `completado` es completado |
| ¿Qué guías ve Descripciones para generar? | **De momento no se ha definido** |
| ¿Contra qué guía audita Validaciones? | Solo las completadas |
| ¿Un modelo puede estar en varias guías? | Se puede, pero no debería, para evitar definiciones contradictorias |
| ¿Coste del nuevo eje? ¿Migración? | El día que se aplique se decidirá si todo pasa a no publicado o se mantiene publicado |
| ¿Publicar afecta a validar/descargar? | Quien no tenga permisos seguirá sin tenerlos |
| ¿El creador descarga informes sin publicar? | Publicada es un flag para que el resto la vea; **no cambia el resto de comportamientos** |
| ¿Los informes siguen las reglas por rol? | Esto no cambia |
| ¿Despublicar una guía en uso? | **No avisamos de nada** |
| ¿Republicar tras editar crea versión? | **No, solo hay una versión** |

**Tres cosas que se abren con estas respuestas:**

1. **Sin versionado y sin aviso**, una guía publicada puede editarse o retirarse y quien la esté usando no se entera → pregunta 7.
2. **Qué guías ve Descripciones está sin definir**, y el nuevo eje obliga a resolverlo → pregunta 3.
3. **Un modelo puede estar en varias guías**; con visibilidad de por medio deja de ser higiene y pasa a ser ambigüedad real → pregunta 8.

## Preguntas para el cliente

*Punto de partida, tal como está hoy: todos ven todas las guías, completadas y pendientes. Y crear guías solo pueden hacerlo los perfiles Administrador General y e-Merch.*

1. **¿Para qué queréis esta funcionalidad?** ¿Qué problema resuelve, o qué está pasando hoy que no debería pasar? Nos ayuda saber si nace de que alguien vio una guía a medias y la usó, de que hace falta una aprobación antes de que sea la oficial, o de otra cosa. Todo lo demás depende de esta respuesta.
2. **Una guía no publicada, ¿se oculta o se marca?** ¿Debe desaparecer del listado de los demás, o seguir viéndose señalada como no publicada? Y si se oculta, ¿también para el Administrador General, o él sigue viéndolo todo?
3. **¿Publicar condiciona algo más que la visibilidad?** ¿Una guía sin publicar puede seguir usándose —generar descripciones, lanzar validaciones, exportar— por quien tenga permiso, o debería quedar fuera de uso hasta publicarse?
4. **Quién publica y despublica.** ¿Solo quien creó la guía? ¿Puede un e-Merch despublicar la de otro e-Merch? ¿Y el Administrador General, la de cualquiera?
5. **¿Se puede publicar una guía incompleta?** "Completada" se calcula solo al tener todos los datos necesarios. Si los dos ejes son independientes, podrían convivir guías publicadas e incompletas.
6. **El día del cambio.** ¿Las guías que ya existen quedan todas publicadas (nadie nota nada) o todas despublicadas (cada creador decide qué enseñar)?
7. **Una guía en uso que se edita o se despublica.** Si se retira o modifica una guía con la que ya se generaron descripciones o se hicieron auditorías (escenario futuro), el cambio es inmediato y silencioso para quien la estuviera usando. ¿Asumible, o hace falta avisar, congelar lo ya generado, o registrar con qué versión se trabajó?
8. **Un modelo en varias guías.** Hoy es posible aunque no debería. Si dos guías publicadas cubren el mismo modelo, no hay regla de cuál manda al generar o auditar. ¿Queréis que el sistema lo impida?

**Cómo leerlas:** la 1 condiciona al resto. Si la respuesta es *"es una aprobación"*, la 3 y la 5 casi se resuelven solas; si es *"para no enseñar borradores"*, la que manda es la 2. La 2 y la 3 están acopladas: si contestan *"marcar, no ocultar"*, publicar deja de ser visibilidad y pasa a ser control de uso, que es otra funcionalidad.

## Estado

- **Enviado al cliente:** pendiente.
- Al volver las respuestas, aterrizarlas aquí y actualizar [PRODUCT.md](../PRODUCT.md), donde el eje figura como *cambio solicitado, sin construir*.
