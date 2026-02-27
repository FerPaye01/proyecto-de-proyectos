# Ejemplo: Core Loop Asíncrono de Python (Internal Runner)

## Contexto
Aunque el usuario final usualmente solo configurará el archivo `mcp.json` y ejecutará el binario instalado por `uv`, es vital que como Agente conozcas cómo el motor local en Python lee Elastic, reporta herramientas y administra sus latidos (heartbeats).

## Código del Orquestador (`runner.py`)

A continuación, la abstracción central de la concurrencia que permite el milagro del aislamiento de firewall seguro:

```python
async def run(self, delay: float = 0.5) -> None:
    """Correcel el runner continuamente."""
    step: int = 0

    async with self.context():
        # Auto-configura el cluster (opcional)
        logger.info(msg="Garantizando que Agent Builder y Workflows están activados.")
        await self._es_builder_client.enable_agent_builder()
        await self._es_builder_client.enable_workflows_ui()

        # Aviso al cluster de existencia del Nodo
        await self.check_in()
        
        # Publicación del catálogo de herramientas (Auto-Discovery)
        await self.report_available_tools()

        # Bucle de Eventos (Polling)
        while True:
            # Latido (Heartbeat) cada ~30s (0.5 * 60)
            if step % 60 == 0:
                await self.check_in()
            try:
                # Intenta descargar y ejecutar (FastMCP) la tarea actual:
                # - check_for_tool_requests(...)
                # - execute(client)
                # - add_tool_result(...)
                await self.run_step()
            except Exception as e:
                logger.error(msg=f"Error en paso asíncrono {step}: {e}")

            # Duerme asíncronamente para no saturar Elastic
            spin(delay)
            step += 1
```

*Por qué esto es brillante:* Nunca se usa `time.sleep` en el thread principal y la resiliencia es delegada al bucle completo. Si Elastic se cae, el loop simplemente reporta Exception hasta conectar nuevamente, sin perder los recursos de conexión FastMCP subyacentes.
