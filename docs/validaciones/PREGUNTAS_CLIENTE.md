# Validaciones — preguntas pendientes para cliente

> **Qué es esto.** La redacción **para enviar al cliente** de las preguntas abiertas del módulo
> de Validaciones. La versión interna, con referencias a `§5.x` del análisis y vocabulario de
> equipo, está en `ESTADO_MODULO_VALIDACIONES.md` — este documento es la misma sustancia
> escrita para que la lea Leroy.
>
> **Alcance:** solo Validaciones. Las preguntas del eje *Publicar / No publicar* de Guías van
> por su lado, en `../guias/PUBLICACION_GUIAS.md`.
>
> **Última actualización:** 2026-09-23

---

Recogemos aquí todo lo que tenemos abierto. Va separado en dos bloques: lo que quedamos en
trabajar en una reunión, y lo que se puede responder por escrito.

## A · Para la sesión de trabajo

Son los tres temas que quedamos en ver juntos porque no se resuelven con un sí o un no.

**A1. Escala de criticidad.** Qué tipo de error corresponde a qué nivel de gravedad, y cuáles
llegan a considerarse *ficha que no debería estar publicada*. Es el núcleo del módulo: de esta
escala dependen las cifras del informe, el orden en que se presentan los errores y el contenido
del documento que recibe el proveedor. Nuestra propuesta actual (Bloqueante / Crítica / Leve)
es eso, una propuesta.

**A2. Falsos positivos — qué errores admiten descartarse.** Cuando el revisor considera que una
alerta no es un error real, puede descartarla. Nuestra hipótesis es que esto solo tiene sentido
en los errores que implican un juicio (un término técnico que el sistema no reconoce, un color
equivalente), y no en las ausencias objetivas (una ficha sin descripción, un atributo vacío).
Queremos confirmarlo con vosotros.

**A3. Falsos positivos — cómo se agrupan.** Cuando se descarta una alerta, es habitual que el
mismo motivo afecte a muchas referencias a la vez. Para poder ofrecer ese descarte en bloque
necesitamos saber qué puede exponer el motor: el término concreto que disparó la alerta, la
jerarquía familia → modelo → referencia, y cómo se determina que dos alertas son equivalentes.

## B · Por escrito

### Guía y reglas

**B1. Atributos obligatorios.** Cuando la guía marca atributos como obligatorios, ¿se refiere
solo a los de la ficha técnica, o también a los que la guía exige que aparezcan en la
designación y en la descripción? Y si faltan estos últimos, ¿con qué gravedad?

**B2. Ficha no publicable.** Además de faltar la designación o la descripción, ¿hay otros
mínimos que hagan que una ficha no debería estar publicada? Por ejemplo, estar por debajo del
mínimo de imágenes.

**B3. Nivel superior al modelo.** ¿Cuál es el nivel inmediatamente superior al modelo en vuestra
estructura: la familia de merchandising, la categoría web, u otra cosa? Lo necesitamos para
organizar el panel de configuración del motor, que va en tres niveles: reglas generales, nivel
intermedio y modelo.

**B4. Versiones de guía.** Una auditoría valida las referencias contra la guía tal y como está
en el momento en que se ejecuta. Pero la guía se puede editar después. Hoy nada registra con qué
versión se hizo cada auditoría, lo que tiene tres consecuencias: una auditoría revisada hace
semanas no se puede justificar después, un informe enviado a un proveedor puede pedir
correcciones que ya no aplican, y los descartes de falsos positivos quedan asociados a reglas
que ya no existen. **¿Queréis que cada auditoría guarde la versión de guía con la que se
ejecutó, y que se avise cuando la guía haya cambiado desde entonces?**

### Health Score *(para Javier Hernán)*

**B5. Granularidad.** ¿Nos entregáis el valor por referencia? Lo damos por hecho, porque vuestro
modelo puntúa cada referencia, y es lo que necesitamos: teniéndolo por referencia podemos
componer el Health Score de cualquier alcance —modelo, guía, categoría, proveedor, gama—
promediando. Si nos llega ya agregado a algún nivel, un Health Score por proveedor puede
sencillamente no existir.

**B6. Momento de cálculo.** ¿Se calcula al lanzar la auditoría, como foto de ese instante, o lo
actualizáis vosotros periódicamente?

**B7. Fecha de actualización.** ¿Nos dais fecha y hora de la última actualización por referencia?

**B8. Sobre qué se calcula el que ve el proveedor.** En vuestro documento, el Health Score
aparece en la cabecera del informe por proveedor junto a *SKUs auditados*, *SKUs conformes* y
*SKUs con alertas*, lo que sugiere que es la media sobre **todas** sus referencias auditadas, no
solo sobre las que fallan. Lo preguntamos porque cambia el significado: si fuera solo el de las
fichas con error, el número saldría siempre bajo y —esto es lo importante— **bajaría cuanto
mejor fuese el catálogo del proveedor**, porque quedarían menos fichas y todas malas.

**B9. Nos faltan dos cifras de esa cabecera.** Nuestro informe contiene los hallazgos, así que
sabemos qué referencias tienen error, pero no cuántas hay conformes ni el total auditado del
proveedor. ¿Nos las dais vosotros o las derivamos de BigQuery?

### Informes al proveedor

**B10. A quién dirigimos al proveedor.** El informe cierra invitándole a contactar con su
responsable e-merch. Como nos comentasteis que en 3P el TIP no interviene y es marketplace quien
habla con el seller, ¿vale ese cierre para todos los casos o cambia según sea 1P o 3P?

**B11. Un documento o varios por proveedor.** Hoy generamos un PDF por proveedor con todas sus
referencias del alcance. Un proveedor grande puede acumular miles de puntos a corregir
repartidos entre secciones distintas, y quien corrige sanitarios no suele ser quien corrige
griferías. **¿Preferís un único documento por proveedor, o poder trocearlo por categoría o
sección?** Si se trocea, hay que decidir además si el Health Score de cada documento se refiere
al proveedor entero o solo a esa parte.

**B12. Categorías web que mezclan modelos.** Hemos decidido que el documento del proveedor hable
de modelos y guías, porque es el lenguaje que él entiende, aunque el perímetro de la auditoría
se haya marcado por categoría web. Queda una duda: **una misma categoría web puede contener
referencias de modelos distintos — ¿cómo queréis que se agrupe el informe en ese caso?**

### Datos

**B13. Designación comercial.** El detalle referencia a referencia se entrega en CSV, y su
segunda columna es el nombre comercial del producto. Damos por hecho que el motor lo expone,
porque es precisamente el texto que auditamos, pero preferimos confirmarlo antes de cerrar el
formato.

## Dos cosas que no son preguntas

- Nos pedisteis el **enlace del prototipo** para ver el flujo de falsos positivos y cómo
  alimenta la configuración del motor. Os lo pasamos aparte.
- Tenemos el **PDF del proveedor rediseñado** y listo para que lo reviséis: portada, estructura
  y tono. Las cifras de impacto de negocio que incluye son de ejemplo, a la espera de que
  analítica facilite datos propios.
