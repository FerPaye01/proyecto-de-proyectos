---
name: Elastic Agent Builder
description: Guía técnica, APIs, configuración y uso avanzado de Model Context Protocol y Agentes en Elastic.
---

# Skill: Elastic Agent Builder

## Propósito
Permitir a los agentes del proyecto estructurar e invocar interacciones conversacionales, flujos MCP (Model Context Protocol) y Tools complejas usando las REST APIs oficiales de Kibana y las directrices de ingeniería de prompts de Elastic Agent Builder.

## Cuándo Usar
- Para construir o actualizar scripts automatizados (bash/python/node) que interactúen con Agentes y Tools de Elastic vía API.
- Para establecer integraciones MCP (ya sea con Kibana como Cliente consumiendo herramientas externas o como Servidor proveyendo herramientas a IDEs/bots).
- Al implementar directrices de "Naming y Descripción" para forzar al LLM a elegir las herramientas correctas.

## Instrucciones

### 1. Manejo de APIs (Con soporte de Espacios)
Todas las interacciones programáticas se hacen agregando headers `kbn-xsrf: true` y `Authorization: ApiKey <value>`.
Si estás usando un espacio personalizado, usa el prefijo `/s/<space_name>/`.
Endpoints principales: `tools`, `agents`, `converse` y `conversations`.

### 2. Creación de Tools Optimizadas
Sigue estrictos patrones recomendados por Elastic:
- **Nombres con Namespaces**: Usa el formato `domain.action_entity` (ej: `security.search_logs`, `sales.get_order`).
- **Descripciones Nutridas**: Es el puente entre el LLM y la lógica. Deben contener: Propósito, Cuándo usarlo, Limitaciones y Relaciones ("Si falla, intenta usar <otra tool>").

### 3. Integración Model Context Protocol (MCP)
- **Kibana como Cliente MCP**: Agent builder permite agregar Tools MCP importándolas si configuras previamente un conector "MCP Action Type".
- **Kibana como Servidor MCP**: Puedes conectar clientes externos (Cursor, Claude) enviando requerimientos al endpoint `/api/agent_builder/mcp`.

### 4. Flujo de Chat Mantenible
Para mantener memoria de conversación, usa `/api/agent_builder/converse/async` pasando el `conversation_id`. Puedes listar chats históricos con `/api/agent_builder/conversations`.

## Logs y Mensajes de Error
- `Errores 404`: Verificar si se olvidó el prefijo de espacio de trabajo `/s/<space>`.
- `Error de LLM (Alucinación)`: Si el LLM no llama al tool esperado, la recomendación de Elastic es refactorizar la Descripción para hacer la directiva `Trigger` más estricta.

## Comandos del Usuario
- "¿Cómo creo un tool de Agent builder en la terminal?"
- "Muéstrame un pipeline para chatear con Agent Builder"
- "Ayúdame a nombrar las herramientas bajo el estándar de Agent Builder"

## Ejemplos
- **Uso rápido**: `examples/uso_rapido.md`
- **Uso detallado**: `examples/uso_detallado.md`
- **Uso avanzado**: `examples/uso_avanzado_mcp.md`
- **Uso con scripts**: `examples/uso_scripts.md`
- **Uso con workflows**: `examples/uso_workflows.md`

## Resources
Revisar el archivo `resources/knowledge-update-[fecha].md` para los payloads brutos extraídos desde la documentación de Kibana REST APIs.
