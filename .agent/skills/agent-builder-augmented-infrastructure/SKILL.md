---
name: Agent Builder Augmented Infrastructure
description: Implementación técnica de Runners asíncronos en Python, descubrimiento de herramientas MCP y arquitecturas desacopladas para Agentes de Elastic.
---

# Skill: Agent Builder Augmented Infrastructure

## Propósito
Guiar la implementación de agentes de IA en Elastic capaces de trascender el chatbox interconectándolos de forma segura y automatizada con sistemas locales (Kubernetes, AWS, Docker). El skill documenta la arquitectura "Three-Way Call" impulsada por Elasticsearch (Polling) y la inicialización asíncrona de los scripts Python que rigen los ejecutores FastMCP.

## Cuándo Usar
- Cuando un usuario requiera "permitir que mi Agente de Kibana acceda a mi base de datos/archivos locales".
- Al configurar un SOAR local e integrar observabilidad (Ej: "Instalar agentes OpenTelemetry cuando ocurra una alerta de Kibana").
- Al desarrollar o debugear la interacción entre el script de Python `elastic_agent_builder_runner`, los índices de Kibana y los MCP Servers mediante `mcp.json`.

## Conceptos Clave
1. **Runner Script (`runner.py`)**: Loop `while True` en Python que monitorea tareas usando Delay (0.5s) y emite Beacons/Check-In cada 30 segundos (`step % 60 == 0`).
2. **Auto-Discovery**: El runner usa `mcp_client.list_tools()` y las sube automáticamente a Elasticsearch, liberando al usuario de declarar inputs y schemas del lado de Kibana.
3. **mcp.json Agnosticismo**: Único archivo necesario en infraestructura para definir si se usa `npx` (Node) o `uvx` (Python) para los servidores MCP locales.

## Instrucciones y Arquitectura
- **Regla Estricta Inbound**: NUNCA sugieras abrir puertos del host local para que el Agente en la nube los consuma (webhooks). Obliga al paradigma de "Distributed Tool Polling" donde la base de datos es el intermediario.
- **Tolerancia a Fallos**: Los runners usan `DiskStore` como capa persistente y nunca fallan silenciosamente; devuelven el error de consola al LLM en Kibana (`ToolCallResult`) para que el Agente gestione el error nativamente o mediante un flujo condicional de Elastic Workflow.

## Ejemplos
- **Configuración Básica Host**: `examples/ejemplo_configuracion_runner.md`
- **Flujo de Arquitectura Three-Way**: `examples/ejemplo_flujo_arquitectura.md`
- **Core Loop Asíncrono de Python**: `examples/ejemplo_python_runner_loop.md`

## Referencias
Revisa el `resources/knowledge-update-2026-02-26.md` para visualizar el código crudo extraído de GitHub (Asyncio, FastMCP, DiskStore).
