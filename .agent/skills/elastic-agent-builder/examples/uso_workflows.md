# Ejemplo: Workflows de Automatización Ostrator

## Contexto
Elastic Agent Builder puede ser el "Cerebro" al final de un Elastic Workflow (El motor de automatización nativo).

## Pasos Prácticos
1. Navega a `Stack Management > Elastic Workflows`.
2. Crea un workflow llamado `Auto Triage`.
3. El trigger debe ser una alerta del sistema (ej: Falla de base de datos) que entrega metadatos genéricos sobre el log.
4. Agrega un bloque tipo **Agent Builder Converse**. Configura que lea el campo `{{alert.summary}}`.
5. El bloque utilizará al Agente (ej. de SecOps), el agente utilizará sus **Tools** (ES|QL queries guardados) para enriquecer la data y cruzarla con los problemas históricos, y depositará la justificación en un ticket de Jira utilizando una integración de salida.
