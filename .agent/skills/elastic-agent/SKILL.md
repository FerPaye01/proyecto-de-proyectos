---
name: Elastic AI Agent Builder Practical Guide
description: Patrones y flujos técnicos exhaustivos para la construcción de agentes de IA en Elastic enfocados a APIs y MCP
---

# Skill: Elastic AI Agent Builder

## Propósito
Guiar al desarrollador en la implementación técnica (API/JSON level) de soluciones de IA usando el Elastic AI Agent Builder y Model Context Protocol (MCP), cubriendo sintaxis de código, scripts API y configuraciones avanzadas.

## Cuándo Usar
- Para automatizar la creación de Agentes o Herramientas ES|QL mediante llamadas API a Kibana conectadas a CI/CD.
- Para configurar un servidor **Model Context Protocol (MCP)** desde Elastic y conectarlo a IDEs o bots.

## Instrucciones y Arquitectura

### 1. Endpoint The MCP Server (Model Context Protocol)
- Endpoint: `POST {KIBANA_URL}/api/agent_builder/mcp`
- Autenticación: Requiere pasar un header con `Authorization: ApiKey <value>`.
- Permite a LLMs de terceros usar ES|QL directamente sobre tu sistema, o viceversa, permite a tu Agent consumir apis de terceros.

### 2. Creación Programática de un Tool (API)
Payload estricto requerido por Elastic para `POST /api/agent_builder/tools`:
- `id` (String): ID de la herramienta
- `type` (String): Normalmente "esql"
- `configuration` (Object): Contiene "query" y un objeto de "params" definiendo los parámetros inferidos (como "?time_duration").

### 3. Creación Programática de un Agente (API)
Payload exigido para `POST /api/agent_builder/agents`:
- `id`, `name`, `type: "chat"`
- `configuration.instructions`: El prompt completo (System Prompt).
- `configuration.tools`: Array de objetos que declara `"tool_ids": ["mis_herramientas"]`.  Asegúrate de incluir tools por defecto de Elastic como `"platform.core.search"` si el agente debe buscar índices básicos.

## Logs y Mensajes de Error Comunes
- `Error 400`: `Tool with id [ID] already exists`.
- `Error 401`: `Unauthorized`. Faltan privilegios. Asegurate de que el API Key usado tiene el feature privilege "Agent Builder" habilitado.
- `Error LLM Context`: `Chat response variable is empty`. Ocurre si el Agente falla la inferencia. 
- *Logs Internos Agente:* `LOG_INFO: "elastic-agent: Configurando MCP Server..."`, `LOG_INFO: "elastic-agent: Testeando herramienta ES|QL con time_duration..."`.

## Comandos del Usuario
- "Crea un script python para registrar el agente de [X]"
- "Actualiza el json de Tool para añadir [X] parametro al ESQL"
- "Conecta Agent Builder a Cursor usando MCP json"

## Ejemplos
- **Uso rápido**: `examples/uso_rapido.md`
- **Uso detallado**: `examples/uso_detallado.md`
- **Uso avanzado (MCP)**: `examples/uso_avanzado.md`
- **Uso con scripts**: `examples/uso_scripts.md`
- **Uso con workflows**: `examples/uso_workflows.md`

## Referencias
Revisa `resources/knowledge-update-2026-02-26.md` para el schema JSON preciso desde el cuaderno Jupyter oficial de Elastic.
