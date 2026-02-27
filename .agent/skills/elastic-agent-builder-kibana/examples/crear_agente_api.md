# Ejemplo: Crear Agente Vía API

## Contexto
Una vez registradas las herramientas, un Agente puede crearse asociándole sus comportamientos (Instrucciones) y las herramientas permitidas.

## Código (cURL)

```bash
curl -X POST "${KIBANA_URL}/api/agent_builder/agents" \
  -H "Authorization: ApiKey ${API_KEY}" \
  -H "kbn-xsrf: true" \
  -H "Content-Type: application/json" \
  -d '{
    "id": "secops-triage-agent",
    "name": "SecOps Triage AI",
    "description": "Investiga y enriquece alertas automáticamente.",
    "labels": ["production", "soar"],
    "avatar_color": "#FF0000",
    "avatar_symbol": "SO",
    "configuration": {
      "instructions": "Eres un analista nivel 3 de operaciones de seguridad. Sigue rigurosamente tu esquema de herramientas para analizar IPs.",
      "tools": [
        {
          "tool_ids": [
            "my-custom-search-tool",
            "platform.core.search"
          ]
        }
      ]
    }
  }'
```
