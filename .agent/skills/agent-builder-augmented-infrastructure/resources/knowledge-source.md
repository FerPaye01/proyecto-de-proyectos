# Knowledge Source: Elastic Agent Builder Augmented Infrastructure

- **URLs Originales**: 
  - https://www.elastic.co/search-labs/blog/agent-builder-augmented-infrastructure
  - https://github.com/strawgate/augmented-infrastructure/blob/main/ARCHITECTURE.md
- **Fecha de Captura**: 2026-02-26

## Resumen de Contenido
Augmented Infrastructure permite a los Agentes de IA de Kibana interactuar de forma segura con recursos locales, APIs y clústeres detrás de firewalls. Resuelve el problema de que los agentes solo operen en la burbuja del chatbox, permitiéndoles ejecutar comandos reales en el mundo exterior mediante el Model Context Protocol (MCP).

## Investigación Profunda Extraída

### La Arquitectura "Three-Way Call" (Llamada a Tres Vías)
Kibana AI Agent **nunca llama directamente** a la infraestructura. Se utiliza un intermediario de estado en Elasticsearch para sortear firewalls y permitir desacoplamiento.

1. **Agente (Kibana)**: Cuando el LLM decide usar una herramienta externa, usa un tool nativo llamado `call_ai_runner_tool`.
2. **Kibana Workflow (Transporte)**: Atrapa esa decisión y guarda un documento JSON con los argumentos de la herramienta en un índice de Elasticsearch (`distributed-tool-requests`), retornando un `exec_id` al Agente.
3. **Runner Local (Servidor/Host)**: Es un loop infinito en Python que hace *polling* (cada 0.5s) al índice de Elasticsearch buscando su `runner_id`. Al encontrar la tarea, la extrae.
4. **Ejecución MCP**: El runner le pasa los argumentos al servidor MCP configurado localmente.
5. **Retorno de Resultados**: El runner guarda el resultado del MCP en el índice `distributed-tool-results`.
6. **Agente (Kibana)**: El agente de IA se queda haciendo polling con la operación `get_tool_results(exec_id)` hasta que obtiene el resultado y continúa la conversación con el usuario.

### Índices de Estado en Elasticsearch (La Base de Datos como Bus)
- `distributed-runner`: Beacons de salud enviados por los runners (cada 60s).
- `distributed-tools`: Catálogo de herramientas que los runners suben para descubrimiento.
- `distributed-tool-requests`: Peticiones de ejecución del Agente hacia el Runner.
- `distributed-tool-results`: Retorno de resultados del Runner hacia el Agente.

### Configuración del Runner
- Se basa en Python 3.13+ y `uv`.
- Configuración de entorno local:
  - `.env` contiene: `KB_URL`, `ES_URL` y `API_KEY`.
- Se usa el estándar `mcp.json` para definir qué servidores MCP corren bajo este runner.

```json
{
  "mcpServers": {
    "docker": {
      "command": "uvx",
      "args": [ "docker-mcp" ]
    }
  }
}
```

### Casos de Uso Demostrados en el Blog
- **DevOps Rescue**: El agente instaló OpenTelemetry (generando configs de Kubernetes, namespaces y secrets) para habilitar observabilidad automática.
- **Security Handoff**: El agente enumeró recursos AWS, descubrió un EKS expuesto, y con aprobación del usuario, le instaló Elastic Security (XDR) localmente.
