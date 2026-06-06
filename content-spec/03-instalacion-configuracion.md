# Sección 3 — Instalación y configuración

> **Para quién**: Desarrolladores que llegan al proyecto y necesitan correrlo desde cero.
> **Pregunta que responde**: "¿Cómo levanto este proyecto en mi máquina?"

## Bloques obligatorios

### 3.1 Requisitos previos
- **Formato**: tabla `doc-table` con `Herramienta | Versión mínima | Cómo verificar | Cómo instalar`.
- **Incluir**: SO soportados, runtime (Node/Python/Java), gestor de paquetes, motor de BD, Docker si aplica, herramientas CLI específicas.
- **Comando de verificación**: incluir el `--version` real.

### 3.2 Versiones específicas de herramientas
- **Formato**: si hay un `.nvmrc`, `.python-version`, `package.json#engines`, mostrarlo en un bloque `<pre>`.
- **Comentar por qué** estas versiones: "Node 20.x — usamos features de fetch nativo".

### 3.3 Variables de entorno
- **Formato obligatorio**: tabla `doc-table` con `Variable | Requerida | Descripción | Ejemplo | Dónde obtenerla`.
- **Nunca poner valores reales** de secretos. Usar `<TU_API_KEY>` o `cambiar-en-prod`.
- **Agrupar por contexto**: `App`, `Base de datos`, `Servicios externos`, `Observabilidad`.
- **Incluir un `.env.example`** referenciado.

### 3.4 Comandos de instalación
- **Formato**: bloques `<pre>` numerados, uno por paso.
- **Cada paso**: precedido de un mini-título explicando qué hace el comando.
- **Ejemplo**:
  ```
  Paso 1 — Clonar el repositorio
  git clone https://github.com/...

  Paso 2 — Instalar dependencias
  npm install

  Paso 3 — Configurar variables
  cp .env.example .env
  ```

### 3.5 Cómo correr el proyecto localmente
- **Formato**: bloques de código + descripción del resultado esperado.
- **Incluir**:
  - Comando para arrancar en modo desarrollo.
  - Puerto que ocupa.
  - URL para verificar que está corriendo (`http://localhost:3000/health`).
  - Logs de éxito que debería ver el dev.
- **Acompañar con captura o ASCII art del log esperado** (cuando aplique).

### 3.6 Cómo ejecutar pruebas
- **Formato**: tabla con `Tipo de prueba | Comando | Cobertura aproximada | Dónde viven los archivos`.
- **Tipos**: unitarias, integración, end-to-end, contract tests, performance.
- **Si hay scripts para coverage**: documentarlos.

### 3.7 Cómo crear datos de prueba
- **Formato**: pasos numerados con bloques de código.
- **Incluir**:
  - Cómo poblar la BD con seed.
  - Cómo crear un usuario de prueba.
  - Cómo restaurar el estado inicial.
- **Importante**: indicar credenciales por defecto (usuario `dev@local`, password `dev1234`) **SOLO si son seguros de exponer** (solo para entorno local, nunca de staging/prod).

### 3.8 Problemas comunes y soluciones
- **Formato**: tabla `doc-table` con `Síntoma | Causa probable | Solución`.
- **Mínimo 5 entradas** para proyectos no triviales.
- **Ejemplos típicos**:
  - "Error EADDRINUSE" → puerto ocupado → matar proceso o cambiar puerto.
  - "Cannot find module" → faltó `npm install` después de cambio de branch.
  - "Connection refused: 5432" → Postgres no está corriendo.
- **Alternativa visual**: cards en grid si hay menos de 4 problemas.

## Bloques opcionales

### 3.9 Setup con Docker (si aplica)
- `docker-compose up` y los servicios que levanta.
- Cómo verificar que todos los contenedores están sanos.

### 3.10 Setup en entornos de equipo (staging, QA)
- Si hay un script de despliegue local que apunta a staging, documentarlo.

## Pautas visuales

- Cada paso numerado va en un `card` o en un `flow__step`.
- Los comandos van en `<pre>` con fondo oscuro (definido en `sodeker.css`).
- Las advertencias críticas ("nunca subas tu `.env` al repo") van en `callout--warning`.

## Tono específico

- Imperativo directo ("ejecuta", "verifica", "guarda").
- Cada comando explicado en 1 línea antes de aparecer.
- Asumir que el lector es un dev intermedio que puede no conocer este stack específico.
