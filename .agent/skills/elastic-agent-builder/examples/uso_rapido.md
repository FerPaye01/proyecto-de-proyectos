# Ejemplo: Uso Rápido (API Converse)

## Contexto
Llamada API mínima con cURL para enviar una pregunta síncrona a un Agente ya creado en un Espacio específico de Kibana.

## Código
```bash
curl -X POST "https://TU_KIBANA_URL/s/mi_espacio/api/agent_builder/converse" \
  -H "Authorization: ApiKey TU_API_KEY" \
  -H "kbn-xsrf: true" \
  -H "Content-Type: application/json" \
  -d '{ 
    "input": "Resumen de alertas de cpu en los últimos 30 min", 
    "agent_id": "security-ai-agent"
  }'
```

## Salida Esperada
El backend retornará el historial de pasos (como identificó la intención, qué herramientas usó) y el "message" final formateado en Markdown.
