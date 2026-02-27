# Knowledge Source: Elastic Agent Builder Kibana APIs

- **URL Original**: https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/kibana-api
- **Fecha de Captura**: 2026-02-26

## Resumen de Contenido
La API REST de Kibana para Agent Builder permite gestionar programáticamente agentes, herramientas (tools) y el historial de conversaciones sin depender de la interfaz gráfica de usuario (UI). Es fundamental para infraestructuras as-code y arquitecturas SOAR.

## Investigación Profunda Extraída

### 1. Manejo de Espacios (Spaces)
Toda petición a la API de Kibana fuera del espacio `default` debe prefijarse con `/s/<space_name>`.
Ejemplo: `GET /s/secops/api/agent_builder/tools`

### 2. Tools APIs (Herramientas)
- **Listar**: `GET /api/agent_builder/tools`
- **Crear (POST)**: 
  ```json
  {
    "id": "example-esql-tool",
    "type": "esql",
    "description": "Herramienta ES|QL para análisis",
    "tags": ["analytics"],
    "configuration": {
      "query": "FROM logs | LIMIT ?limit",
      "params": {
        "limit": { "type": "integer", "description": "Max results" }
      }
    }
  }
  ```
- **Actualizar (PUT)** y **Eliminar (DELETE)** sobre `/api/agent_builder/tools/{id}`.

### 3. Agents APIs (Agentes)
- **Crear (POST)**:
  ```json
  {
    "id": "new-agent-id",
    "name": "Search Index Helper",
    "description": "Agente experto en índices",
    "labels": ["custom"],
    "avatar_color": "#BFDBFF",
    "avatar_symbol": "SI",
    "configuration": {
      "instructions": "Eres un agente customizado...",
      "tools": [
        {
          "tool_ids": [
            "platform.core.search",
            "example-esql-tool"
          ]
        }
      ]
    }
  }
  ```

### 4. Chat y Conversaciones (Converse API)
- **Conversación Síncrona**: `POST /api/agent_builder/converse`
  ```json
  {
    "input": "What is Elasticsearch?",
    "agent_id": "elastic-ai-agent"
  }
  ```
- **Conversación Asíncrona (Streaming)**: `POST /api/agent_builder/converse/async`
  Requiere enviar el parámetro `conversation_id`.
- **Historial**: `GET /api/agent_builder/conversations/{conversation_id}`
