---
name: Elastic Workflow Library
description: Guía de sintaxis YAML avanzada, Control de Flujo, Action Types y automatización nativa (incluyendo Agentes LLM).
---

# Skill: Elastic Workflow Library

## Propósito
Permitir a los agentes diseñar, redactar e importar procesos de automatización declarativos en formato YAML (Workflows) que corren de forma nativa en Kibana. Estos workflows pueden integrarse con Elasticsearch, incidentes, llamar a Agentes LLM (AI) configurados y consumir APIs de terceros.

## Cuándo Usar
- Para automatizar tareas y alertas en Elastic Security u Observability.
- Para unir capacidades analíticas (ES|QL) con inteligencia artificial (Agent Builder) en un proceso secuencial.
- Al orquestar procesos controlados con manejo de errores, reintentos o paralelización de consultas.

## Instrucciones y Arquitectura

### 1. Estructura y Triggers
- Componentes obligatorios: `name`, `steps`.
- `triggers`: `manual`, `scheduled` (cron/every), o `alert` (disparado por Security).

### 2. Sintaxis y Templating (Liquid)
Además de `inputs` y `consts`, se dispone de contextos de tiempo real:
- `{{ env.HOME }}` para entorno local del servidor.
- `{{ now }}` para inyectar tiempo en ISO.
- `{{ event | json }}` transfiere en bruto un evento disparador al paso actual.

### 3. Action Types y AI Agents
Tipos clave y su sintaxis:
- `elasticsearch.esql.query`
- `kibana.cases`: Para gestionar Security Incidents.
- `kibana.post_agent_builder_converse`: Acción crítica para orquestar Agentes. Toma un `agent_id` y procesa un `input`.
- Flujos de control: `type: if` con la key `condition`, y `type: foreach` iterando `items: "{{ array }}"`.

### 4. Importación API y Buenas Prácticas
Para importar por REST, transformar siempre el string YAML en un payload `{"yaml": "..."}` mediante `jq -Rs`.

## Comandos del Usuario
- "Hazme un workflow que llame al agente financiero y le envíe las alertas"
- "Añade un condicional a mi archivo yaml para que el paso X solo corra si severidad es high"
- "Explica cómo pasar logs en bruto a un LLM en Kibana Workflows"

## Ejemplos
- **Básico HTTP**: `examples/ejemplo_basico_http.md`
- **Avanzado (ES|QL)**: `examples/ejemplo_avanzado_esql.md`
- **Integración IA (Agentes)**: `examples/ejemplo_invocacion_agente.md`
- **Despliegue Programático**: `examples/ejemplo_api_import.md`

## Resources
Revisar `resources/knowledge-update-2026-02-26.md` para sintaxis cruda de dependencias condicionales extraídas del repositorio principal.
