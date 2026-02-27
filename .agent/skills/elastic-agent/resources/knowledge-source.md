# Knowledge Source: AI Agent Builder - Elasticsearch Technical Documentation

- **Fechas de Captura**: 2026-02-26
- **Fuente Principal**: "AI Agent Builder: Creating specialized AI agents in Elasticsearch" (Search Labs) y "Elastic API Reference for MCP and Agent Builder".

## Resumen de Contenido Técnico

Elastic Agent Builder es la plataforma oficial de IA de Elastic para la creación de herramientas conectadas a Elasticsearch y Agentes con estado. Utiliza Model Context Protocol (MCP) para exponer y consumir Tools.

### Arquitectura Técnica
1. **Model Context Protocol (MCP)**:
   - Elastic Agent Builder **actúa nativamente como un MCP Server**.
   - El endpoint del Servidor MCP es: `{KIBANA_URL}/api/agent_builder/mcp`
   - Los clientes MCP (como Cursor, Claude Desktop) se conectan al Agent Builder vía JSON-RPC 2.0. El payload de configuración clásico `mcpServers.json` necesita autorizaciones API estáticas.

2. **Endpoints Centrales de Agent Builder**:
   - `GET|POST /s/{spaceid}/api/agent_builder/tools`: Maneja el CRUD de Tools. Cada tool recibe un `name`, `description` (requerido por LLM) y `esql_query` (si es un tool de Elasticsearch).
   - `GET|POST /s/{spaceid}/api/agent_builder/agents`: Crea el metadata del LLM, el `system_prompt` (instrucciones) y asigna un Array de `tool_ids`.
   - `POST /s/{spaceid}/api/agent_builder/converse`: Ejecuta el razonamiento LLM de forma iterativa.

### Estructura de Integración de Cliente MCP a Elastic
Un cliente MCP como Cursor requiere un archivo de configuración que conecte al puerto de Kibana:
```json
{
  "mcpServers": {
    "elastic-agent": {
      "command": "curl", 
      "args": [
        "-H", "Authorization: ApiKey YOUR_API_KEY",
        "https://TU_KIBANA_URL/api/agent_builder/mcp"
      ]
    }
  }
}
```
*(Nota: Diferentes frameworks de MCP pueden pedir adaptadores NPM/npx en lugar de cURL para los streams persistentes stdio)*.
