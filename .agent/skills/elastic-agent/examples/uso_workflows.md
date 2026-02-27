# Ejemplo: Uso con Workflows

## Contexto
Los agentes de Elastic Agent Builder pueden ser orquestados e invocados desde **Elastic Workflows** para crear respuestas automatizadas frente a incidentes.  

## Flujo de Trabajo
1. Crea un **Elastic Workflow** que se dispara mediante una alerta (ej. *CPU Alta en nodo X*).
2. Dentro del Workflow, añade un paso del tipo "Invoke Agent Builder".
3. Pasa dinámicamente el mensaje alertado como prompt al agente (`"Analiza este servidor y encuentra si hay algún error relacionado: {{alert.context}}"`)
4. El Agente usa internamente su herramienta de `find_server_logs_tool` (ES|QL), lee los logs del servidor problemático, resume la falla y expone el resultado en la UI de alertas de Elastic Security de manera pre-evaluada.
