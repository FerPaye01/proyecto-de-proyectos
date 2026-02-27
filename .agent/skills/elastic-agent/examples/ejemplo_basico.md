# Ejemplo Técnico: Configuración de Cliente MCP interactuando con Elastic

## Contexto
Deseas usar **Cursor** o la aplicación **Claude Desktop** para que tenga acceso completo, en tiempo real, a la base de datos de Elastic utilizando las tools de "Agent Builder" mediante el Model Context Protocol (MCP).

## Entrada (El Archivo de Configuración del Cliente)

En Claude Desktop, editarías el archivo de configuración usualmente localizado en `~/Library/Application Support/Claude/claude_desktop_config.json`.
En Cursor, lo configuras en las opciones de "Features -> MCP".

### Payload Típico JSON
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
*(Asumiendo el uso del adaptador oficial Node.js de MCP para Elastic, el cual traduce el stdin/stdout del IDE a requests HTTP hacia la API `/api/agent_builder/mcp`)*

## Salida Esperada
Una vez reiniciado el IDE, el LLM (Claude o Cursor) notará nuevos `tools` en su catálogo. Por ejemplo, al preguntar "Busca incidentes de seguridad en los últimos 15 min", el LLM del IDE reconocerá automáticamente y ejecutará el tool ES|QL `security_incidents_tool` que creaste en Elastic Agent Builder, usando tu Kibana remotamente como motor de búsqueda.
