# Knowledge Update: Elastic AI Agent Builder - Deep Technical Investigation

- **URL de origen**: https://www.elastic.co/search-labs/blog/ai-agent-builder-elasticsearch y notebook `https://raw.githubusercontent.com/elastic/elasticsearch-labs/main/supporting-blog-content/your-first-elastic-agent/Your_First_Elastic_Agent.ipynb`
- **Fecha de captura**: 2026-02-26

## Diferencias con versión anterior
En implementaciones teóricas previas, carecíamos del conocimiento exacto de los payloads para las APIs de Kibana. Ahora, hemos extraído el diccionario de configuración exacto desde Python y JSON, confirmando la estructura interna requerida por Elastic.

## Contenido Nuevo Extraído (Extracción Exhaustiva JSON)

### Estructura Técnica API `POST /api/agent_builder/tools`
Para crear la herramienta *find_client_exposure_to_negative_news*:
```json
{
  "id": "find_client_exposure_to_negative_news",
  "type": "esql",
  "description": "Finds client portfolio exposure to negative news...",
  "tags": [
    "retrieval",
    "risk-analysis"
  ],
  "configuration": {
    "query": "FROM financial_news... | LIMIT 50",
    "params": {
      "time_duration": {
        "type": "keyword",
        "description": "The timeframe to search back for negative news. Format is 'X hours' DEFAULT TO 8760 hours "
      }
    }
  }
}
```

### Estructura Técnica API `POST /api/agent_builder/agents`
Muestra el metadata exacto del Payload del Agente:
```json
{
  "id": "financial_assistant",
  "type": "chat",
  "name": "Financial Assistant",
  "description": "An assistant for analyzing and understanding your financial data",
  "labels": ["Finance"],
  "avatar_color": "#16C5C0",
  "avatar_symbol": "💰",
  "configuration": {
    "instructions": "You are a specialized Data Intelligence Assistant...\n\n**Your Core Mission:**\n...",
    "tools": [
      {
        "tool_ids": [
          "platform.core.search",
          "platform.core.list_indices",
          "platform.core.get_index_mapping",
          "platform.core.get_document_by_id",
          "find_client_exposure_to_negative_news"
        ]
      }
    ]
  }
}
```

Manejo de errores encontrados (logs/excepciones):
- `400 Tool with id [id] already exists`
- Al fallar un request general: el body contiene `e.response.text`.
