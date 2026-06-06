# Sección 4 — Soporte

> **Para quién**: Equipo de soporte funcional, mesa de ayuda, QA, analistas que NO necesariamente leen código.
> **Pregunta que responde**: "El sistema falló o un usuario reportó algo, ¿qué hago?"

> ⚠ **EXCEPCIÓN OBLIGATORIA AL VOCABULARIO TÉCNICO**: esta sección se escribe en lenguaje accesible. Cada término técnico se traduce. Nada de jerga sin explicación.

## Bloques obligatorios

### 4.1 Errores comunes
- **Formato**: tabla `doc-table` con columnas `Código / Mensaje | Qué significa en palabras simples | Qué hacer | Cuándo escalar a desarrollo`.
- **Para cada error**:
  - Mostrar el mensaje exacto como aparece en pantalla/log.
  - Traducirlo: "Esto significa que el sistema no pudo conectar con la base de datos".
  - Procedimiento paso a paso.
  - Criterio claro de escalación.
- **Mínimo 5 errores** documentados (los más frecuentes en producción).

### 4.2 Logs importantes
- **Formato**: tabla con `Qué buscar | Dónde verlo | Cómo se ve | Qué indica`.
- **Explicar dónde están los logs** sin asumir que el lector sabe SSH o Kibana de memoria — incluir links o pasos visuales.
- **Cada log importante**: un ejemplo de cómo se ve (`<pre>`), qué palabras clave buscar.
- **Ejemplo**:
  > **Qué buscar**: `payment_failed`
  > **Dónde**: Kibana → índice `app-prod-*`
  > **Cómo se ve**: `{"level": "error", "event": "payment_failed", "user_id": "..."}`
  > **Qué indica**: un cobro fue rechazado por el procesador de pagos.

### 4.3 Alertas
- **Formato**: tabla `doc-table` con `Alerta | Severidad | Qué la dispara | Qué hacer primero`.
- **Severidad**: usar badges `Crítica` (rojo) / `Alta` (warning) / `Media` (neutral).
- **Explicar dónde llega** (Slack, PagerDuty, correo).
- **Qué NO hacer**: incluir si aplica ("no reiniciar antes de tomar un snapshot del estado").

### 4.4 Métricas para vigilar
- **Formato**: `metric-grid` mostrando las métricas clave + tabla explicativa.
- **Por métrica**: nombre, qué mide, valor normal, valor que prende alarmas, dónde verla.
- **Ejemplos**: latencia p95, tasa de error 5xx, requests/min, conexiones activas a BD, espacio en disco.

### 4.5 Cómo reiniciar servicios
- **Formato**: pasos numerados con bloques de comandos + `callout--warning` con advertencias.
- **Para cada servicio crítico**:
  1. Cómo verificar el estado actual.
  2. Cómo reiniciarlo (comando exacto).
  3. Cómo verificar que volvió a estar sano.
  4. Qué hacer si NO arranca.
- **Incluir un diagrama de flujo** del procedimiento.

### 4.6 Cómo revisar el estado del sistema
- **Formato**: checklist visual (lista con `card-grid` o tabla).
- **Cada item del checklist**: qué revisar, comando o URL, qué significa "sano".
- **Ejemplo de checklist**:
  - [ ] Endpoint `/health` responde 200.
  - [ ] Cola de mensajes con <100 items pendientes.
  - [ ] Conexión a BD activa (revisar pool).
  - [ ] Uso de CPU <70% en cada nodo.

### 4.7 Contactos responsables
- **Formato**: tabla `doc-table` con `Rol | Persona / equipo | Cuándo contactar | Cómo`.
- **Roles típicos**: oncall actual, lead técnico, product owner, DevOps, proveedor externo (ej. payment gateway).
- **Incluir canales**: Slack channel, email, teléfono on-call.
- **Mantener fechas de validación** ("contactos actualizados al YYYY-MM-DD").

### 4.8 Procedimientos ante incidentes
- **Formato obligatorio**: diagrama de flujo + tabla de severidades.
- **Diagrama**: árbol de decisión. "¿Es crítico? → SÍ: activar protocolo P1 / NO: ¿afecta a usuarios? → ...".
- **Tabla**: Severidad (P1/P2/P3) | Definición | Tiempo de respuesta | Quién se activa | Comunicación.
- **Incluir plantilla de comunicación**:
  > "Estamos investigando un incidente en [servicio]. Próxima actualización en X minutos."

### 4.9 Preguntas frecuentes
- **Formato**: acordeón visual o lista de `card`s.
- **Mínimo 8 FAQs** cubriendo dudas reales que el equipo de soporte enfrenta.
- **Cada FAQ**: pregunta exacta como la formularía el usuario + respuesta directa + paso siguiente si aplica.

## Tono específico (REGLA DURA)

- **Cero jerga sin traducir**. "HTTP 503" → "el servicio no está disponible temporalmente; no es culpa del usuario".
- **Frases cortas**. Máximo 18 palabras por frase.
- **Imperativo claro**. "Abre la consola", "haz clic en", "copia el ID del usuario".
- **Asumir cero contexto técnico**. El analista de soporte puede no saber qué es un endpoint o un container.
- **Visuales obligatorios**: capturas, diagramas de flujo, callouts. Mucha estructura visual.

## Recordatorio visual

Esta sección DEBE tener:
- Al menos 2 tablas (errores + alertas).
- Al menos 1 diagrama de flujo (incidentes o reinicio).
- Al menos 1 callout de advertencia (cosa peligrosa o irreversible).
- 1 `metric-grid` (estado normal del sistema).
