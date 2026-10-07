# Modelo de datos — Cockpit

> Modelo conceptual, sin detalles de implementación.

## Entidades principales

### KPI

La propuesta fija su forma aunque no su contenido: **fórmula, dueño y fuente, definidos una sola
vez**. El conjunto de los KPIs con sus definiciones es el **catálogo de métricas**, una de las dos
piezas que no se ven pero sostienen el cockpit.

| Campo | Notas |
|---|---|
| Nombre | ⬜ TODO |
| Fórmula | Exacta. Es lo que hace que la misma pregunta dé el mismo resultado. |
| Dueño | Persona que responde de la definición. |
| Fuente | Vista o tabla concreta. |
| Ámbito | Mundo y familias sobre los que se calcula. |

### Alerta

| Campo | Notas |
|---|---|
| Tipo | Tres en esta fase. ⬜ TODO cuáles. |
| Condición | La regla que la dispara. |
| Impacto | Qué está en juego. |
| Siguiente paso | Sugerido, nunca ejecutado. |
| Responsable de la regla | ⬜ TODO. |

### Fuente documental

Lo que el agente puede leer. Por contrato, **solo documentación que ya exista** y **solo en los
formatos acordados en la inmersión** — no ingesta de cualquier formato.

⬜ TODO — qué formato tiene hoy la guía eMerch.

### Vista

La capa preparada sobre la que trabaja la IA, porque no consulta las fuentes libremente.

⬜ TODO — qué tablas de BigQuery la alimentan.

### Traza

La segunda pieza invisible. Registra lo que el sistema propone y lo que la persona decide, y
también **lo que se pidió y no existía** — que es la base para decidir la fase siguiente.

⬜ TODO — qué se guarda exactamente y dónde.

## Dependencias de datos

Las dos que pueden recortar alcance, literales de la propuesta:

- **BigQuery** — las tablas necesarias identificadas y un entorno con datos reales. Si no:
  *entran solo las áreas que tengan fuente; se ajusta el alcance, no el plazo*.
- **Guía eMerch** — existente para las familias elegidas, en formato digital legible. Si no:
  *el alcance se acota a las familias que sí la tengan*.
