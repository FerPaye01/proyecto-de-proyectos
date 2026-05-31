# Template de Proyectos

> Plantilla base para proyectos web con configuración completa de Antigravity (skills, rules, workflows)

## 📁 Estructura del Proyecto

```
├── .agent/
│   ├── rules/                          # Reglas del proyecto (8 archivos)
│   │   └── ...
│   ├── skills/                         # Habilidades del agente (3 skills)
│   └── workflows/                      # Flujos automatizados (6 workflows)
│
├── .analisis/                          # Documentación técnica y diseño
│   ├── ARCHITECTURE_DESIGN.md          # Diseño de arquitectura
│   ├── REQUIREMENTS_SPEC.md            # Especificación de requisitos
│   ├── SPRINT_ROADMAP.md               # Plan de sprints
│   └── TEST_ACCEPTANCE.md              # Pruebas de aceptación
│
├── src/
│   ├── utils/logger.js                 # Sistema de logs
│   └── styles/variables.css            # Variables CSS responsive
│
└── README.md
```

## 🛠️ Skills Disponibles

| Skill | Propósito |
|-------|-----------|
| `creador-skills` | Crea skills desde documentación |
| `gestor-rules` | Gestiona `.agent/rules/` |
| `monitor-skills` | Monitorea y mejora skills |

## 📋 Workflows

| Comando | Descripción |
|---------|-------------|
| `/iniciar-proyecto` | Configura estructura inicial |
| `/agregar-componente` | Añade componente con estilos |
| `/crear-skill-conocimiento` | Crea skill con fuente del usuario |
| `/crear-skill-conocimiento-documentado` | Crea skill buscando docs online |
| `/actualizar-skill-conocimiento` | Actualiza skill existente |
| `/actualizar-skill-conocimiento-documentado` | Actualiza skill con docs online |

## 🎯 Convenciones (English Naming)

- **Variables:** `camelCase` → `userData`, `flashcardList`
- **Funciones:** `camelCase` → `getFlashcard()`, `saveData()`
- **Archivos:** `camelCase` → `flashcardList.js`
- **Logs:** `logInfo()`, `logSequence()`, `logError()`
- **Comentarios:** En español
- **Sin tests:** Validar con logs

## 📦 Stack

> ⚠️ Pendiente de confirmar

Ver `.agent/rules/stack-tecnologico.md`

## 🚀 Reutilización

La carpeta `.agent/` es 100% portable para otros proyectos.
