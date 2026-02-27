# Ejemplo: Diseño Básico de Agente con Agent Builder

## Contexto
Este es un caso de uso conceptual donde el usuario desea construir un asistente que busque en la base de datos de incidentes o manuales almacenados en Elasticsearch.

## Entrada
- Un índice de Elasticsearch que contiene incidentes.
- Un LLM configurado en Elastic Cloud (o por un conector usando OpenAI, Anthropic, Gemini, etc).

## Proceso
1. **Tool Creation**: El desarrollador del skill define una herramienta llamada `search_incidents_tool` que corre una query de Elasticsearch.
2. **Agent Setup**: Se crea un nuevo Agente en Elastic Agent Builder.
3. **Instruction Assign**: Se le indica al LLM: *"Eres un asistente de IT. Utiliza la herramienta 'search_incidents_tool' para buscar errores antiguos antes de responder al usuario."*
4. **Agent Chat**: El usuario final pregunta: "¿Ha ocurrido el error 500 en la base de datos de facturación recientemente?"

## Salida Esperada
El Agente llama a la base de datos a través de la herramienta provista, recibe los resultados relevantes con contexto fundamentado (RAG) respetando los permisos, y contesta resumiendo lo encontrado mediante lenguaje natural.
