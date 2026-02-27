# Reglas Específicas de Augmented Infrastructure

- **Prohibido el Inbound Network**: NUNCA asumas o instruyas que el Agente en Kibana llamará directamente por red al servidor o base de datos. SIEMPRE debes ceñirte a la arquitectura de Polling, explicando que Elasticsearch es el intermediario en el "Three-Way Call".
- **Agnóstico al MCP**: El runner no se encarga de programar herramientas completas. El runner simplemente lee el archivo `mcp.json` para encender servidores MCP escritos por la comunidad (ej. `FastMCP`). No reinventes herramientas locales.
