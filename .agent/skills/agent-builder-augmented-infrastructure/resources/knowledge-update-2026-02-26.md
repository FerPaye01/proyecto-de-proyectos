# Knowledge Update: Elastic Agent Builder Augmented Infrastructure - Python Runner Implementation

- **URLs de origen**: 
  - https://raw.githubusercontent.com/strawgate/augmented-infrastructure/main/src/elastic_agent_builder_runner/main.py
  - https://raw.githubusercontent.com/strawgate/augmented-infrastructure/main/src/elastic_agent_builder_runner/runner.py
  - https://raw.githubusercontent.com/strawgate/augmented-infrastructure/main/examples/homelab/mcp.json
- **Fecha de captura**: 2026-02-26

## Diferencias con la versión anterior
Se ha descendido desde el plano teórico de la "Arquitectura a 3 vías" hacia el código ejecutable de Python exacto que orquesta este proceso, extrayendo las invocaciones de `fastmcp` y la estructura real del bucle de eventos (`asyncio`). 

## Contenido Nuevo Extraído

### 1. Inicialización y Persistencia (main.py)
El runner no almacena su estado interno en RAM volátil, utiliza capas passthrough entre disco y memoria:
```python
key_value_store: DiskStore = DiskStore(directory=storage_dir)
cached_store: PassthroughCacheWrapper = PassthroughCacheWrapper(
    primary_key_value=key_value_store, 
    cache_key_value=MemoryStore()
)
```
Y crea los clientes hacia Elasticsearch usando: `ElasticAgentBuilderClient` y `ElasticsearchClient`.

### 2. El Polling Asíncrono (runner.py)
El núcleo del runner es un bucle `while True` que combina Heartbeats (Check-Ins) y procesamiento asíncrono.
- La frecuencia predeterminada de sondeo (Spin delay) es de `0.5` segundos.
- Cada 60 steps (30 segundos), el runner emite su Health Check-In.

```python
async def run(self, delay: float = 0.5) -> None:
    step: int = 0
    # Inicialización, habilita UI y Workflows
    await self.check_in()
    await self.report_available_tools() # Lee el mcp.json

    while True:
        if step % 60 == 0:
            await self.check_in()
        try:
            await self.run_step() # Aquí consulta ES -> corre MCP -> responde a ES
        except Exception as e:
            logger.error(msg=f"Error occurred: {e}")
        spin(delay)
        step += 1
```

### 3. Declaración de Tools a Kibana (Discovery)
El Runner reporta automáticamente lo que su `mcp.json` ofrezca hacia Elasticsearch, de este modo el Agente LlM en Kibana se entera dinámicamente de sus capacidades sin configuración manual en la UI de Kibana.
```python
async def report_available_tools(self) -> None:
    tools: list[Tool] = await self._mcp_client.list_tools()
    await self._es_client.replace_tools(tools=tools)
```

### 4. mcp.json del mundo real
El archivo JSON permite cargar binarios de la comunidad (vía `uvx` o `npx`):
```json
{
    "mcpServers": {
        "docker": { "command": "uvx", "args": [ "docker-mcp" ] },
        "filesystem": { "command": "uvx", "args": [ "filesystem-operations-mcp" ] },
        "kubernetes": { "command": "npx", "args": [ "-y", "kubernetes-mcp-server@latest" ] }
    }
}
```
