# Leroy Merlin — eMerch Toolkit

Repositorio de trabajo del **eMerch Toolkit**: prototipos (wireframes) y documentación de producto de sus módulos. Hoy cubre **Descripciones** (generación masiva de descripciones) y **Validaciones** (auditor de calidad de producto), sobre la base del sistema de **Guías de Estilo**.

## Wireframes (prototipos)

Se abren directamente en el navegador. Se sirven además vía GitHub Pages:

| Prototipo | Archivo | URL pública |
|---|---|---|
| Descripciones (completo) | [wireframe_descripciones.html](wireframe_descripciones.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_descripciones.html |
| Descripciones (versión recortada) | [wireframe_descripciones_recortado.html](wireframe_descripciones_recortado.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_descripciones_recortado.html |
| Validaciones (auditor de calidad) | [wireframe_validaciones.html](wireframe_validaciones.html) | https://miquel-multiplica.github.io/leroy-merlin-emerch-kit/wireframe_validaciones.html |

## Documentación

### General
- [docs/PRODUCT.md](docs/PRODUCT.md) — contexto de producto del sistema completo (Guías de Estilo).
- [docs/diseno/DESIGN_LEROY_GUIAS_ESTILOS.md](docs/diseno/DESIGN_LEROY_GUIAS_ESTILOS.md) — convenciones de design system.

### Descripciones
- [docs/descripciones/FUNCIONAL_DESCRIPCIONES.md](docs/descripciones/FUNCIONAL_DESCRIPCIONES.md) — documentación funcional del módulo.
- [docs/descripciones/PROPUESTA_RECORTES_DESCRIPCIONES.md](docs/descripciones/PROPUESTA_RECORTES_DESCRIPCIONES.md) — análisis de recortes (sin decisiones tomadas).
- [docs/descripciones/ESTADO_MODULO_DESCRIPCIONES.md](docs/descripciones/ESTADO_MODULO_DESCRIPCIONES.md) — estado del trabajo / contexto entre sesiones.
- [openspec/specs/descripciones/spec.md](openspec/specs/descripciones/spec.md) — especificación en formato OpenSpec.

### Validaciones
- [docs/validaciones/ANALISIS_VALIDACIONES.md](docs/validaciones/ANALISIS_VALIDACIONES.md) — análisis consolidado del módulo (auditor de calidad).

## Estructura del repo

```
/
├─ README.md
├─ wireframe_descripciones.html           # prototipos (se quedan en raíz → URLs estables)
├─ wireframe_descripciones_recortado.html
├─ docs/
│  ├─ PRODUCT.md
│  ├─ descripciones/
│  ├─ validaciones/
│  └─ diseno/
└─ openspec/specs/descripciones/spec.md
```
