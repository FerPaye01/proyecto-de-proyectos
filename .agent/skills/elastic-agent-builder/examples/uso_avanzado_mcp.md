# Ejemplo: Uso Avanzado MCP y Componentes Asíncronos

## Contexto
El Modelo (LLM) no solo puede funcionar preguntando, también puedes mantener historiales (Chat History) e integrarte bidirectionalmente vía Model Context Protocol.

## Historial de Conversacionales API (Async / Memory)
Elastic Agent Builder maneja `conversation_id`.
```bash
# 1. Creamos la conversación en streaming
curl -X POST "${KIBANA_URL}/s/<space_name>/api/agent_builder/converse/async" \
  -H "Authorization: ApiKey ${API_KEY}" \
  -H "kbn-xsrf: true" \
  -H "Content-Type: application/json" \
  -d '{ 
        "input": "Hola, ayúdame con un problema asincrono", 
        "agent_id": "support-agent", 
        "conversation_id": "mi-chat-fijo" 
      }'

# 2. De ahora en adelante, el LLM tendrá contexto y recordará este input
```

## Agent Builder como Servidor MCP (IDEs)
Cualquier cliente compatible con MCP (VS Code, Cursor) enviará paquetes `JSON-RPC` a Elastic automáticamente si provees la siguiente config local (ej. archivo `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "elastic-agent-builder": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-elastic",
        "--url", "https://Micluster.url:443",
        "--api-key", "MI_KIBANA_KEY",
        "--space-id", "default"
      ]
    }
  }
}
```
*(Cursor interrogará a `/api/agent_builder/mcp` por la lista de herramientas ES|QL disponibles y las ejecutará)*
