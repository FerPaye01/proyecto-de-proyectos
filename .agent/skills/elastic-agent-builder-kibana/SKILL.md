---
name: Elastic Agent Builder Kibana API
description: Guía técnica, endpoints y payloads JSON para gestionar Agentes y Herramientas programáticamente mediante la API REST de Kibana.
---

# Skill: Elastic Agent Builder Kibana API

## Propósito
Facilitar la automatización (Infraestructura como código) de los agentes de IA de Elastic, permitiendo su creación, actualización y evaluación conversacional desde scripts, pipelines CI/CD o workflows externos sin usar la interfaz gráfica (UI) de Kibana.

## Cuándo Usar
- Cuando un usuario pida "automatizar la creación de mis agentes en Kibana".
- Al consumir o testear un Agente desde scripts en Python o cURL (ej: "Haz un script que pregunte a mi agente X").
- Para orquestar dinámicamente Herramientas (Tools) a través de integraciones.

## Instrucciones y Arquitectura

### 1. Headers Obligatorios en Kibana
Toda invocación de la API de Kibana que realice alteraciones (POST, PUT, DELETE) debe incluir:
- `kbn-xsrf: true`
- Etiqueta de autorización: `Authorization: ApiKey TU_API_KEY`
- Header de formato: `Content-Type: application/json`

### 2. Gestión de Espacios (Kibana Spaces)
Si operas fuera del default, incluye invariablemente: `/s/<nombre-espacio>/api/agent_builder/...`

### 3. Endpoints Base
- **Tools**: `/api/agent_builder/tools`
- **Agentes**: `/api/agent_builder/agents`
- **Conversación**: `/api/agent_builder/converse`

## Comandos del Usuario
- "Hazme un script en shell que registre este archivo JSON como un nuevo Agent de Kibana."
- "Quiero interactuar con el agente financiero por API ¿cómo le paso una petición?"
- "¿Cómo elimino una herramienta vieja de Agent Builder usando cURL?"

## Ejemplos
- **Creación de Tools**: `examples/crear_herramienta_api.md`
- **Creación de Agentes**: `examples/crear_agente_api.md`
- **Iniciar Conversación**: `examples/conversacion_api.md`

## Referencias
Revisa el `resources/knowledge-source.md` para extraer los mapeos exactos de los JSONs de cada entidad de la API.
