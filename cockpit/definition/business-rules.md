# Reglas de negocio — Cockpit

> Documento fuente de las reglas que gobiernan el producto. Nadie las resume en otros archivos.

## Reglas generales — los principios de la propuesta

Son compromisos firmados, no preferencias de diseño:

1. **La IA no consulta las fuentes libremente.** Trabaja sobre vistas y documentos preparados para
   ese fin.
2. **Determinismo.** Ante una misma pregunta, el sistema devuelve el mismo resultado.
3. **Una definición por KPI.** Cada indicador tiene una fórmula, un dueño y una fuente, definidos
   una sola vez.
4. **Si el agente no puede responder, lo dice. No inventa.**
5. **Cada respuesta cita de dónde sale.** Nivel de cita: ⬜ TODO.
6. **El cockpit no decide por el colaborador**, ni en esta fase ni en las siguientes.
7. **Todo queda registrado:** lo que el sistema propone y lo que la persona decide.

## La regla de alcance

> **Entra lo que tiene fuente.** Si un indicador no tiene de dónde alimentarse, o una
> funcionalidad depende de información que hoy se mantiene a mano, no forma parte de esta fase.

Es la regla que permite comprometer tres meses, y se aplica en la inmersión antes de diseñar nada.

## KPIs

⬜ TODO — los tres KPIs se definen en la inmersión. Cada uno necesita, antes de entrar:

- **Fórmula** — cómo se calcula, exactamente.
- **Dueño** — persona concreta que responde de esa definición.
- **Fuente** — tabla o vista de la que sale.

## Alertas

⬜ TODO — los tres tipos de alerta se definen en la inmersión. Cada una necesita:

- **Regla** — qué condición la dispara.
- **Impacto** — qué se pierde o se gana.
- **Siguiente paso sugerido** — sin ejecutarlo: el cockpit informa, no acciona.
- **Responsable de la regla** — sin esta figura, la propuesta dice que se entregan **umbrales
  fijos, no alertas con criterio**.

## Plazos

El proyecto va del **1 de octubre al 31 de diciembre de 2026**, y la primera versión debe estar
**en producción antes del cierre de 2026**.
