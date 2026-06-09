---
name: sodeker-docs
description: Genera documentación de proyectos de software con identidad visual Sódeker (blanco + turquesa + sans-serif) y diagramas/figuras interactivas estilo Distill. Cubre resumen funcional, documentación técnica, instalación, soporte y manual de usuario. Salida HTML por defecto, Markdown bajo petición. Layout con contenido a la izquierda y sidebar de navegación a la derecha.
user-invocable: true
---

# Sódeker Docs

Skill para producir documentación de software con estética Sódeker (landing page corporativa minimalista en blanco + turquesa) combinada con la mecánica de figuras y controles interactivos de Distill.

## Cuándo activarse

Invocar cuando el usuario pida:
- Documentar un módulo, servicio, sistema o feature.
- Crear documentación técnica o funcional.
- Construir una guía de instalación, runbook de soporte o manual de usuario.
- Generar un portal de documentación para un proyecto.

Frases gatillo: "documenta este proyecto", "crea la documentación de…", "necesito un manual de usuario para…", "haz un runbook de soporte de…", "documentación técnica de…", "/sodeker-docs".

## Qué produce

Documentación organizada en hasta 6 bloques:

1. **Resumen funcional** — objetivo, problema, usuarios, funcionalidades, reglas de negocio, casos de uso, restricciones.
2. **Documentación técnica** — arquitectura, stack, estructura, patrones, decisiones, integraciones, dependencias, configs, diagramas (arquitectura, BD, despliegue, flujos).
3. **Instalación y configuración** — requisitos, variables, comandos, ejecución local, pruebas, datos seed, troubleshooting.
4. **Soporte** — errores comunes, logs, alertas, métricas, reinicio de servicios, estado del sistema, contactos, procedimientos ante incidentes, FAQs. **Escrito en lenguaje accesible para personas no técnicas.**
5. **Manual de usuario** (cuando aplica) — login, funciones principales, capturas, errores comunes, FAQs, guías paso a paso.

Formato de salida:
- **HTML** (por defecto) — interactivo, sidebar a la derecha, figuras estilo Distill.
- **Markdown** (a petición) — `git-friendly`, diagramas Mermaid, bloques HTML embebidos cuando el renderer los soporta.

## Cómo invocar

```
/sodeker-docs [módulo o proyecto] [--md|--html]
/sodeker-docs documentar el módulo de inventarios
/sodeker-docs en markdown — sistema de facturación
```

## Flujo de trabajo (paso a paso obligatorio)

Cuando se invoca la skill, ejecutar este flujo:

### 1. Descubrir el contexto
Hacer preguntas focalizadas (máximo 5–6) antes de generar nada:
- Nombre y propósito del proyecto/módulo.
- Usuarios principales (roles).
- Stack principal (lenguajes, frameworks, base de datos, infraestructura).
- ¿Existe repo? Si sí, ¿ruta? — para extraer estructura real y `package.json`/`requirements.txt`/etc.
- ¿Qué secciones aplican? (a veces no hay manual de usuario, a veces no hay soporte aún).
- Formato preferido (HTML/Markdown).

### 2. Detectar el formato
- Por defecto: **HTML**.
- Cambiar a Markdown si el usuario menciona: "markdown", ".md", "README", "para el repo", "para Git", "para GitHub", "para GitLab".

### 3. Cargar las referencias internas de la skill
**Antes de escribir cualquier output**, leer:
- `style/sodeker.css` — tokens visuales. Nunca inventar colores ni fuentes.
- `content-spec/00-principios.md` — reglas globales de redacción.
- `content-spec/0{N}-*.md` — qué debe responder cada sección que vas a generar.
- `templates/html/shell.html` y `templates/html/components.html` — estructura base y snippets de componentes.
- `templates/markdown/shell.md` — estructura base markdown.
- `documentation-widgets/README.md` — catálogo de 13 patrones interactivos (flujos, hotspots, scrubber temporal, drag pipeline, árbol de decisión, capas, antes/después, matching, editor de estructura, tabla comparativa, mapa conceptual, joins SQL, wizard). Consultar la **tabla de selección** y los **esquemas de datos por patrón** para decidir cuál usar y cómo poblarlo.
- `documentation-widgets/patterns/0{N}-*.html` — implementación lista para inyectar de cada patrón.
- `examples/ejemplo-inventarios.html` — output de referencia general.
- `examples/documentation-importacion-data.html` — **referencia obligatoria** para: terminales estilo mac-apple (clases `.terminal`, `.terminal__bar`, `.terminal__dots`, `.terminal__body` con tokens `.t-cmt`, `.t-prompt`, `.t-cmd`, `.t-arg`, `.t-flag`, `.t-ok`), bloques SQL (`.sql-box`), comandos numerados (`.cmd-stack`/`.cmd-item`), flujo interactivo de N etapas (`.interactive-flow` con nodos `.if-node`, conectores `.if-arrow` y navegación `.if-nav`) y listas de pills (`.pill-list`, `.pill--accent`).

### 4. Generar la documentación

**Ubicación por defecto (HTML)**:
- Carpeta destino: `documentation/` en la raíz del proyecto. Si no existe, créala. Si ya existe, **reutilízala** (no crear `documentation-2/`, `documentation-new/`, etc.).
- Archivo HTML: `documentation/documentation-<slug-del-modulo>.html` (slug en kebab-case, ej. `documentation-inventarios.html`, `documentation-facturacion.html`).
- Hoja de estilos: `documentation/sodeker.css` — **una sola copia compartida** por todos los HTML de la carpeta.

**Regla de reutilización del CSS** (evita duplicar el archivo de estilos):
1. Antes de copiar `sodeker.css`, verifica si ya existe en `documentation/sodeker.css`.
2. Si existe, **no lo sobreescribas** ni lo copies de nuevo. El nuevo HTML simplemente lo referencia con `<link rel="stylesheet" href="./sodeker.css">`.
3. Si no existe, cópialo una sola vez desde la skill.
4. Nunca generar el CSS al lado del HTML cuando ya vive en la misma carpeta `documentation/`.

**Override del usuario**: si el usuario pidió explícitamente una ruta distinta (ej. "déjalo en `docs/inventarios/`", "ponlo en el escritorio"), respeta esa ruta y aplica la misma regla de reutilización del CSS en ese directorio.

**Layout HTML**: usar `.doc-layout` con contenido a la izquierda y `<nav class="doc-nav">` a la derecha (sticky, scroll independiente).

**Tema claro / oscuro (obligatorio en HTML)**: cada documento incluye el botón `.theme-toggle` (luna/sol) fijo en la esquina superior derecha. Las tres piezas — script anti-flash en `<head>`, botón al inicio del `<body>`, handler al final — están en `templates/html/shell.html` y en el ejemplo. El estado se guarda en `localStorage` (`sodeker-docs-theme`) y respeta `prefers-color-scheme` en la primera visita. **No** quitar el botón ni el script: forma parte del estándar Sódeker Docs.

**Markdown**: un archivo `.md` por sección dentro de una carpeta `docs/`, más un `README.md` que actúa como índice. (Markdown no genera CSS, así que no aplica la regla de reutilización.)

### 5. Verificar visualmente
- HTML: abrir el archivo en el navegador con la herramienta apropiada. Confirmar que el sidebar está a la derecha, los colores son turquesa/blanco/gris (nunca crema o coral), y no hay muros de texto.
- Markdown: confirmar que Mermaid renderiza, las tablas se ven limpias, y no hay bloques HTML rotos.

## Reglas inviolables

### Identidad visual (Sódeker)
- Color acento único: `#0097A7` (turquesa). Nunca coral, púrpura, degradados, ni tonos cálidos.
- Fondo: blanco (`#FFFFFF` o `#FCFCFD`). Nunca crema cálido tipo Distill.
- Tipografía: sans-serif (Inter, Manrope). Nunca serif para el cuerpo.
- Títulos mixtos: parte en negro + palabra clave en turquesa. Usar `<span class="accent">` o equivalente.
- Layout: contenido a la izquierda, sidebar `<nav>` **a la derecha** (sticky). NADA arriba, NADA en la columna izquierda.

### Principios de contenido
- **Cero muros de texto.** Siempre cards, tablas, callouts, métricas, o bloques visuales.
- Cada sección abre con: badge pequeño turquesa → título grande → subtítulo gris → bloques de contenido.
- **Usa analogías** para explicar conceptos complejos. El lector objetivo aprende mejor con gráficos y ejemplos.
- Mantén vocabulario técnico, pero **explícalo siempre**. No asumas que el lector es el desarrollador original.
- La sección de Soporte se escribe en **lenguaje accesible para no desarrolladores** (analista de soporte, QA, mesa de ayuda). Sin jerga sin contexto.
- **Diagramas cuando aporten claridad, no por inercia.** Si un concepto se entiende sin ayuda visual (definición corta, lista de pasos lineal, parámetros), no fuerces un SVG. Reserva el diagrama para lo que sí lo necesita: relaciones que se ven mejor que se leen (X → Y → Z con bifurcación), arquitecturas con varios componentes, flujos con ramas, transformaciones de datos. Mejor pocos diagramas potentes que muchos decorativos.
- **Consistencia en card-grids**: dentro de un mismo `.card-grid`, todas las cards usan la misma variante (`card`, `card--accent` o `card--accent-left`). No mezclar dentro del mismo grupo conceptual.
- **Métricas solo con datos reales**: `metric-grid` solo cuando represente cifras verificables (capacidad, latencia, volumen, KPIs). Nunca como decoración tipo dashboard en guías conceptuales.
- Sentence case en titulares, etiquetas y botones (no Title Case ni MAYÚSCULAS).

### Elementos interactivos (Distill)
- Figuras: borde hairline (`1px solid var(--ink-100)` o `--border-hair`), nunca sombras pesadas.
- Conectores: flechas grises delgadas (~1.5px) con curvas bezier suaves.
- Elementos interactivos (sliders, drag): badge "mano" turquesa (`assets/interactive-hand.svg`) para señalarlos.
- Animaciones cortas (120–360ms), respeta `prefers-reduced-motion`. Sin bounces.
- Diagramas de flujo: tarjetas conectadas + flechas, numeradas, breves.

### Gráficos interactivos (uso selectivo, no decorativo)
- **Regla de uso**: un widget interactivo cuesta atención del lector. Úsalo solo cuando *aporte comprensión real* que un diagrama estático no puede dar (explorar estados, comparar variantes, recorrer pasos, manipular un input). Si el concepto se entiende igual con un SVG estático o una tabla, no metas widget.
- **Mínimo y máximo por documento**:
  - **Obligatorio**: el concepto central del módulo (lo que el lector debe entender primero) lleva **un** widget interactivo bien construido — datos reales, etiquetas claras, navegación que se siente útil.
  - **Máximo recomendado**: 2 widgets por documento. Solo agrega un segundo si hay un concepto secundario *igualmente complejo* que justifique otro modo de interacción distinto. Tres o más casi siempre es abuso.
  - **Nunca**: dos widgets del mismo patrón en el mismo documento salvo que comparen explícitamente dos cosas (ej. flujo de importación vs. flujo de exportación). Repetir el mismo patrón con datos distintos cansa al lector.
- **Antes de meter un widget, pregúntate**: ¿el lector necesita *hacer click / arrastrar / navegar* para entender esto, o solo necesita *ver* la relación? Si la respuesta es "solo ver", usa SVG estático o tabla.
- **Cuándo SÍ usar widget** (orientativo, no exhaustivo): la arquitectura del sistema con componentes que el lector debe explorar uno por uno; una máquina de estados con transiciones; un pipeline donde el orden importa y se aprende moviendo; un árbol de decisión con varias ramas; una comparación N×M de opciones. **Cuándo NO usar widget**: definiciones, listas de pasos lineales sin ramificación, dependencias simples, FAQs, configuraciones.
- Cuando decidas que sí va widget, abre `documentation-widgets/README.md`, ubica el concepto en la **tabla de selección** y elige el patrón:
  - Procesos lineales (3–6 pasos) → `01-flow-steps.html`
  - Arquitectura con componentes → `02-hotspots-architecture.html`
  - Máquinas de estado / ciclos de vida → `03-time-scrubber.html`
  - Composición de middlewares / pipelines → `04-drag-pipeline.html`
  - Reglas de negocio condicionales → `05-decision-tree.html`
  - Stack por niveles → `06-toggle-layers.html`
  - Migraciones / refactors → `07-before-after-slider.html`
  - Glosarios / terminología → `08-term-matching.html`
  - Configs / queries que se construyen → `09-json-editor.html`
  - Comparación N×atributos → `10-comparison-table.html`
  - Relaciones no lineales → `11-concept-map.html`
  - Operaciones de conjuntos / SQL joins → `12-sql-joins.html`
  - Tutoriales con validación → `13-wizard-steps.html`
- Copia el bloque del patrón, **reemplaza únicamente el objeto `// === DATOS ===`** del `<script>` con los datos del concepto. No modifiques la lógica.
- Si en una página coexisten dos widgets del mismo patrón (caso justificado), prefija los `id` con el slug del concepto (ej. `flow-imports-nc0`, `flow-exports-nc0`) para evitar colisiones.
- Cuando el concepto no encaje exactamente en ningún patrón, prioriza adaptar el más cercano antes que inventar uno nuevo desde cero.
- Para conceptos secundarios donde una imagen ayude pero la interacción no aporte, usa **SVG estático estilo Distill** (hairline, conectores grises, tipografía Inter). No es obligatorio que cada concepto tenga uno: solo cuando el texto no basta para que el lector "vea" la relación. Tabla, cards o pills suelen ser suficientes para definiciones, parámetros o listados.

### Bloques de consola (estilo mac-apple)
- Toda sección que muestre comandos de terminal usa el bloque `.terminal` del ejemplo `examples/documentation-importacion-data.html`: cabecera oscura `.terminal__bar` con los tres puntos `.terminal__dots`, título en caps con tracking, y cuerpo `.terminal__body` en mono claro sobre fondo `#0F172A`.
- Colorea cada token con la clase correspondiente: `.t-cmt` (comentarios), `.t-prompt` (`$`), `.t-cmd` (comando), `.t-arg` (argumentos), `.t-flag` (flags), `.t-ok` (resultado positivo). No usar HTML genérico (`<pre><code>`) cuando el contenido es una sesión de terminal.
- Cuando convivan dos terminales (ej. worker + cliente), usa el grid `.terminal-pair` con `.terminal-col` para alinearlos lado a lado.
- Para comandos numerados con explicación, usa `.cmd-stack` + `.cmd-item` (cabecera clara con número turquesa, cuerpo oscuro con el comando, footer claro con la nota).
- Para queries SQL u otros snippets DSL con fondo oscuro, usa `.sql-box` con `.sql-box__bar` (etiqueta del motor/contexto) y `.sql-box__body` (tokens `.kw`, `.tbl`, `.val`, `.cm`).
- Nunca renderices comandos en bloques claros con borde. La consola siempre va oscura, tipo macOS.

### Lo que NUNCA hacer
- No usar emojis (Distill no los usa, Sódeker tampoco).
- No usar degradados, fondos oscuros dominantes, sombras pesadas Material Design.
- No usar Bootstrap, Tailwind utility classes, ni frameworks de UI por encima. Usar solo `sodeker.css`.
- No inventar colores fuera de la paleta de tokens.
- No copiar literalmente texto del repo del usuario. Siempre parafrasear con voz Sódeker.

## Archivos de la skill

```
documentation-skill/
├── SKILL.md                           ← este archivo
├── README.md                          ← readme público (GitHub)
├── LICENSE                            ← MIT
├── style/
│   └── sodeker.css                    ← tokens + componentes + layout
├── content-spec/                      ← qué debe responder cada sección
│   ├── 00-principios.md
│   ├── 01-resumen-funcional.md
│   ├── 02-documentacion-tecnica.md
│   ├── 03-instalacion-configuracion.md
│   ├── 04-soporte.md
│   └── 05-manual-usuario.md
├── templates/
│   ├── html/
│   │   ├── shell.html                 ← layout base con sidebar derecho
│   │   └── components.html            ← snippets reutilizables
│   └── markdown/
│       └── shell.md                   ← estructura base markdown
├── documentation-widgets/             ← catálogo de patrones interactivos
│   ├── README.md                      ← tabla de selección + esquemas de datos
│   └── patterns/
│       ├── 01-flow-steps.html
│       ├── 02-hotspots-architecture.html
│       ├── 03-time-scrubber.html
│       ├── 04-drag-pipeline.html
│       ├── 05-decision-tree.html
│       ├── 06-toggle-layers.html
│       ├── 07-before-after-slider.html
│       ├── 08-term-matching.html
│       ├── 09-json-editor.html
│       ├── 10-comparison-table.html
│       ├── 11-concept-map.html
│       ├── 12-sql-joins.html
│       └── 13-wizard-steps.html
├── examples/
│   ├── ejemplo-inventarios.html               ← output de referencia general
│   ├── ejemplo-inventarios.md
│   └── documentation-importacion-data.html    ← referencia para terminales mac-apple,
│                                                 .cmd-stack, .sql-box e .interactive-flow
└── assets/
    └── interactive-hand.svg           ← badge mano turquesa
```

## Notas finales

- Si el usuario no especifica algo (ej. qué módulo, qué stack), **pregunta** — no inventes.
- Si el repo del proyecto está disponible, léelo para extraer hechos verificables (estructura, dependencias, variables de entorno reales). Nunca documentes algo que no se pueda verificar.
- Cuando termines de generar, dile al usuario qué archivo abrir y qué revisar primero.
