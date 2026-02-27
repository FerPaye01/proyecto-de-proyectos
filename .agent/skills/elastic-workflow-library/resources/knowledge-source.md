# Knowledge Source: Elastic Workflow Library

- **URL Original**: https://github.com/elastic/workflows/ (y documentación interna `docs/schema.md`, `docs/importing.md`)
- **Fecha de Captura**: 2026-02-26

## Resumen de Contenido
Elastic Workflows es una librería y arquitectura para la automatización declarativa (basada en YAML) dentro del ecosistema Elastic/Kibana. Permite integrar Elasticsearch (búsquedas, indexación, ES|QL), Kibana (creación de casos, alertas), APIs externas (HTTP, Slack, Jira) y modelos de AI/ML.

## Investigación Profunda Extraída

### Workflow Schema (Estructura YAML Mínima Válida)
Todo flujo requiere el campo `name` y un array de `steps`.
Campos opcionales clave: `triggers` (manual, scheduled, alert), `consts`, `inputs`.

```yaml
name: "Minimal Workflow"
triggers:
  - type: scheduled
    with:
      every: "1d"
inputs:
  - name: my_input
    type: string
steps:
  - name: "Log Message"
    type: console
    with:
      message: "Hello {{ inputs.my_input }}"
```

### Sintaxis de Variables y Liquid Templating
El motor usa llaves dobles (`{{ }}`) para la inyección de variables.
Contextos principales:
- `{{ consts.api_key }}`
- `{{ inputs.target_ip }}`
- `{{ steps.nombre_paso.output.campo }}`
- `{{ foreach.item._id }}` (en loops)
Filtros: `{{ inputs.name | uppercase }}`, `{{ variables | default: "fallback" }}`

### Action Types Soportados
- `http`: Peticiones REST (con method, headers, auth).
- `elasticsearch.search` o `elasticsearch.esql.query`: Ejecuta queries y retorna la salida (p.ej. `steps.search.output.hits.hits`).
- `kibana.cases` o `kibana.alert`: Interactuar con gestión de incidentes.
- `foreach`: Itera arrays con la variable contextual `foreach`.

### Importación de Workflows por API (Programática)
Kibana expone el endpoint `POST /api/workflows`. El cuerpo debe ser un JSON que envuelva el contenido YAML bajo la llave `yaml`.  
Se requieren headers: `kbn-xsrf: true`, `x-elastic-internal-origin: Kibana` y `Authorization: ApiKey <KEY>`.

Ejemplo de payload JSON para el API:
```json
{
  "yaml": "name: Mi Workflow\nsteps:\n  - name: test\n    type: console\n..."
}
```

## Patrones Comunes
- Búsqueda y Enriquecimiento: Buscar en ES, hacer loop (`foreach`) sobre los resultados y llamar a un `http` por cada uno.
- Alerting: Query ES|QL programada cada `1h` que envía un resumen a Slack si el conteo superó cierto umbral.
