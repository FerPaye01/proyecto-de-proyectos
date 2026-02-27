# Ejemplo Avanzado: Automatización con ES|QL y Alertas

## Contexto
Flujo programado. Ejecuta una consulta a Elasticsearch, y si detecta múltiples hosts con problemas en las últimas 24 horas, envía un resumen a Slack.

## Código (YAML)

```yaml
name: Daily Security Summary
triggers:
  - type: scheduled
    with:
      every: "1d"
      
consts:
  slack_webhook: "https://hooks.slack.com/services/T000.../B000..."

steps:
  - name: query_alerts
    type: elasticsearch.esql.query
    with:
      format: json
      query: |
        FROM .alerts-security.alerts-default
        | WHERE @timestamp > NOW() - 24 hours
        | STATS alert_count = COUNT(*) BY host.name
        | SORT alert_count DESC
        | LIMIT 10

  - name: notify_slack
    type: http
    with:
      url: "{{ consts.slack_webhook }}"
      method: POST
      body:
        text: "🔔 Daily Alert Summary: Encontramos {{ steps.query_alerts.output.values | size }} hosts con alertas críticas en las últimas 24h."
```
