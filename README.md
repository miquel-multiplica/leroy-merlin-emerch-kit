# Leroy Merlin — eMerch Toolkit

Repositorio de trabajo del **eMerch Toolkit**: prototipos y documentación de producto de sus
módulos. Hoy cubre **Guías de Estilo**, **Descripciones**, **Validaciones** y **Cockpit**.

## Cómo está organizado

Dos niveles: lo que sirve a todos los productos vive arriba, en `context/`; lo específico de
cada uno vive en su carpeta. Y dentro de cada producto se separa **lo que hemos aprendido**
(`context/`) de **lo que hemos decidido** (`definition/`).

```
context/                        ← transversal: sirve a los cuatro productos
├── business/                   ← contexto de negocio y glosario
├── user/user-research/         ← investigación con usuarios
└── pseudo-design-system/       ← convenciones visuales de los prototipos

<producto>/
├── context/                    ← lo que sabemos
│   ├── scope/                  ← propuesta, alcance comprometido
│   ├── business/               ← entrevistas con stakeholders, material de cliente
│   ├── user/                   ← investigación propia del producto
│   └── product-as-is/          ← el producto tal como está hoy
└── definition/                 ← lo que decidimos
    ├── decisiones-y-pendientes.md   ← contexto entre sesiones
    ├── artefactos/             ← flujos, arquitectura de información, personas
    └── pendings/               ← abierto, sin cerrar
```

**Los prototipos se quedan en la raíz** para que las URLs de GitHub Pages no cambien.

## Prototipos

| Prototipo | Archivo | URL pública |
|---|---|---|
| Descripciones | [wireframe_descripciones.html](wireframe_descripciones.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_descripciones.html |
| Descripciones · MVP | [wireframe_descripciones_recortado.html](wireframe_descripciones_recortado.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_descripciones_recortado.html |
| Validaciones | [wireframe_validaciones.html](wireframe_validaciones.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_validaciones.html |
| Validaciones · MVP | [wireframe_validaciones_recortado.html](wireframe_validaciones_recortado.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_validaciones_recortado.html |

## Transversal

- [context/pseudo-design-system/](context/pseudo-design-system/) — convenciones visuales de los
  prototipos. Se llama *pseudo* porque el diseño final de UI todavía no existe.
- [context/business/](context/business/) — ⬜ pendiente: falta un **glosario** (gama, sección,
  familia, mundo, modelo, ATT, GV, TIP, 1P/3P). Leroy lo pidió en el kickoff.
- [context/user/user-research/](context/user/user-research/) — ⬜ pendiente: las entrevistas con
  eMerchs de la semana del 20 de octubre.

## Guías de Estilo

- [guias/definition/producto.md](guias/definition/producto.md) — documentación funcional.
- [guias/definition/pendings/publicacion-guias.md](guias/definition/pendings/publicacion-guias.md) — publicar o no publicar guías.

## Descripciones

- [descripciones/context/scope/alcance-mvp.md](descripciones/context/scope/alcance-mvp.md) — qué queda fuera de una primera versión.
- [descripciones/definition/funcional.md](descripciones/definition/funcional.md) — documentación funcional.
- [descripciones/definition/open-spec.md](descripciones/definition/open-spec.md) — especificación en formato OpenSpec.
- [descripciones/definition/decisiones-y-pendientes.md](descripciones/definition/decisiones-y-pendientes.md) — contexto entre sesiones.

## Validaciones

- [validaciones/context/scope/alcance-mvp.md](validaciones/context/scope/alcance-mvp.md) — los recortes del MVP y su razonamiento.
- [validaciones/definition/analisis.md](validaciones/definition/analisis.md) — análisis consolidado del módulo.
- [validaciones/definition/decisiones-y-pendientes.md](validaciones/definition/decisiones-y-pendientes.md) — contexto entre sesiones.
- [validaciones/definition/pendings/preguntas-cliente.md](validaciones/definition/pendings/preguntas-cliente.md) — preguntas abiertas con el cliente.

## Cockpit

- [cockpit/context/scope/propo.md](cockpit/context/scope/propo.md) — **la propuesta firmada**. Manda sobre cualquier otro material.
- [cockpit/context/scope/project-context.md](cockpit/context/scope/project-context.md) — qué es, por qué existe y de dónde parte.
- [cockpit/context/business/stakeholders-interviews/](cockpit/context/business/stakeholders-interviews/) — sesiones del 7 y del 14 de octubre.
- [cockpit/context/user/user-research/guion-entrevistas.md](cockpit/context/user/user-research/guion-entrevistas.md) — guion de inmersión.
- [cockpit/definition/decisiones-y-pendientes.md](cockpit/definition/decisiones-y-pendientes.md) — contexto entre sesiones.

## Infra

GitHub Pages vía GitHub Actions. El material en bruto del cliente vive en
`validaciones/context/business/docs-de-cliente/` y **no se versiona** — lo cubre el `.gitignore`.
