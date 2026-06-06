# Sección 2 — Documentación técnica

> **Para quién**: Desarrolladores que entran al proyecto y necesitan entender cómo está construido.
> **Pregunta que responde**: "¿Cómo está armado el sistema y por qué se tomaron estas decisiones?"

## Bloques obligatorios

### 2.1 Arquitectura general
- **Formato**: párrafo introductorio + diagrama de arquitectura como `figure`.
- **Diagrama**: SVG inline en HTML, o Mermaid (`graph TD` / `graph LR`) en Markdown.
- **Estilo de diagrama**: cajas con borde hairline, flechas grises 1.5px, etiquetas cortas, máximo 9 cajas. Si es más complejo, dividir en sub-diagramas.
- **Acompañar con tabla `Componente | Responsabilidad | Tecnología`**.

### 2.2 Stack tecnológico
- **Formato**: tabla `doc-table` con columnas `Capa | Tecnología | Versión | Motivo de la elección`.
- **Capas típicas**: Frontend, Backend, Base de datos, Cola/Eventos, Cache, Infraestructura, Observabilidad.
- **Importante**: incluir el `Motivo` — qué problema resuelve cada elección. No es "Postgres porque sí".

### 2.3 Lenguajes, frameworks y librerías
- **Formato**: agrupar por capa, dentro de cada una un `card-grid` o tabla con la lib + propósito.
- **Solo librerías que importan**, no dependencias triviales. Si hay >15, listar las críticas y enlazar al `package.json`/`requirements.txt`.

### 2.4 Estructura del proyecto
- **Formato**: árbol de carpetas en `<pre>` + tabla explicativa por carpeta principal.
- **Si el repo está disponible**: extraer la estructura real.
- **Cada carpeta importante**: una línea con "para qué sirve".

### 2.5 Patrones de diseño usados
- **Formato**: `card-grid`, una card por patrón.
- **Cada card**: nombre del patrón + dónde se usa + por qué.
- **Si se aplica de forma poco ortodoxa**: callout explicando la variación.

### 2.6 Decisiones técnicas importantes (ADRs)
- **Formato**: lista de decisiones, una por sección o card.
- **Cada decisión**: `Decisión` / `Contexto` / `Alternativas consideradas` / `Consecuencias`.
- **Estilo**: similar a un ADR (Architecture Decision Record) ligero.

### 2.7 Servicios internos y externos
- **Formato**: tabla `doc-table` + diagrama de integraciones como `figure`.
- **Columnas tabla**: Servicio | Tipo (interno/externo) | Comunicación (REST/gRPC/eventos) | Propósito | SLA / contacto.
- **Diagrama de integraciones**: el sistema en el centro, servicios externos alrededor, flechas etiquetadas.

### 2.8 Dependencias
- **Formato**: tabla o lista, separar críticas vs auxiliares.
- **Indicar**: versión, licencia, si requiere mantenimiento (ej. lib sin actualizaciones recientes).

### 2.9 Configuraciones relevantes
- **Formato**: tabla `doc-table` con `Configuración | Dónde se define | Valor por defecto | Cuándo cambiarla`.
- **No incluir secretos reales**, usar placeholders (`<API_KEY>`).

## Diagramas obligatorios

Cada uno como `figure` con label `DIAGRAMA N` (en HTML, dentro de `.figure__label`):

### Diagrama de arquitectura
- Componentes principales + flujos de datos + entradas/salidas.
- Tipo: `graph LR` en Mermaid o SVG cajas+flechas en HTML.

### Diagrama de base de datos
- ERD simplificado (tablas + relaciones, sin todos los campos).
- Si el modelo tiene >15 tablas, mostrar solo las core + enlazar al ERD completo.
- En Markdown: usar Mermaid `erDiagram`.

### Diagrama de despliegue
- Infraestructura: servidores, contenedores, regiones, balanceadores, CDN.
- Mostrar dónde corre qué.
- Mermaid `graph TB` o SVG.

### Diagramas de flujo para procesos importantes
- Uno por proceso crítico (ej. autenticación, transacción, sincronización).
- Mermaid `sequenceDiagram` cuando hay actores.

## Pautas para diagramas

- **Cajas**: borde hairline gris (`--ink-100`), fondo blanco o turquesa muy pálido (`--accent-50`).
- **Flechas**: gris medio (`--ink-300`), 1.5px, simples con punta abierta.
- **Etiquetas en flechas**: 1–3 palabras máximo.
- **Color en Mermaid**: tema personalizado con `init` directive (ejemplo en `templates/markdown/shell.md`).
- **Nunca**: gradientes, sombras, colores múltiples por caja, animaciones decorativas.

## Tono específico

- Vocabulario técnico libre (esta sección es para devs).
- Pero siempre explica la **razón** de cada elección, no solo el "qué".
- Evita "se decidió usar X" — di "elegimos X porque Y".
