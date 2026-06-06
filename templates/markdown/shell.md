<!--
  Plantilla base Sódeker Docs — Markdown
  Esta plantilla está pensada para vivir en `docs/` dentro de un repo Git.
  Render: GitHub, GitLab, MkDocs, Docusaurus, VS Code preview.
-->

<!-- Tema Mermaid: turquesa Sódeker. Pegar al inicio de TODOS los bloques mermaid. -->
<!-- ```mermaid
%%{init: {'theme':'base','themeVariables':{
  'primaryColor':'#E6F4F6',
  'primaryTextColor':'#0A0A0A',
  'primaryBorderColor':'#0097A7',
  'lineColor':'#9CA3AF',
  'secondaryColor':'#F7F9FB',
  'tertiaryColor':'#FFFFFF',
  'fontFamily':'Inter, sans-serif'
}}}%%
``` -->

# {{TITULO_PARTE_NEGRA}} **{{TITULO_PARTE_TURQUESA}}**

> {{LEDE_UNA_FRASE}}

<table>
<tr>
  <td><strong>50+</strong><br><sub>Procesos automatizados</sub></td>
  <td><strong>10+</strong><br><sub>Años de experiencia</sub></td>
  <td><strong>24/7</strong><br><sub>Soporte técnico</sub></td>
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

> Breve descripción del propósito.

### Usuarios principales

| Rol | Objetivo en el sistema |
|---|---|
| Operador de bodega | Registra entradas y salidas de mercancía. |
| Supervisor | Aprueba ajustes y revisa reportes. |

### Reglas de negocio

| Regla | Cuándo aplica | Consecuencia si se rompe |
|---|---|---|
| Un producto no puede tener stock negativo | En cada movimiento de salida | El movimiento se rechaza |
| Los ajustes >100 unidades requieren aprobación | Al guardar un ajuste manual | Queda en estado "Pendiente" |

### Flujo principal

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF','fontFamily':'Inter, sans-serif'}}}%%
flowchart LR
  A[Recepción] --> B[Validación]
  B --> C{¿Stock disponible?}
  C -- Sí --> D[Registrar movimiento]
  C -- No --> E[Notificar al supervisor]
  D --> F[Actualizar inventario]
```

> **Por qué construimos esto**
> Antes, el conteo se hacía en hojas Excel que no se sincronizaban entre bodegas.

---

## Documentación técnica

`Técnica`

### Arquitectura

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF','fontFamily':'Inter, sans-serif'}}}%%
flowchart LR
  Cliente[Cliente web] --> Gateway[API gateway]
  Gateway --> Ordenes[Servicio órdenes]
  Gateway --> Stock[Servicio stock]
  Stock --> DB[(Postgres)]
  Stock --> Cache[(Redis)]
```

### Stack

| Capa | Tecnología | Versión | Motivo |
|---|---|---|---|
| Frontend | React | 18.x | Stack del equipo, ecosystem maduro |
| Backend | Node.js + Express | 20.x | Soporte para fetch nativo, TypeScript |
| BD | PostgreSQL | 15.x | Soporte JSONB para metadatos flexibles |
| Cache | Redis | 7.x | Latencia <1ms para validar stock |

### Modelo de datos

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF'}}}%%
erDiagram
  PRODUCTO ||--o{ MOVIMIENTO : tiene
  BODEGA   ||--o{ MOVIMIENTO : ocurre_en
  USUARIO  ||--o{ MOVIMIENTO : registra
```

---

## Instalación y configuración

`Instalación`

### Requisitos previos

| Herramienta | Versión mínima | Verificar |
|---|---|---|
| Node.js | 20.x | `node --version` |
| Postgres | 15.x | `psql --version` |
| Redis | 7.x | `redis-cli --version` |

### Variables de entorno

| Variable | Requerida | Ejemplo |
|---|---|---|
| `DATABASE_URL` | Sí | `postgres://user:pass@localhost:5432/inventario` |
| `REDIS_URL` | Sí | `redis://localhost:6379` |
| `LOG_LEVEL` | No | `info` |

### Pasos

```bash
# 1. Clonar
git clone <repo>

# 2. Instalar dependencias
npm install

# 3. Copiar variables
cp .env.example .env

# 4. Correr migraciones
npm run db:migrate

# 5. Levantar el servicio
npm run dev
```

> **Verifica que arrancó bien**
> Abre `http://localhost:3000/health`. Debe responder `{"status":"ok"}`.

### Problemas comunes

| Síntoma | Causa probable | Solución |
|---|---|---|
| `EADDRINUSE :3000` | Puerto ocupado | `lsof -ti:3000 \| xargs kill` |
| `connection refused 5432` | Postgres no está corriendo | `brew services start postgresql` |
| `Cannot find module` | Faltó `npm install` | Volver a instalar dependencias |

---

## Soporte

`Soporte`

> 🛟 Esta sección está escrita para el equipo de soporte. Lenguaje accesible, cero jerga sin traducir.

### Errores comunes

| Mensaje | Qué significa | Qué hacer | Escalar si |
|---|---|---|---|
| `Stock insuficiente` | El producto no tiene unidades disponibles para esa operación | Verificar inventario con el supervisor de bodega | El sistema dice que SÍ hay stock pero el error sigue apareciendo |
| `Movimiento duplicado` | Ya se registró un movimiento con el mismo ID en los últimos 10 segundos | Esperar 30 seg y reintentar | El error persiste después de 2 minutos |
| `Error interno del servidor` | Algo falló en el sistema, no en lo que hizo el usuario | Tomar captura, registrar hora, abrir ticket a desarrollo | Siempre — esto requiere desarrollo |

### Estado del sistema (checklist)

- [ ] Endpoint `https://app/health` responde con `{"status":"ok"}`.
- [ ] Cola de movimientos con menos de 100 items pendientes.
- [ ] Latencia promedio de respuesta <300ms.

### Procedimiento ante incidente

```mermaid
%%{init: {'theme':'base','themeVariables':{'primaryColor':'#E6F4F6','primaryBorderColor':'#0097A7','lineColor':'#9CA3AF'}}}%%
flowchart TD
  A[Llega el reporte] --> B{¿Afecta a usuarios ahora?}
  B -- Sí --> C[Severidad alta - llamar al on-call]
  B -- No --> D[Crear ticket en Jira]
  C --> E[Notificar en #incidentes cada 15 min]
  D --> F[Asignar a próximo sprint]
```

### Contactos

| Rol | Persona / equipo | Canal | Cuándo contactar |
|---|---|---|---|
| On-call | Rotación semanal | PagerDuty | Incidente P1/P2 |
| Lead técnico | María Pérez | Slack `@maria` | Decisiones de arquitectura urgentes |
| Product owner | Juan Gómez | Slack `@juan` | Dudas funcionales |

---

## Manual de usuario

`Usuario final`

> Aplica solo si el sistema tiene UI para usuarios finales.

### Cómo iniciar sesión

1. Abre `https://app.tu-empresa.com`.
2. Ingresa tu usuario y contraseña corporativos.
3. Si olvidaste tu contraseña, haz clic en **"Recuperar contraseña"**.

### Cómo registrar un movimiento

1. En el menú lateral, selecciona **Movimientos** → **Nuevo**.
2. Escanea el código del producto o búscalo por nombre.
3. Ingresa la cantidad.
4. Confirma con **Guardar**.

### Errores comunes en la UI

| Mensaje que ves | Por qué aparece | Qué hacer |
|---|---|---|
| "Sin conexión" | Tu equipo perdió internet | Verifica el wifi y reintenta |
| "Sesión expirada" | Pasaron más de 8 horas desde tu login | Vuelve a iniciar sesión |

### Glosario

- **Movimiento**: entrada o salida de un producto en el inventario.
- **SKU**: código único que identifica cada producto.
- **Bodega**: ubicación física donde se almacena la mercancía.

---

<sub>Documentación generada con [Sódeker Docs Skill](https://github.com/<owner>/<repo>) · Estética minimalista turquesa · Diagramas estilo Distill.</sub>
