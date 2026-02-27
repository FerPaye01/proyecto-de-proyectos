# Ejemplo: Uso Avanzado (Model Context Protocol)

## Contexto
Deseas usar **Cursor**, **VS Code** o **Claude Desktop** para que tenga acceso completo, en tiempo real, a la base de datos de Elastic y a las Herramientas (Tools) de Elasticsearch preparadas por Agent Builder.

## Configuración

En Claude Desktop o Cursor, debes declarar un Servidor MCP para enlazar tu IDE con Elastic Agent Builder.
```json
{
  "mcpServers": {
    "elastic-agent-builder": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-elastic",
        "--url", "https://TU_CLUSTER_ID.us-central1.gcp.cloud.es.io:443",
        "--api-key", "TU_KIBANA_API_KEY",
        "--space-id", "default"
      ],
      "env": {}
    }
  }
}
```

## Detalles
El adaptador MCP de Elastic (via npx `@modelcontextprotocol/server-elastic`) convierte las comunicaciones estándar TCP/stdio de tu IDE en peticiones HTTP seguras contra el endpoint `{KIBANA_URL}/api/agent_builder/mcp`.
