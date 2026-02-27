# Knowledge Source: Elastic Workflows

- **URLs Originales**: 
  - https://www.elastic.co/docs/explore-analyze/workflows
  - https://www.elastic.co/docs/explore-analyze/workflows/get-started
  - https://www.elastic.co/docs/explore-analyze/workflows/triggers
  - https://www.elastic.co/docs/explore-analyze/workflows/steps
  - https://www.elastic.co/docs/explore-analyze/workflows/data
- **Fecha de Captura**: 2026-02-26

## Resumen de Contenido
Los Elastic Workflows son secuencias de pasos automatizados definidos en YAML dentro de Kibana. Permiten transformar insights de datos en resultados automatizados sin herramientas externas, abordando retos como la fatiga por alertas y el trabajo repetitivo.

## Investigación Profunda Extraída

### 1. Estructura YAML
- **Metadata**: `name`, `description`, `enabled`, `tags`.
- **Constants (`consts`)**: Valores fijos accesibles vía `{{ consts.key }}`.
- **Inputs (`inputs`)**: Parámetros con `type`, `required`, `default`, `description`.
- **Triggers (`triggers`)**: Disparadores del flujo.
- **Steps (`steps`)**: Lógica secuencial (o ramificada).

### 2. Disparadores (Triggers)
- **`manual`**: Ejecución bajo demanda (UI o API).
- **`scheduled`**: 
  - `every: 5m` (intervalos).
  - RRule para horarios específicos.
- **`alert`**: Inicia cuando una regla de detección genera una alerta. Recibe el objeto `event`.

### 3. Pasos (Steps)
#### Acciones Comunes
- `elasticsearch.search`: Consultas a índices.
- `elasticsearch.indices.create/delete/exists`: Gestión de esquemas.
- `elasticsearch.request`: Peticiones REST genéricas (`method`, `path`, `headers`, `body`).
- `console`: Logs en el panel de ejecución (`message`).
- `slack`, `email`: Notificaciones externas vía conectores.

#### Control de Flujo
- `if`: Disyunción lógica (`condition`, `steps`, `else`).
- `foreach`: Iteración sobre listas (`foreach`, `steps`, accede vía `foreach.item`).
- `wait`: Pausas y retrasos.

#### Inteligencia Artificial (AI Steps)
- `ai.prompt`: Envía un prompt a un conector LLM.
- `ai.agent`: Llama a un agente de Elastic Agent Builder.
- **Integración Bidireccional**: Los agentes de Agent Builder pueden activar workflows si se definen como "Workflow Tools".

### 4. Manejo de Datos y Plantillas
- **Sintaxis Liquid**: `{{ variables }}` para strings.
- **Preservación de Tipos**: `${{ variables }}` para objetos/arrays (JSON).
- **Contexto**:
  - `workflow.name`, `workflow.id`.
  - `execution.id`, `execution.start_time`.
  - `steps.<name>.output`.
  - `steps.<name>.error`.
  - `event` (en disparadores de alerta).

### 5. Manejo de Errores (`on-failure`)
Se puede configurar a nivel de paso o a nivel global (`settings`).
- **`retry`**: `max-attempts`, `delay` (formato duración como "5s", "1m").
- **`fallback`**: Pasos alternativos si falla el principal. Acceso al error vía `{{ steps.original.error }}`.
- **`continue: true`**: Ignora el error y prosigue.
