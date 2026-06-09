# Documentation Widgets

Catálogo de 13 patrones interactivos para enriquecer documentación técnica generada por la skill. Cada patrón es un fragmento HTML autocontenido (markup + CSS + JS) listo para inyectar en una página de documentación.

## Cómo usar este catálogo desde la skill

1. La skill analiza el concepto que está documentando.
2. Selecciona el patrón más adecuado (ver tabla de selección abajo).
3. Carga el archivo HTML correspondiente desde `patterns/`.
4. Reemplaza el bloque `// === DATOS ===` del `<script>` con los datos del concepto.
5. Inyecta el fragmento en la página.

Todos los patrones siguen la misma estructura:

```html
<section class="section pattern-XX" id="...">
  <header class="section__header">
    <span class="badge">...</span>
    <h2>...</h2>
    <p class="lede">...</p>
  </header>
  <p class="pattern-hint">
    <span class="interactive-badge">↺</span> Pista de interactividad.
  </p>
  <div class="pattern-content">...</div>
</section>
<style>/* estilos namespaced */</style>
<script>(function(){
  // === DATOS === (la skill reemplaza este bloque)
  const data = {...};
  // === LÓGICA === (no tocar)
})();</script>
```

## Variables CSS esperadas

Define estas variables en tu hoja de estilos global. Si alguna falta, los patrones tienen valores por defecto en línea (vía `var(--ink-500, #5f5e5a)`).

```css
:root {
  /* Tipografía */
  --fs-small: 13px;

  /* Escala de grises */
  --ink-50:  #f8f7f4;   /* superficie suave */
  --ink-100: #f1efe8;   /* fondo de panel */
  --ink-200: #e4e2d8;   /* borde tertiary */
  --ink-300: #d3d1c7;   /* borde secondary */
  --ink-500: #78716c;   /* texto muted */
  --ink-700: #292524;   /* texto principal */

  /* Marca */
  --accent:       #534AB7;
  --accent-soft:  #EEEDFE;
  --accent-dark:  #3C3489;

  /* Semánticos */
  --success:      #0F6E56;
  --success-soft: #E1F5EE;
  --success-dark: #04342C;
  --warning:      #854F0B;
  --warning-soft: #FAEEDA;
  --warning-dark: #412402;
  --danger:       #A32D2D;
  --danger-soft:  #FCEBEB;
  --danger-dark:  #501313;

  /* Acento secundario (mapas/categorización) */
  --info:         #185FA5;
  --info-soft:    #E6F1FB;
  --info-dark:    #0C447C;
}
```

## Clases globales asumidas

Estas clases ya existen en tu sistema de estilos:

| Clase | Uso |
|-------|-----|
| `.section` | contenedor de bloque |
| `.section__header` | cabecera con badge + h2 + lede |
| `.badge` | etiqueta pequeña sobre el título |
| `.accent` | resaltado en línea dentro del título |
| `.lede` | párrafo introductorio |
| `.btn`, `.btn--primary`, `.btn--secondary` | botones |
| `.interactive-badge` | el ↺ que indica interactividad |

Si alguna no existe, cada patrón funciona igual (usa fallback inline).

## Tabla de selección de patrón

| # | Patrón | Cuándo usarlo |
|---|--------|---------------|
| 01 | Flujo por etapas | Procesos lineales con 3-6 pasos cronológicos |
| 02 | Hotspots arquitectura | Sistemas con componentes que deben explicarse uno a uno |
| 03 | Scrubber temporal | Ciclos de vida, máquinas de estado con tiempo |
| 04 | Pipeline arrastrable | Composición de middlewares, filtros, transformaciones |
| 05 | Árbol de decisión | Reglas de negocio con condiciones encadenadas |
| 06 | Capas conmutables | Stack tecnológico, arquitectura por niveles |
| 07 | Antes/después | Refactors, migraciones, comparación de dos estados |
| 08 | Matching | Glosarios, refuerzo de terminología |
| 09 | Editor de estructura | Configs, validaciones, queries — cualquier cosa que se "construye" |
| 10 | Tabla comparativa | Comparación de N opciones con múltiples atributos |
| 11 | Mapa conceptual | Relaciones no lineales entre múltiples conceptos |
| 12 | Visualizador de joins | Operaciones de conjuntos: SQL joins, unión/intersección |
| 13 | Wizard de pasos | Tutoriales de configuración con validación entre pasos |

## Esquema de datos por patrón

### 01 — Flujo por etapas
```js
{
  titulo: "string",
  badge: "string",
  lede: "string",
  etapas: [{ titulo: "string", descripcion: "string", icono: "svgString" }]
}
```

### 02 — Hotspots arquitectura
```js
{
  titulo, badge, lede,
  nodos: [{ id, nombre, subtitulo, x, y, w, h, color: "blue|purple|teal|amber|coral", info: { descripcion, ruta }}],
  conexiones: [{ desde: "nodeId", hasta: "nodeId", estilo: "solid|dashed" }]
}
```

### 03 — Scrubber temporal
```js
{
  titulo, badge, lede,
  etapas: [{ nombre, tiempoMs: number, descripcion }]
}
```

### 04 — Pipeline arrastrable
```js
{
  titulo, badge, lede,
  bloques: [{ id, nombre, icono: "svgString" }]
}
```

### 05 — Árbol de decisión
```js
{
  titulo, badge, lede,
  nodos: [{ id, pregunta, padre, rama: "yes|no", etiqueta }],
  hojas: [{ id, resultado, tipo: "ok|warn|no", padre, rama, descripcion }]
}
```

### 06 — Capas conmutables
```js
{
  titulo, badge, lede,
  capas: [{ id, titulo, subtitulo, color: "blue|purple|teal|amber|coral", descripcion, modulo }]
}
```

### 07 — Antes/después
```js
{
  titulo, badge, lede,
  antesSvg: "svgString",
  despuesSvg: "svgString",
  etiquetaAntes: "string",
  etiquetaDespues: "string"
}
```

### 08 — Matching término-definición
```js
{
  titulo, badge, lede,
  pares: [{ termino, definicion }]
}
```

### 09 — Editor de estructura
```js
{
  titulo, badge, lede,
  campos: [{ id, tipo: "text|number|select|checkbox", label, defecto, opciones? }],
  plantilla: "string con {placeholders}"
}
```

### 10 — Tabla comparativa
```js
{
  titulo, badge, lede,
  columnas: [{ key, label, tipo: "text|num|pill" }],
  filas: [{ ...campos, _detalle: { descripcion, codigo }}]
}
```

### 11 — Mapa conceptual
```js
{
  titulo, badge, lede,
  nodos: [{ id, titulo, x, y, color: "blue|purple|teal|amber|coral|gray", descripcion }],
  aristas: [{ desde, hasta, etiqueta }]
}
```

### 12 — Visualizador de joins
```js
{
  titulo, badge, lede,
  tablaA: { nombre, registros: [{...}] },
  tablaB: { nombre, registros: [{...}] },
  llaveA: "campo", llaveB: "campo",
  joinPorDefecto: "inner|left|right|full|cross"
}
```

### 13 — Wizard de pasos
```js
{
  titulo, badge, lede,
  pasos: [{
    titulo, descripcion,
    campos: [{ id, tipo, label, defecto, validar: "regex|required|range" }]
  }]
}
```

## Convenciones técnicas comunes

- **IIFE por widget**: cada `<script>` está envuelto en `(function(){ ... })()` para evitar colisiones entre múltiples widgets en la misma página.
- **IDs únicos**: si la skill genera múltiples widgets del mismo tipo en una página, debe prefijar los IDs con el slug del concepto (ej. `flow-imports-`, `flow-exports-`).
- **HTML5 Drag & Drop** para arrastrar chips/cards entre zonas (patrones 04, 08).
- **Pointer Events** para arrastrar elementos dentro de SVG o sliders continuos (patrones 03, 07).
- **Highlight-and-dim** para exploración no destructiva (patrones 05, 11, 12).
- **Estado interno en objeto plano** sin frameworks reactivos (patrón 13).
- **Renderizado declarativo** desde un array de datos en cada widget — cambiar los datos basta para tener un widget nuevo.
