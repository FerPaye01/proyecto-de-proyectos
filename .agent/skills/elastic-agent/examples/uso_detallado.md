# Ejemplo: Uso Detallado (Estructura de Creación de Tools y Agentes)

## Contexto
Flujo detallado de los dos payloads principales (Tool y Agent) que se deben enviar a Kibana para que el asistente tenga una base de razonamiento de ES|QL.

## Payload de la Herramienta (Tool)
Endpoint: `/api/agent_builder/tools`

```json
{
  "id": "financial_risk_calculator",
  "type": "esql",
  "description": "Calculates strictly the risk of accounts.",
  "configuration": {
    "query": "FROM financial_news | WHERE sentiment == \"negative\" | RENAME primary_symbol AS symbol | LOOKUP JOIN financial_asset_details ON symbol | LOOKUP JOIN financial_holdings ON symbol | LIMIT ?limite",
    "params": {
      "limite": {
        "type": "integer",
        "description": "Limita los resultados. Pasa 10 por defecto."
      }
    }
  }
}
```

## Payload del Agente (Agent)
Endpoint: `/api/agent_builder/agents`

```json
{
  "id": "financial_assistant",
  "type": "chat",
  "name": "Financial Assistant",
  "description": "An assistant for analysis",
  "labels": ["Finance"],
  "configuration": {
    "instructions": "You are a specialized Data Intelligence Assistant... Use your given tools iteratively.",
    "tools": [
      {
        "tool_ids": [
          "platform.core.search",
          "financial_risk_calculator"
        ]
      }
    ]
  }
}
```
