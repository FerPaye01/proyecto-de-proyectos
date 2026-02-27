---
name: Elastic Workflows
description: Guía oficial de sintaxis YAML, pasos de IA, disparadores y manejo de errores para Workflows en Kibana.
---

# Skill: Elastic Workflows

## Propósito
Automatizar procesos operativos dentro de la suite Elastic utilizando el motor nativo de Workflows de Kibana. Este skill permite diseñar flujos que reaccionan a alertas, programan tareas de mantenimiento y utilizan IA para toma de decisiones rápida.

## Cuándo Usar
- Automatización de triage de alertas de seguridad.
- Tareas programadas de gestión de índices (borrado, snapshot, reindex).
- Enriquecimiento de datos usando Large Language Models (LLMs).
- Orquestación de agentes de IA cuando se requiere una lógica de pasos estructurada.

## Estructura YAML Base
```yaml
name: Título del Workflow
enabled: true
consts:
  target_index: "logs-security-*"
triggers:
  - type: alert
steps:
  - name: lookup_details
    type: elasticsearch.search
    with:
      index: "{{ consts.target_index }}"
      query:
        term: { "user.name": "{{ event.user.name }}" }
```

## Capacidades Avanzadas

### 1. Manejo de Errores (`on-failure`)
Configura resiliencia en tus flujos:
```yaml
steps:
  - name: external_api_call
    type: elasticsearch.request
    with:
      method: POST
      path: /_bulk
    on-failure:
      retry:
        max-attempts: 3
        delay: "10s"
      fallback:
        - name: log_critical_error
          type: console
          with:
            message: "Fallo tras 3 intentos: {{ steps.external_api_call.error }}"
```

### 2. Integración con IA
- **`ai.prompt`**: Consultas directas a LLM.
- **`ai.agent`**: Ejecutar agentes construidos con Agent Builder.

### 3. Plantillas dinámicas
- **Strings**: `{{ value }}`.
- **Objetos/Listas**: `${{ steps.search.output.hits.hits }}`.

## Mejores Prácticas
- **Usa Constantes**: Para IDs de conectores o nombres de índices que no cambian.
- **Logs de Consola**: Incluye pasos `type: console` para facilitar la depuración en el panel de monitoreo.
- **On-Failure Global**: Define una política de reintento por defecto en `settings`.

## Referencias
- Documentación oficial: `resources/knowledge-source.md`.
