# Documentación del módulo de **Inventarios**

> Gestión centralizada de stock multibodega con control de movimientos en tiempo real, reglas de aprobación y trazabilidad por SKU.

<table>
<tr>
  <td><strong>10K+</strong><br><sub>SKU gestionados</sub></td>
  <td><strong>50/s</strong><br><sub>Movimientos pico</sub></td>
  <td><strong>99.9%</strong><br><sub>Disponibilidad objetivo</sub></td>
  <td><strong>24/7</strong><br><sub>Soporte</sub></td>
</tr>
</table>

---

## Contenido

1. [Resumen funcional](#resumen-funcional)
2. [Documentación técnica](#documentación-técnica)
3. [Instalación y configuración](#instalación-y-configuración)
4. [Soporte](#soporte)
5. [Manual de usuario](#manual-de-usuario)

---

## Resumen funcional

`Resumen funcional`

### Qué hace el módulo

El módulo de Inventarios mantiene una vista única del stock disponible en todas las bodegas. Reemplaza el conteo manual y las hojas Excel descentralizadas con un sistema en tiempo real conectado a los lectores de código de barras.

> **Por qué construimos esto**
> Antes del módulo, cada bodega gestionaba su stock en archivos Excel locales. La conciliación tardaba 3 días por mes y arrastraba errores que afectaban la planeación de compras.

### Usuarios principales

| Rol | Objetivo en el sistema |
|---|---|
| Operador de bodega | Registra entradas y salidas con lector de código de barras |
| Supervisor | Aprueba ajustes mayores a 100 unidades y revisa el reporte diario |
| Analista de compras | Consulta niveles de stock para decidir reposiciones |
| Auditor | Accede al historial de movimientos para verificar trazabilidad |

### Reglas de negocio

| Regla | Cuándo aplica | Consecuencia si se rompe |
|---|---|---|
| Un producto no puede tener stock negativo | En cada salida | El movimiento se rechaza |
| Ajustes >100 unidades requieren aprobación | Al guardar un ajuste manual | Queda en estado "Pendiente" |
| Cada movimiento debe tener responsable | Siempre | No se puede registrar sin sesión activa |
| Cierre diario bloquea movimientos 12h | Al ejecutar cierre | Movimientos rechazados en ese período |

### Flujo principal: registrar un movimiento

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF','fontFamily':'Inter, sans-serif'}}}%%
flowchart LR
  A[Escanear SKU] --> B[Validar stock]
  B --> C{¿Disponible?}
  C -- Sí --> D[Registrar movimiento]
  C -- No --> E[Notificar al supervisor]
  D --> F[Publicar evento]
```

### Restricciones conocidas

> ⚠ **Capacidad máxima**
> El sistema soporta hasta 10 000 SKU activos por bodega y 50 movimientos/segundo. Por encima, hay degradación en latencia. Para más, contactar al equipo de arquitectura.

---

## Documentación técnica

`Técnica`

### Arquitectura

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF','fontFamily':'Inter, sans-serif'}}}%%
flowchart LR
  Tablet[Tablet bodega] --> API[Inventarios API]
  API --> DB[(Postgres)]
  API --> Cache[(Redis cache)]
  API --> MQ[/RabbitMQ/]
  MQ --> Compras[Compras]
  MQ --> Ventas[Ventas]
```

### Stack tecnológico

| Capa | Tecnología | Versión | Motivo |
|---|---|---|---|
| API | Node.js + Fastify | 20.x | Throughput alto, TypeScript nativo |
| BD | PostgreSQL | 15.x | Transacciones fuertes + JSONB para metadatos |
| Cache | Redis | 7.x | Latencia <1ms al validar stock |
| Eventos | RabbitMQ | 3.12 | Routing por tópico para múltiples consumidores |
| Observabilidad | OpenTelemetry + Grafana | — | Trazas distribuidas |

### Decisión técnica: ADR-007 — Cache de stock en Redis

> **Contexto**: validar stock contra Postgres en cada movimiento generaba latencias de 80–120ms en hora pico.
>
> **Decisión**: mantener el stock disponible en Redis con TTL de 5s y revalidar en escritura.
>
> **Consecuencia**: latencia bajó a 3–8ms, a cambio de una ventana de 5s donde una lectura puede estar desactualizada.

### Modelo de datos

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF'}}}%%
erDiagram
  PRODUCTO ||--o{ MOVIMIENTO : tiene
  BODEGA   ||--o{ MOVIMIENTO : ocurre_en
  USUARIO  ||--o{ MOVIMIENTO : registra
  PRODUCTO {
    string sku PK
    string nombre
    int stock_minimo
  }
  MOVIMIENTO {
    uuid id PK
    string sku FK
    int cantidad
    string motivo
    timestamp creado_en
  }
```

---

## Instalación y configuración

`Instalación`

### Requisitos previos

| Herramienta | Versión mínima | Verificar |
|---|---|---|
| Node.js | 20.x | `node --version` |
| npm | 10.x | `npm --version` |
| Postgres | 15.x | `psql --version` |
| Redis | 7.x | `redis-cli --version` |

### Variables de entorno

| Variable | Requerida | Descripción | Ejemplo |
|---|---|---|---|
| `DATABASE_URL` | Sí | URL Postgres | `postgres://user:pass@localhost:5432/inventario` |
| `REDIS_URL` | Sí | URL del cache | `redis://localhost:6379` |
| `RABBITMQ_URL` | Sí | URL del broker | `amqp://localhost:5672` |
| `JWT_SECRET` | Sí | Secreto para tokens | `<cambiar-en-prod>` |
| `LOG_LEVEL` | No | Nivel de log | `info` |

### Pasos

```bash
# 1. Clonar
git clone https://github.com/sodeker/inventarios.git
cd inventarios

# 2. Instalar dependencias
npm install

# 3. Variables
cp .env.example .env

# 4. Migraciones
npm run db:migrate

# 5. Datos seed
npm run db:seed

# 6. Levantar
npm run dev
```

> ✓ **Verifica que arrancó**
> Abre `http://localhost:3000/health`. Debe responder `{"status":"ok","version":"1.x.x"}`.

### Problemas comunes

| Síntoma | Causa | Solución |
|---|---|---|
| `EADDRINUSE :3000` | Puerto ocupado | `lsof -ti:3000 \| xargs kill` |
| `ECONNREFUSED 5432` | Postgres no corre | `brew services start postgresql` |
| `Cannot find module` | Faltó npm install | `rm -rf node_modules && npm install` |
| Login devuelve 401 siempre | `JWT_SECRET` incorrecto | Copiar del vault de equipo |

---

## Soporte

`Soporte` · Lenguaje accesible

> 🛟 Esta sección está escrita para el equipo de soporte funcional. No requiere conocimientos técnicos.

### Estado normal del sistema

| Indicador | Valor normal |
|---|---|
| Latencia de respuesta | <300ms |
| Items en cola pendiente | <100 |
| Tasa de error | <1% |
| Disponibilidad mensual | >99% |

### Errores más frecuentes

| Mensaje | Qué significa | Qué hacer | Escalar si |
|---|---|---|---|
| `Stock insuficiente` | El producto no tiene unidades disponibles | Verificar inventario físico con el supervisor | Sistema dice "sí hay" pero el error sigue |
| `Movimiento duplicado` | Ya hay uno idéntico en los últimos 10s | Esperar 30s y reintentar | Persiste tras 2 minutos |
| `Error interno del servidor (500)` | Algo falló en el sistema | Captura + hora + ticket a desarrollo | Siempre — requiere desarrollo |
| `Sesión expirada` | Pasaron más de 8h desde el login | Pedir al usuario que vuelva a iniciar sesión | Aparece justo después de login |

### Procedimiento ante un incidente

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF'}}}%%
flowchart TD
  A[Llega el reporte] --> B{¿Afecta a usuarios ahora?}
  B -- Sí --> C[Severidad P1 - activar on-call]
  B -- No --> D[Crear ticket Jira]
  C --> E[Update en #incidentes cada 15 min]
  D --> F[Asignar a próximo sprint]
```

### Contactos

| Rol | Persona / equipo | Canal | Cuándo |
|---|---|---|---|
| On-call | Rotación semanal | PagerDuty | Incidente P1/P2 |
| Lead técnico | María Pérez | Slack `@maria` | Decisiones de arquitectura urgentes |
| Product owner | Juan Gómez | Slack `@juan` | Dudas funcionales |
| Proveedor pagos | Soporte Wompi | +57 1 555 1234 | Errores en flujo de cobro |

---

## Manual de usuario

`Manual de usuario`

### Iniciar sesión

1. Abre `https://inventarios.sodeker.co` en tu navegador.
2. Ingresa tu correo corporativo y contraseña.
3. Si olvidaste tu contraseña, haz clic en **"Recuperar contraseña"**.

### Registrar un movimiento de salida

1. En el menú lateral selecciona **Movimientos → Nuevo**.
2. Escanea el código de barras o escribe el nombre del producto.
3. Ingresa cantidad y motivo del movimiento.
4. Haz clic en **Guardar**. Verás un mensaje verde de confirmación.

### Glosario

- **SKU**: código único de cada producto (ej: `SKU-00123`).
- **Movimiento**: cualquier entrada o salida de mercancía.
- **Bodega**: ubicación física donde se almacena mercancía.
- **Ajuste**: movimiento para corregir diferencias entre el inventario físico y el del sistema.

---

<sub>Documentación generada con Sódeker Docs Skill · Estilo Sódeker (turquesa + minimalista) · Diagramas con tema personalizado Mermaid.</sub>
