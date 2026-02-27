# Changelog de Skills

## 2026-02-26
- Actualizado desde: https://raw.githubusercontent.com/strawgate/augmented-infrastructure/main/src/elastic_agent_builder_runner/
- Cambios detectados: El skill original era muy teórico en torno al concepto SOAR/MCP, ahora la skill ha sido actualizada de manera exhaustiva extrayendo el script oficial de Python. Se documentó cómo funciona el motor `DiskStore`/`PassthroughCacheWrapper`, la inyección de Latidos (Heartbeats) cada 30s dentro del loop asíncrono, y las reglas reales de descubrimiento de tools usando la librería `fastmcp`. Esto provee el contexto completo para que los agentes en este repositorio entiendan exactamente cómo operar un Runner de Kibana a nivel de código crudo.
