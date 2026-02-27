# Knowledge Update: Elastic Workflow Library - Advanced Concepts & AI Agents Integration

- **URLs de origen**: 
  - https://raw.githubusercontent.com/elastic/workflows/main/docs/concepts.md
  - https://raw.githubusercontent.com/elastic/workflows/main/workflows/ai-agents/invoke-an-agent.yaml
- **Fecha de captura**: 2026-02-26

## Diferencias con versión anterior
Durante la extracción exhaustiva se encontraron contextos adicionales de variables no descritos en el frontend, condicionales lógicos (`if`), y el método estructurado para disparar agentes LLM pre-configurados llamando a la acción de Agent Builder dentro del flujo YAML.

## Contenido Nuevo Extraído

### 1. Variables Contextuales y Filtros Avanzados
El motor Liquid expone más que solo *consts* e *inputs*:
- `{{ env.VARIABLE }}`: Variables de entorno del nodo.
- `{{ now }}`: Timestamp actual.
- Filtros importantes: `{{ inputs.query | urlencode }}`, `{{ pasos | default: "valor" }}`, y conversión de JSON `{{ event | json }}`.

### 2. Flujo de Control y Dependencias
Por defecto los pasos son secuenciales, pero permiten ramas. 
- **Explicit Dependencies**: Se puede forzar que un paso espere a múltiples paralelos usando la key `requires: [step1, step2]`.
- **Condicionales (`if`)**: Los pasos pueden llevar una directriz `condition` o ser Action Types de `type: if` con un nodo hijo `steps`.
  ```yaml
  - name: check
    type: if
    condition: 'inputs.severity: "high"'
    steps: ...
  ```

### 3. Invocando un Agente de IA desde Workflow
La integración nativa que une *Elastic Workflow Library* con *Elastic Agent Builder* ocurre bajo el Action Type `kibana.post_agent_builder_converse`.
Parámetros:
- `agent_id`: El identificador único del agente.
- `input`: El prompt (que usualmente usa sintaxis Liquid para inyectar logs o alertas, ej. `"... {{ event | json }}"`).
- `timeout`: Expresado en string (ej. `600s`).
El resultado del agente queda en `{{ steps.kibana_post_agent_builder_converse_step.output.message }}`.
