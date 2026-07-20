# Guías de Estilo — Documentación de Producto

## El problema que resuelve

Las empresas que gestionan catálogos de productos a gran escala (miles de modelos, millones de SKUs) necesitan garantizar que cada producto esté descrito de forma coherente, completa y alineada con los estándares editoriales de la compañía.

Sin una herramienta centralizada, los equipos de contenido trabajan de forma dispersa: criterios diferentes por producto, errores en atributos obligatorios, multimedia incompleta o fuera de norma. El resultado es un catálogo inconsistente que penaliza la experiencia de compra y genera retrabajo costoso.

**Guías de Estilo** resuelve esto proporcionando un sistema que:

1. Define qué debe contener la descripción de cada modelo de producto (atributos, ejemplos de texto, multimedia).
2. Permite que los equipos de contenido tengan una referencia clara y validada.
3. Valida automáticamente si los CSV de productos entregados por los clientes cumplen con esa referencia.
4. Acelera el trabajo editorial con sugerencias generadas por IA.

---

## A quién va dirigido

| Rol | Necesidad |
|-----|-----------|
| **Editor de contenido** | Completar guías de estilo por modelo de producto (designación, descripción, atributos, multimedia) |
| **Responsable de catálogo** | Publicar guías, gestionar el estado de los modelos y validar entregas de CSVs de cliente |
| **Administrador** | Gestionar usuarios y permisos, importar el catálogo maestro de datos |

---

## Los datos del sistema

### Catálogo maestro (datos de referencia, importados desde CSV)

Son los datos estructurales sobre los que trabaja toda la aplicación. Se importan una vez y se actualizan periódicamente.

| Entidad | Descripción |
|---------|-------------|
| **Section** | Categorías de producto (Construcción, Climatización, Decoración, etc.) — 13 en total |
| **Model** | Modelos de producto (`MOD_XXXXX`) — ~1.000 modelos |
| **ModelAttribute** | Atributos de cada modelo (`ATT_XXXXX`): nombre, tipo (cualitativo/cuantitativo), obligatoriedad — ~30.000 atributos |
| **AttributeAvailableValue** | Valores permitidos por atributo |
| **ModelMediaOrder** | Reglas de multimedia por modelo: tipo de imagen/vídeo requerido, cantidad mínima y máxima |
| **ProductSku** | SKUs vendibles vinculados a un modelo — ~2 millones |
| **SkuAttrValue** | Valores de atributos por SKU |
| **SkuMedia** | Referencias multimedia por SKU — ~20 millones |

### Datos operacionales (creados por los usuarios)

| Entidad | Descripción |
|---------|-------------|
| **Guide** | Guía de estilo. Tiene nombre, sección, trimestre (`Q1`–`Q4`) y estado (`DRAFT` / `PUBLISHED` / `ARCHIVED`) |
| **GuideModel** | Cada modelo asignado a una guía. Tiene un estado automático (`PENDING` / `COMPLETED`) |
| **GuideModelStepItem** | Items que forman la plantilla de designación o descripción: pueden ser atributos del catálogo o texto libre |
| **GuideModelStepMeta** | Ejemplos de texto (hasta 3) y SEO text para designación y descripción |
| **GuideModelAttribute** | Atributos documentados en la pestaña "Atributos" con su valor, tipo y posición |
| **GuideMedia** | Items multimedia de la guía: tipo, subcategoría, ángulo, imagen de referencia, cantidades mín/máx |
| **AiCallLog** | Registro de auditoría de cada llamada a la IA: prompt, respuesta, tokens, latencia, estado |

### Datos auxiliares fijos

| Entidad | Descripción |
|---------|-------------|
| **MediaType** | Tipos de multimedia disponibles: Packshot, Ambient, Packaging, etc. — 15 en total |
| **MediaDropdownOption** | Valores de dropdown para subcategoría (5) y ángulo (8) |

---

## Flujo de trabajo

### 1. Autenticación

El usuario accede con email y contraseña. El sistema devuelve un JWT. Todas las rutas están protegidas; hay tres roles: `ADMIN`, `EDITOR`, `USER`.

---

### 2. Listado y gestión de guías (`/`)

La pantalla principal muestra todas las guías con sus estados. Desde aquí el usuario puede:

- **Crear** una nueva guía eligiendo nombre, sección y trimestre.
- **Editar** los datos básicos de una guía existente.
- **Duplicar** una guía para reutilizar su estructura en otro trimestre.
- **Eliminar** (soft delete) una guía.
- **Filtrar** por sección, trimestre, estado o búsqueda por nombre.

---

### 3. Wizard de edición de guía

Una guía se completa a través de un wizard con pasos secuenciales. Hay una barra lateral siempre visible con la lista de modelos asignados y su estado (`PENDING` / `COMPLETED`).

#### Paso 1 — Seleccionar modelos (`/wizard`)

El usuario busca y selecciona los modelos del catálogo que quiere incluir en la guía. Cada modelo seleccionado crea un `GuideModel` con estado `PENDING`.

---

#### Paso 2 — Designación (`/wizard/:modelId/designacion`)

Define la plantilla de **cómo se nombra el producto** (título corto).

El usuario configura:
- **Items**: lista ordenada de atributos del catálogo o textos libres que componen la designación.
- **SEO text**: texto optimizado para buscadores.
- **Ejemplos** (hasta 3): ejemplos reales de designación para ese modelo.

Los ejemplos pueden generarse automáticamente con IA (Google Gemini).

**Requisito para completar**: mínimo 3 items y 1 ejemplo.

---

#### Paso 3 — Descripción (`/wizard/:modelId/descripcion`)

Define la plantilla de **cómo se describe el producto** (texto largo).

El usuario configura:
- **Atributos**: los atributos que forman el cuerpo de la descripción.
- **Keywords**: palabras clave relevantes.
- **Ejemplos** (hasta 3): ejemplos reales de descripción para ese modelo.

Los ejemplos también pueden generarse con IA.

**Requisito para completar**: mínimo 3 atributos y 1 ejemplo.

---

#### Paso 4 — Atributos (`/wizard/:modelId/atributos`)

Define los **atributos obligatorios y opcionales** que deben estar presentes en la ficha del producto.

El usuario revisa y documenta:
- Qué atributos son obligatorios.
- El valor esperado para cada atributo.
- El tipo (cualitativo o cuantitativo).

**Requisito para completar**: mínimo 7 atributos.

---

#### Estado automático del modelo

El sistema recalcula automáticamente el estado del `GuideModel` cada vez que se guarda una pestaña:

```
COMPLETED si:
  ✓ Designación: >= 3 items + >= 1 ejemplo
  ✓ Descripción: >= 3 atributos + >= 1 ejemplo
  ✓ Atributos: >= 7 atributos

PENDING si no se cumplen todos los requisitos
```

---

#### Paso 5 — Multimedia (`/wizard/multimedia`)

La multimedia es **transversal a toda la guía** (no por modelo). Define qué tipos de imagen o vídeo son necesarios para los productos de esa guía.

El usuario configura:
- Tipo de multimedia (Packshot, Ambient, Packaging, vídeo…).
- Subcategoría y ángulo.
- Cantidad mínima y máxima requerida.
- Imagen de referencia.

---

### 4. Validación y publicación

#### Validación interna

Antes de publicar, el sistema comprueba que todos los modelos de la guía tengan estado `COMPLETED` y que la multimedia esté correctamente definida.

#### Publicación

El responsable cambia el estado de la guía a `PUBLISHED`. A partir de ese momento la guía es la referencia oficial para ese conjunto de modelos y trimestre.

#### Exportación

Una guía publicada puede exportarse en dos formatos:
- **JSON**: estructura completa de la guía (para integraciones).
- **PDF**: documento visual con toda la información (modelos, designación, descripción, atributos, multimedia).

---

### 5. Validación de CSV de cliente (`/validation/:guideId`)

Una vez publicada la guía, los equipos de cliente entregan sus ficheros de producto en formato CSV. El sistema valida automáticamente si esos ficheros cumplen con la guía.

El usuario sube el CSV y recibe:
- **Resumen**: total de filas, válidas, con error, con advertencia.
- **Detalle de errores por fila**:
  - `MISSING_MANDATORY`: atributo obligatorio vacío.
  - `EMPTY_OPTIONAL`: atributo opcional sin rellenar (advertencia).
  - `INVALID_VALUE`: valor cuantitativo fuera de rango permitido.
  - `INVALID_MEDIA_TYPE`: tipo de multimedia no permitido para el modelo.
  - `MISSING_MEDIA`: multimedia requerida no presente.

El usuario puede descargar los errores en un CSV con codificación UTF-8 (BOM) para compartir con el cliente.

---

## Integraciones externas

### Google Gemini (IA)

Se usa para generar ejemplos de designación y descripción. La IA recibe el contexto del modelo (atributos, valores, sección) y devuelve tres propuestas de texto que el usuario puede aceptar, modificar o descartar.

Cada llamada queda registrada en `AiCallLog` con: prompt enviado, respuesta recibida, tokens consumidos, latencia y estado (éxito / error). Hay un panel de uso en `/ai/usage`.

---

## Restricciones y decisiones de diseño relevantes

- **Soft delete en guías**: las guías eliminadas no se borran de la base de datos, se marcan con `deletedAt`.
- **La multimedia es por guía, no por modelo**: todos los modelos de una guía comparten las mismas reglas de multimedia.
- **El catálogo es de solo lectura desde el frontend**: los modelos, atributos y SKUs solo se pueden consultar, no modificar desde la interfaz.
- **La reimportación del catálogo es aditiva (upsert)**: no borra datos existentes.
- **Duplicación de guías**: copia toda la estructura (modelos, designación, descripción, atributos, multimedia) con estado `DRAFT`.
- **Paginación offset-limit** en todos los listados.
- **JWT con refresh token**: el `accessToken` tiene vida corta; el `refreshToken` permite renovarlo sin relogin.

---

## Mapa de pantallas

```
/login                          → Autenticación
/                               → Listado de guías (HOME)
/wizard                         → Paso 1: seleccionar modelos
/wizard/:modelId/designacion    → Paso 2: designación
/wizard/:modelId/descripcion    → Paso 3: descripción
/wizard/:modelId/atributos      → Paso 4: atributos
/wizard/multimedia              → Paso 5: multimedia (transversal)
/wizard/summary                 → Resumen final
/guides/:id                     → Detalle de guía
/validation/:guideId            → Validación CSV de cliente
```
