# Sódeker Docs Skill

> Genera documentación de proyectos de software con identidad visual Sódeker — minimalista, turquesa, profesional — y diagramas interactivos estilo Distill.

Una skill para [Claude Code](https://claude.com/claude-code) que produce documentación técnica y de negocio coherente, navegable y con estética de landing tecnológica corporativa.

## Qué genera

Hasta 6 secciones por documento, cada una opcional según el proyecto:

| Sección | Para quién | Qué incluye |
|---|---|---|
| **Resumen funcional** | Producto, negocio, nuevos miembros del equipo | Objetivo, problema, usuarios, funcionalidades, reglas de negocio, casos de uso |
| **Documentación técnica** | Desarrolladores | Arquitectura, stack, estructura, patrones, decisiones, integraciones, dependencias, diagramas |
| **Instalación y configuración** | Desarrolladores | Requisitos, variables, comandos, pruebas, datos seed, troubleshooting |
| **Soporte** | Mesa de ayuda, QA, soporte funcional | Errores comunes, logs, alertas, métricas, reinicio de servicios, contactos, incidentes, FAQs |
| **Manual de usuario** | Usuarios finales no técnicos | Login, funciones principales, capturas, errores comunes, FAQs, guías paso a paso |

## Salidas soportadas

- **HTML** (por defecto) — un solo archivo, sidebar de navegación a la derecha, figuras interactivas, sticky scroll. Listo para abrir en navegador o exportar a PDF.
- **Markdown** — `git-friendly`, diagramas Mermaid, tablas limpias, bloques HTML embebidos donde el renderer los soporta.

## Instalación

### Global (recomendado)

```bash
npx skills add <owner>/<repo>@sodeker-docs -g
```

O manualmente:

```bash
git clone https://github.com/<owner>/<repo>.git ~/.claude/skills/sodeker-docs
```

### Local al proyecto

```bash
mkdir -p .claude/skills
git clone https://github.com/<owner>/<repo>.git .claude/skills/sodeker-docs
```

## Uso

Dentro de Claude Code, invoca la skill:

```
/sodeker-docs documenta el módulo de inventarios
/sodeker-docs en markdown — sistema de facturación
/sodeker-docs runbook de soporte para el servicio de notificaciones
```

La skill te hará 5–6 preguntas para entender el proyecto, leerá tus archivos si das acceso al repo, y generará la documentación en el formato elegido.

## Identidad visual

- **Paleta**: blanco + negro + grises + turquesa `#0097A7` como único acento.
- **Tipografía**: Inter / Manrope — sans-serif moderna, geométrica.
- **Layout**: contenido a la izquierda, navegación sticky a la derecha.
- **Componentes**: cards con borde hairline, badges turquesa pálido, métricas grandes en turquesa, tablas sin bordes verticales, callouts pálidos, diagramas de flujo con flechas grises delgadas (estilo Distill).
- **Reglas duras**: sin emojis, sin gradientes, sin sombras pesadas, sin colores fuera de la paleta.

## Estructura

```
sodeker-docs/
├── SKILL.md                # punto de entrada
├── style/sodeker.css       # tokens + componentes
├── content-spec/           # qué debe responder cada sección
├── templates/              # HTML y Markdown
├── examples/               # output de referencia
└── assets/                 # SVGs (badge mano, grilla)
```

## Crédito

- Identidad visual inspirada en Sódeker.
- Mecánica de figuras interactivas, conectores y badge "mano" inspirados en [Distill](https://distill.pub) (open agent skills ecosystem).

## Licencia

MIT — ver `LICENSE`.
