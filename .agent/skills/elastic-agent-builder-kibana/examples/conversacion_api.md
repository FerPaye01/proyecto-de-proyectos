# Ejemplo: Conversar con el Agente Vía API

## Contexto
Permite a sistemas externos (Discord, Slack, Scripts locales) realizar preguntas al Agente y recibir la respuesta que el LLM genera tras ejecutar sus herramientas.

## Código (cURL Síncrono)

```bash
curl -X POST "${KIBANA_URL}/api/agent_builder/converse" \
  -H "Authorization: ApiKey ${API_KEY}" \
  -H "kbn-xsrf: true" \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": "secops-triage-agent",
    "input": "¿Alguien bloqueó al usuario admin en la última hora?"
  }'
```

*Nota: Para mantener un historial lógico (memoria chat), el cuerpo JSON también soporta `"conversation_id": "tu_id"` para añadir el mensaje al histórico del hilo.*
