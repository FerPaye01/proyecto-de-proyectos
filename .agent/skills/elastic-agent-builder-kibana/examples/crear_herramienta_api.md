# Ejemplo: Crear Herramienta (Tool) Vía API

## Contexto
Script de cURL puro documentando los parámetros obligatorios para registrar una nueva herramienta para Agent Builder en un Espacio específico.

## Código (cURL)
```bash
SPACE_ID="security-team"
KIBANA_URL="https://tu_instancia_kibana.es.io"
API_KEY="MASCARA_KEY"

curl -X POST "${KIBANA_URL}/s/${SPACE_ID}/api/agent_builder/tools" \
  -H "Authorization: ApiKey ${API_KEY}" \
  -H "kbn-xsrf: true" \
  -H "Content-Type: application/json" \
  -d '{
    "id": "my-custom-search-tool",
    "type": "esql",
    "description": "Una herramienta obligatoria que busca en los índices de log.",
    "tags": ["logs", "security"],
    "configuration": {
      "query": "FROM logs-* | WHERE user.name == ?username | LIMIT 10",
      "params": {
        "username": {
          "type": "string",
          "description": "El nombre de usuario a buscar"
        }
      }
    }
  }'
```
