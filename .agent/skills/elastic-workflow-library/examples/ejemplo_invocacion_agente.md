# Ejemplo: Invocación de Agente LLM (Agent Builder)

## Contexto
Este workflow se asocia a alertas de seguridad (`type: alert`). Extrae el payload en bruto de la alerta (`{{ event | json }}`) y se lo delega a un poderoso Agente provisto por *Elastic Agent Builder* para ejecutar el Triage o investigación correspondiente de forma síncrona.

## Código (YAML)

```yaml
version: "1"
name: 🤖 LLM Alert Triage Agent
enabled: true

triggers:
  - type: alert
  - type: manual

steps:
  - name: log_payload
    type: console
    with:
      message: "Procesando alerta entrante: {{ event | json }}"

  - name: invoke_triage_agent
    type: kibana.post_agent_builder_converse
    with:
      agent_id: alert.triage  # Agent previamente creado en Kibana
      input: "Analiza y devuelve priorización para esta alerta basándote en su schema de evento y tus custom instructions:\n\n{{ event | json }}"
    timeout: 600s

  - name: post_update_to_case
    type: kibana.cases
    condition: 'steps.invoke_triage_agent.output.message != ""'
    with:
      action: comment
      id: "{{ event.case_id }}"
      comment: "Triage LLM: {{ steps.invoke_triage_agent.output.message }}"
```
