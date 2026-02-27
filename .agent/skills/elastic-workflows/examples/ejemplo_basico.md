# Ejemplo Básico: Triage de Alertas de Seguridad

## Contexto
Este workflow se activa mediante una alerta, busca eventos relacionados en un índice de auditoría y registra un resumen en la consola.

## YAML
```yaml
name: Security Alert Triage
description: Enriquecimiento básico de alertas de seguridad
enabled: true
tags: ["security", "triage"]

triggers:
  - type: alert

steps:
  - name: fetch_audit_logs
    type: elasticsearch.search
    with:
      index: "audit-logs-*"
      query:
        match:
          user.name: "{{ event.user.name }}"
    on-failure:
      retry:
        max-attempts: 2
        delay: "5s"

  - name: summarize_with_ai
    type: ai.prompt
    with:
      prompt: |
        Analiza estos eventos para el usuario {{ event.user.name }}:
        ${{ steps.fetch_audit_logs.output.hits.hits }}
        Indica si hay un patrón de fuerza bruta.

  - name: log_analysis
    type: console
    with:
      message: "Análisis completado para {{ event.user.name }}. Resultado: {{ steps.summarize_with_ai.output }}"
```
