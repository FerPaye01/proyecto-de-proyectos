# Reglas Específicas de Elastic Workflow Library

- **Obligatorio**: Todos los comandos cURL que inyecten pipelines YAML deben realizar parseo a JSON previamente, usando `jq -Rs '{yaml: .}'`. No inyectar YAML crudo en bodies JSON.
- **Validación de Contexto**: Al usar Action Types en YAML, validar que siempre se llame a outputs utilizando corchetes dobles `{{ consts.tu_var }}`.
