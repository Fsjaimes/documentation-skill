# Sección 5 — Manual de usuario

> **Para quién**: Usuarios finales no técnicos del sistema.
> **Pregunta que responde**: "¿Cómo uso esta aplicación para hacer mi trabajo?"
> **Aplica solo si**: el sistema tiene una interfaz para usuarios finales (no aplica a APIs internas, servicios backend, librerías).

## Bloques obligatorios

### 5.1 Cómo iniciar sesión
- **Formato**: pasos numerados con captura por paso.
- **Cada paso**: 1 frase + 1 captura.
- **Cubrir**:
  - URL de acceso.
  - Pantalla de login (captura).
  - Qué hacer si olvidó la contraseña.
  - Qué hacer si la cuenta está bloqueada.
- **Mostrar la pantalla de éxito** después del login.

### 5.2 Cómo usar las funciones principales
- **Formato**: guía paso a paso por cada función importante.
- **Estructura repetible**:
  1. **Qué vas a hacer**: 1 frase.
  2. **Cuándo lo usarías**: contexto de uso real.
  3. **Pasos**: numerados, cada uno con captura cuando ayude.
  4. **Resultado esperado**: cómo se ve la pantalla cuando terminó bien.
- **Acompañar con un diagrama de flujo simple** mostrando el recorrido en la UI.

### 5.3 Capturas de pantalla
- **Cada captura**: con borde hairline (`figure` o `card` con borde).
- **Cuando se refiere a una zona específica**: usar anotación con número o flecha turquesa.
- **Caption corto**: 1 frase, sentence case.
- **NO incluir datos sensibles** en capturas (usar nombres ficticios, IDs anonimizados).

### 5.4 Explicación de errores comunes
- **Formato**: tabla con `Mensaje que ves | Por qué aparece | Qué hacer`.
- **Distinto de la tabla de errores de soporte**: aquí el lector es el usuario final, no soporte.
- **Tono**: "Tranquilo, esto significa que…" — empático, no acusatorio.
- **Cada error**: incluir captura del mensaje real.

### 5.5 Preguntas frecuentes
- **Formato**: acordeón o lista de `card`s.
- **Preguntas reales de usuarios**, no inventadas.
- **Respuestas directas** sin abrir nuevas preguntas.

### 5.6 Guías paso a paso para tareas comunes
- **Formato**: cada guía como una mini-sección con `figure` numeradas.
- **Cada guía**: título claro ("Cómo hacer un movimiento de inventario").
- **Estructura**:
  - 1. Antes de empezar: qué necesitas tener listo.
  - 2. Pasos numerados con capturas.
  - 3. Cómo confirmar que se hizo bien.
  - 4. Qué hacer si algo sale mal.

## Pautas visuales

- **Capturas**: máximo 1200px de ancho, comprimidas, con borde hairline.
- **Anotaciones sobre capturas**: círculos turquesa numerados, flechas turquesa simples.
- **Diagramas de recorrido**: minimal, máximo 5 pasos por flujo.
- **NO usar términos técnicos** sin glosario.

## Glosario obligatorio

Al final de esta sección, incluir un glosario breve traduciendo:
- Términos técnicos que aparecen en la app ("dashboard" = "tablero", "log" = "registro de actividad").
- Acrónimos del dominio.
- Términos propios del sistema.

## Tono específico (REGLA DURA)

- **Segunda persona** ("haz clic", "selecciona").
- **Tono amable** sin ser condescendiente.
- **Cero jerga**.
- **Si una función requiere conocimiento previo**, lincar a la sección que lo explica.
- **Reconocer cuando algo es confuso**: "Esta pantalla puede parecer compleja al inicio; aquí te guiamos paso a paso".

## Cuándo NO incluir esta sección

- Es una API o servicio sin UI.
- Es una librería para devs.
- Es infraestructura backend.
- El usuario lo dice explícitamente.

Si el sistema tiene UI mínima (un panel admin para 2 usuarios técnicos), incluir una versión reducida con solo: login + 2–3 tareas principales + glosario corto.
