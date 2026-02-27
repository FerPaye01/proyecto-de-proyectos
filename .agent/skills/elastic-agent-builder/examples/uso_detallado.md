# Ejemplo: Uso Detallado de Creación de Tools (Gestión de Contexto y Relaciones)

## Contexto
Los Agentes LLM deciden qué Tool usar basándose **exclusivamente** en los nombres y descripciones. Elastic Agent Builder exige ciertas convenciones.

## Convención de Naming (Namespaces)
En lugar de nombrar una herramienta como `buscar_ticket`, utiliza namespaces de dominio y acción:
`support.search_tickets` o `finance.get_transaction`

## Múltiples Instrucciones (Core, Trigger, Limitations)
El JSON enviado a la API `/api/agent_builder/tools` debe tener una propiedad `description` muy completa para evitar alucinaciones.

```json
{
  "id": "support.search_articles",
  "type": "esql",
  "description": "Searches the internal Knowledge Base for technical support articles. \n\nTrigger: Use this tool when a user asks about error codes, troubleshooting steps, or product configurations.\nLimitations: Returns a maximum of 3 articles. Data is updated daily.\nNote: If this tool returns irrelevant results, try the support.search_tickets tool to see how similar historical issues were resolved.",
  "configuration": {
    "query": "FROM support_kb | WHERE category == ?param1 | LIMIT 3",
    "params": {
       "param1": {
         "type": "keyword",
         "description": "Categoria principal (hardware, software, network)"
       }
    }
  }
}
```
