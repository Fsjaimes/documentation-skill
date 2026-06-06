# Sección 1 — Resumen funcional

> **Para quién**: Product, negocio, nuevos miembros del equipo, stakeholders.
> **Pregunta que responde**: "¿Qué hace este sistema y por qué existe?"

## Bloques obligatorios

Cada uno de estos bloques debe aparecer (en este orden) salvo que no aplique:

### 1.1 Objetivo del producto
- **Formato**: párrafo corto (máx. 3 frases) + lista de 3–5 outcomes clave.
- **Visual**: arranca con badge "Resumen funcional", título grande con palabra clave en turquesa.
- **Ejemplo de título**: `Documentación del módulo de` + `Inventarios` (turquesa).

### 1.2 Problema que resuelve
- **Formato**: 1 párrafo describiendo el dolor antes del sistema + 1 callout turquesa con la decisión.
- **Visual**: `<div class="callout">` con título "Por qué construimos esto" y cuerpo breve.

### 1.3 Usuarios principales
- **Formato**: `card-grid` con una card por rol. Cada card: nombre del rol + 1 frase de su objetivo en el sistema.
- **Mínimo**: 2 cards. **Máximo**: 6.
- **Ejemplo**:
  ```
  Card 1: Operador de bodega
  "Registra entradas y salidas de mercancía en tiempo real."

  Card 2: Supervisor
  "Aprueba ajustes de inventario y revisa reportes diarios."
  ```

### 1.4 Funcionalidades principales
- **Formato**: tabla `doc-table` o `card-grid`.
- **Columnas (si tabla)**: Funcionalidad | Para quién | Resultado esperado.
- **Si son más de 8**: agrupar por categoría (ej. "Movimientos", "Reportes", "Configuración").

### 1.5 Reglas de negocio
- **Formato**: tabla `doc-table`.
- **Columnas**: Regla | Cuándo aplica | Consecuencia si se rompe.
- **Cada regla**: redactada como afirmación clara ("Un producto no puede tener stock negativo").

### 1.6 Flujos importantes del usuario
- **Formato obligatorio**: diagrama de flujo `.flow` con tarjetas numeradas + descripción debajo.
- **Por cada flujo crítico**: una `figure` con label `FLUJO 1`, `FLUJO 2`, etc.
- **Mínimo 1 flujo**, idealmente 2–4.
- **Cada paso**: número en círculo turquesa + título corto + descripción breve.

### 1.7 Casos de uso
- **Formato**: lista de casos en `card-grid` o tabla.
- **Cada caso**: actor → acción → resultado.
- **Estilo de redacción**: "Como [rol], necesito [acción] para [beneficio]" o equivalente narrativo.

### 1.8 Restricciones y límites conocidos
- **Formato**: callouts (uno por restricción) o tabla.
- **Para cada restricción**: qué es, por qué existe, qué hacer si se necesita superarla.
- **Ejemplos**: "El sistema soporta hasta 10 000 SKU por bodega", "No procesa más de 50 movimientos/seg sin degradación".

## Métricas opcionales

Si el módulo tiene cifras representativas (volumen, usuarios concurrentes, transacciones/día), incluir `metric-grid` al inicio de la sección:

```html
<div class="metric-grid">
  <div class="metric"><div class="metric__value">10K+</div><div class="metric__label">SKU gestionados</div></div>
  <div class="metric"><div class="metric__value">50/s</div><div class="metric__label">Movimientos pico</div></div>
  <div class="metric"><div class="metric__value">99.9%</div><div class="metric__label">Disponibilidad objetivo</div></div>
</div>
```

## Tono específico de esta sección

- Orientado a **negocio**, no a implementación.
- Cero referencias a clases, librerías o lenguajes (eso va en la sección técnica).
- Usar analogías de la vida diaria cuando explique procesos complejos.
