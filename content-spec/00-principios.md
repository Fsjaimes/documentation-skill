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

## Diagramas

Toda decisión arquitectónica importante, flujo de usuario o secuencia de eventos DEBE acompañarse de un diagrama:

- **HTML**: SVG inline con conectores `.connector` (grises, 1.5px, bezier).
- **Markdown**: Mermaid con tema turquesa.

Los diagramas NO pueden estar recargados. Cada paso una caja, cada relación una flecha simple. Si un diagrama tiene más de 9 cajas, dividirlo en dos.

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

- [ ] ¿Hay sidebar a la derecha? ¿Nada flotando a la izquierda o arriba?
- [ ] ¿El acento es turquesa `#0097A7`? ¿Ningún coral, púrpura o naranja?
- [ ] ¿La tipografía es sans-serif en todo? (cero serif)
- [ ] ¿Cada sección abre con badge + título + subtítulo?
- [ ] ¿Hay al menos 1 diagrama o figura por sección no trivial?
- [ ] ¿La sección de soporte se entiende sin background técnico?
- [ ] ¿Ningún muro de texto de más de 3 párrafos seguidos?
- [ ] ¿Ningún emoji?
