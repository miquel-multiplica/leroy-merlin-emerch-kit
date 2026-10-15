# Reunión de kickoff — 14 de octubre de 2026

**Proyecto:** Cockpit eMerch con agente de inteligencia artificial para Leroy Merlin  
**Equipos:** Multiplica y Leroy Merlin  
**Reunión de origen:** Evolución Q4 Guías eMerch + Analítica

Este documento recoge conclusiones e interpretación de la conversación. No constituye un acta de acuerdos ni un alcance cerrado. La fuente es una transcripción automática que puede contener errores.

## Conclusión principal

Leroy Merlin quiere una herramienta que ayude al equipo a decidir dónde actuar cada día y a poner en marcha esas acciones. El dashboard y el agente de IA son las piezas para conseguirlo.

La dirección del proyecto está bastante clara, aunque todavía hay que concretar las funcionalidades y los criterios de éxito.

## 1. Visión amplia y primer alcance centrado en calidad de catálogo

La visión futura es un cockpit que acompañe toda la misión del eMerch: navegación y experiencia, animación comercial, calidad de catálogo y analítica digital.

Para esta primera versión, se insiste en concentrarse en **calidad de catálogo y la analítica necesaria para mejorarla**, integrado en la herramienta de Guías eMerch.

Multiplica debe mantener visible esta diferencia: la visión futura orienta el diseño, pero no constituye el alcance comprometido.

## 2. Una experiencia que empieza con propuestas útiles

Una expectativa explícita es que, al comenzar la jornada, el usuario encuentre respuestas a estas preguntas:

- ¿Qué ha cambiado y requiere mi atención?
- ¿Qué problemas u oportunidades debería priorizar?
- ¿Qué acción puedo realizar y por qué?

El agente necesita un comportamiento proactivo. La conversación sirve para profundizar, pedir explicaciones o ajustar una propuesta, pero la herramienta debe aportar valor desde que se abre.

## 3. La conexión entre datos y acciones es el centro del producto

El recorrido que se extrae de la conversación es:

**Detectar una situación → explicar la evidencia → proponer una acción → validarla → hacer seguimiento.**

En la reunión aparecen ejemplos concretos:

- Información de producto incompleta.
- Modelos relevantes sin Guía eMerch.
- Necesidades de información que deben trasladarse a proveedores y sellers.

Algunas acciones corresponderán a información de producto, contenido o SEO. Aunque el usuario principal sea el eMerch, hay que definir cómo se derivan esas tareas y quién se responsabiliza de ellas.

La integración con Jira se plantea en este contexto, pero su funcionamiento concreto sigue pendiente.

## 4. La confianza será una condición de adopción

Se repite la preocupación por respuestas inventadas, errores numéricos y conclusiones equivocadas sobre el impacto comercial.

Como traducción propuesta a requisitos de producto, cada recomendación debería permitir entender:

- En qué datos se apoya.
- Qué criterio aplica.
- Qué información falta.

Los cálculos deberían ejecutarse mediante consultas y lógica validada. El agente puede interpretar la pregunta y explicar el resultado.

Observar que mejora la conversión después de completar información no demuestra que esa mejora se deba al cambio. Las estimaciones de impacto deberán presentarse con sus límites.

## 5. Las Guías eMerch aportan el conocimiento específico

Las guías contienen criterios sobre qué necesita cada familia de producto y cómo debería presentarse. Ese conocimiento, combinado con los datos, permitirá formular recomendaciones pertinentes para Leroy Merlin.

El proyecto depende de conocer la cobertura, validación y actualización de las guías, además de disponer de las vistas necesarias en BigQuery.

## 6. Una tensión de alcance que debe resolverse pronto

En la conversación aparecen aspiraciones como múltiples agentes, personalización individual, configuración autónoma y conexión entre áreas. También se reconoce que buena parte de estas capacidades queda para futuras fases.

Un matiz relevante es que **se pide mantener una visión transversal entre mundos**, aunque haya que reducir el número de métricas o funcionalidades. La propuesta inicial de limitarse a un mundo se cuestiona durante la reunión.

No deben considerarse cerrados ni el número de widgets ni todos los casos de uso mencionados. Son propuestas que deben contrastarse con su valor, la disponibilidad de datos y el esfuerzo necesario.

## 7. El éxito debe medirse en el trabajo del equipo

La conversación apunta al uso habitual, la confianza y la reducción de las dificultades actuales.

Como propuesta para concretar la medición, se podrían utilizar indicadores como:

- Tiempo necesario para detectar y preparar una acción.
- Utilidad de las recomendaciones.
- Acciones que llegan a resolverse.

Es necesario acordar con Leroy Merlin una situación de partida y unos objetivos medibles.

## Siguiente paso propuesto para Multiplica

Cerrar una lista corta de situaciones reales que la primera versión resolverá de principio a fin. Para cada una, definir:

- Usuario.
- Problema.
- Datos disponibles.
- Criterio de detección.
- Propuesta del agente.
- Responsable de actuar.
- Forma de medir el resultado.

## Formulación propuesta del proyecto

> Crear un espacio de trabajo para que el equipo eMerch identifique y priorice mejoras de calidad de catálogo, apoyado por un agente que explique los datos y proponga acciones bajo control del usuario.
