# Principios globales de redacción

Reglas que aplican a TODA la documentación generada por esta skill, en cualquier sección y en cualquier formato (HTML o Markdown).

## Audiencia

El lector ideal NO es el desarrollador original del sistema. Puede ser:

- Un nuevo miembro del equipo de desarrollo que llega 6 meses después.
- Un analista de soporte que nunca leyó el código.
- Un PM o stakeholder que necesita entender qué hace el módulo.
- Un usuario final no técnico (solo en la sección manual de usuario).

**Regla**: la documentación debe ser autosuficiente. Nadie debe necesitar "preguntarle al que lo hizo" para entender, usar, mantener o evolucionar el sistema.

## Voz y tono

- **Persona**: tercera persona impersonal ("el sistema permite…", "el módulo expone…") o segunda persona invitando a la acción ("revisa el log", "ejecuta el comando").
- **Tiempo**: presente ("el servicio responde con un JSON…").
- **Tono**: profesional, claro, directo. Ni acartonado ni casual.
- **Densidad**: frases cortas. Si una frase pasa de 25 palabras, dividirla.
- **Casing**: sentence case en titulares y etiquetas. Solo MAYÚSCULAS para labels metadata muy puntuales (`AUTOR`, `VERSIÓN`, `PUBLICADO`) con `letter-spacing`.

## Estructura por sección

Toda sección DEBE iniciar con esta secuencia visual:

```
[Badge pequeño turquesa]   ← Tipo de sección (ej: "Soporte", "Técnica")
[Título grande con palabra clave en turquesa]
[Subtítulo en gris medio explicando qué encontrarás aquí]
[Contenido en cards / tablas / callouts / diagramas]
```

Nunca empezar una sección con un párrafo de texto plano.

## Cero muros de texto

Si tienes más de 3 párrafos seguidos sin un elemento visual, **rompe el bloque** con uno de:

- Una `card` con un punto clave.
- Una `tabla` con datos comparables.
- Un `callout` con una nota o decisión.
- Un diagrama de flujo o figura.
- Una `metric` cuando hay números (latencia objetivo, usuarios concurrentes, etc.).

## Consistencia visual en card-grids

**Regla dura**: dentro de un mismo `.card-grid`, todas las cards deben usar la **misma variante** de borde.

Variantes disponibles:
- `card` — sin acento.
- `card card--accent` — línea turquesa superior.
- `card card--accent-left` — línea turquesa izquierda.

**Por qué importa**: si en un grid hablamos del mismo tema (ej. "Usuarios principales", "Por qué importan", "Funcionalidades"), mezclar acentos arriba y a la izquierda da impresión de descuido o de que algunas cards son más importantes que otras sin razón. La consistencia comunica "estos elementos son pares".

Puedes cambiar de variante entre grids distintos (grupos conceptuales separados). Lo que no se permite es mezclar dentro del mismo grupo.

```html
<!-- ✅ CORRECTO: todas con accent superior -->
<div class="card-grid">
  <article class="card card--accent">...</article>
  <article class="card card--accent">...</article>
  <article class="card card--accent">...</article>
</div>

<!-- ❌ INCORRECTO: variantes mezcladas en el mismo grupo -->
<div class="card-grid">
  <article class="card card--accent">...</article>
  <article class="card">...</article>
  <article class="card card--accent-left">...</article>
</div>
```

## Métricas con datos reales únicamente

`metric-grid` solo se usa cuando representa **datos verificables del sistema documentado**: capacidad, latencia, volumen, KPIs, SLAs, etc.

**Prohibido**: usar métricas como decoración tipo "dashboard" en guías conceptuales o tutoriales cuando no hay cifras reales relevantes. Inventar números genéricos ("10+ comandos disponibles", "0 aprobaciones para publicar") empobrece el diseño porque señala que el autor llenó espacio con relleno.

**Antes de poner una métrica, pregúntate**: ¿este número viene de un dato real del sistema, un benchmark medido o un objetivo concreto? Si la respuesta es no, omítela y reemplaza con otro componente (cards, callout, tabla).

## Analogías

El lector objetivo es visual. Cada concepto técnico que pueda confundir debe acompañarse de:

1. **Una analogía cotidiana** ("piensa en esto como una sala de espera donde…").
2. **Un diagrama** que muestre el flujo.
3. **Un ejemplo concreto** ("si un usuario hace X, el sistema responde Y").

Ejemplos de buenas analogías:
- Cola de mensajes → "fila de tickets en un banco; cada ticket se procesa en orden".
- Cache → "tener la calculadora a mano para no tener que abrir Excel cada vez".
- Webhook → "como dejar tu número para que te llamen cuando el pedido esté listo".

## Vocabulario técnico

Usar el término técnico **siempre que sea preciso**, pero acompañarlo de su definición la primera vez que aparece:

> "El sistema usa un *event bus* (un canal central donde los módulos publican mensajes y otros se suscriben para escucharlos), construido sobre RabbitMQ."

## Diagramas: uno por cada concepto, flujo o transformación

**Regla dura**: cualquier idea que se pueda expresar como una secuencia "X → Y → Z" DEBE tener un diagrama visual al lado. No basta con describirla en texto. El lector objetivo aprende mejor con gráficos.

Casos típicos que **siempre** requieren diagrama:

| Tipo de contenido | Diagrama esperado |
|---|---|
| Concepto que involucra un flujo | Cajas conectadas mostrando la cadena de pasos |
| Arquitectura de un servicio | Componentes + integraciones |
| Comando que dispara una cadena de procesos | Comando → Job → Job → Resultado (ver ejemplo abajo) |
| Procedimiento de soporte / incidente | Árbol de decisión o flowchart |
| Modelo de datos | ERD simplificado |
| Despliegue | Mapa de infraestructura |
| Estados de un proceso | Máquina de estados |

**Ejemplo concreto** — si documentamos un comando Laravel que ejecuta un job que lee un JSON, luego otro job que procesa por chunks, y finalmente un importer con lógica de módulo, el diagrama debe verse así:

```
[Comando]  →  [Job: lectura JSON]  →  [Job: procesa chunk]  →  [Importer: lógica de módulo]
```

Cuatro cajas, tres flechas, etiquetas cortas. Nada más.

**Reglas de composición**:

- **HTML**: SVG inline con conectores `.connector` (grises, 1.5px, curvas bezier). Cajas con borde hairline (`--border-hair`) o fondo turquesa pálido para destacar el paso clave.
- **Markdown**: Mermaid con tema turquesa.
- **Densidad**: máximo 9 cajas por diagrama. Si pasa de eso, dividir en dos diagramas.
- **Etiquetas**: 1–3 palabras por caja. Detalles van en la caption, no en la caja.
- **Interactividad**: opcional. Solo añadir slider/hover si aporta entendimiento real; no decorar por decorar.

**No basta con un `flow` de tarjetas numeradas para representar un concepto**. El `flow` numerado sirve para listas de pasos secuenciales. Un concepto (qué es algo, cómo se conecta con su ecosistema) pide un diagrama de cajas y flechas SVG.

## Sección de soporte: lenguaje accesible

La sección de soporte es la **excepción obligatoria** al vocabulario técnico libre. En esa sección:

- Cada término técnico se traduce ("HTTP 500" → "error interno del servidor — algo falló en el sistema, no en lo que el usuario hizo").
- Los procedimientos se presentan paso a paso con números.
- Cada error común tiene: **qué pasó** / **por qué pasó** / **qué hacer**.
- Los logs se explican como "qué buscar y cómo se ve".

## Idioma

- Español por defecto (es-CO).
- Inglés solo si el usuario lo pide explícitamente o si el proyecto es internacional.
- Nombres técnicos (clases, funciones, librerías) NUNCA se traducen.

## Lo que NO se documenta

- Información sensible: credenciales reales, IPs internas no enmascaradas, secretos.
- Información obsoleta sin marcar como `[DEPRECATED]`.
- Detalles de implementación tan triviales que el código los muestra mejor.

## Verificación antes de entregar

Antes de devolver el output al usuario, validar:

- [ ] ¿Hay sidebar a la derecha, pegada con poco margen? ¿Nada flotando a la izquierda o arriba?
- [ ] ¿El acento es turquesa `#0097A7`? ¿Ningún coral, púrpura o naranja?
- [ ] ¿La tipografía es sans-serif en todo? (cero serif)
- [ ] ¿Cada sección abre con badge + título + subtítulo?
- [ ] ¿Cada concepto/flujo/transformación tiene un diagrama SVG al lado?
- [ ] ¿Dentro de cada `card-grid`, todas las cards usan la misma variante de acento?
- [ ] ¿Las métricas representan datos reales del sistema (no relleno tipo dashboard)?
- [ ] ¿La sección de soporte se entiende sin background técnico?
- [ ] ¿Ningún muro de texto de más de 3 párrafos seguidos?
- [ ] ¿Ningún emoji?
