# Knowledge Update: Elastic AI Agent Builder - Deep Technical Investigation

- **URLs de origen**: 
  - https://www.elastic.co/docs/explore-analyze/ai-features/elastic-agent-builder
  - https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/kibana-api
  - https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/tools/custom-tools
  - https://www.elastic.co/docs/explore-analyze/ai-features/agent-builder/tools/mcp-tools
- **Fecha de captura**: 2026-02-26

## Diferencias con versión anterior
La versión previa solo enumeraba conceptos abstractos. Esta actualización extrae el enrutamiento real para Spaces de Kibana, el detalle técnico de la API, las mejores prácticas de nombrado ("namespaces") para Tools y la doble función del soporte MCP.

## Contenido Nuevo Extraído (API y Arquitectura)

### Estructura de APIs (Soporte para Kibana Spaces)
Todas las llamadas al Agent Builder deben respetar el espacio de Kibana. La ruta base es `/api/agent_builder/`. Si se usa un espacio diferente al default, la ruta es `/s/<space_name>/api/agent_builder/`. Se debe pasar el header `Authorization: ApiKey ${API_KEY}` y `kbn-xsrf: true`.

- Colección `agents`: GET y POST `/api/agent_builder/agents`. 
  Payload: `{"id", "name", "description", "labels", "avatar_color", "avatar_symbol", "configuration.instructions", "configuration.tools[...]"}`
- Colección `tools`: GET y POST `/api/agent_builder/tools`
- Colección `converse`: POST `/api/agent_builder/converse`. 
  Payload: `{"input": "Pregunta del usuario", "agent_id": "mi-agente"}`
  Para streaming de respuesta: POST `/api/agent_builder/converse/async` incluyendo `conversation_id`.

### Mejores Prácticas de Tools (Naming Conventions & Descriptions)
Agregada arquitectura exigida por el LLM:
- **Naming**: Los IDs deben usar namespaces (ej. `domain.action_entity` -> `finance.search_ticker`, `support.get_ticket_details`).
- **Description**: Para que el LLM decida bien, una tool description debe incluir obligatoriamente: Core purpose, Trigger (cuándo usarlo), Action, Limitations y Relationships con otros tools.

### Model Context Protocol (MCP) Bidireccional
- Agent Builder puede **consumir** un servidor MCP externo mediante la creación de un MCP Connector en Kibana, convirtiendo API externas en Tools que el agente de Elastic puede usar.
- Agent Builder también sirve como MCP Server hacia el exterior (ej: para Cursor) en la ruta `/api/agent_builder/mcp`.
