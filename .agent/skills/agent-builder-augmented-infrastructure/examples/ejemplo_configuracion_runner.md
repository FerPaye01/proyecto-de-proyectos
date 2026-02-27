# Ejemplo: Configuración del Runner Local (mcp.json y .env)

## Contexto
El Augmented Infrastructure trabaja instalando un software ligero (Runner) en tu máquina local o clúster. Para ello usa Python `uv`. 

## 1. Variables de Entorno (`.env`)
El Runner requiere los puntos de anclaje a Elastic Cloud o clúster on-premise:
```bash
KB_URL=https://tu-kibana.example.com
ES_URL=https://tu-elasticsearch.example.com
API_KEY=tu_es_api_key_str
```

## 2. Configuración de Model Context Protocol (`mcp.json`)
El Runner leerá este archivo. Si el Agente de IA decide usar comandos de docker, será este archivo el que diga a qué binario llamar en tu máquina.
```json
{
  "mcpServers": {
    "docker": {
      "command": "uvx",
      "args": [
        "docker-mcp"
      ]
    },
    "kubernetes": {
      "command": "uvx",
      "args": [
        "mcp-k8s-go"
      ]
    }
  }
}
```

## 3. Ejecución
```bash
uv run python -m elastic_agent_builder_runner.main
```
Al correrlo, el runner anunciará sus herramientas leyendo el `mcp.json` y empezará a preguntar (polling) a Elasticsearch si el Agente necesita que ejecute algo.
