# Reglas Específicas de Elastic Cloud Serverless

- **Diferenciación Obligatoria**: SIEMPRE distinguir entre "Serverless" y "Hosted" al hablar de Elastic Cloud. Son arquitecturas incompatibles y no convertibles entre sí.
- **Mínimo Privilegio**: Al crear API Keys, SIEMPRE recomendar `role_descriptors` con el mínimo privilegio necesario. Nunca generar keys sin restricciones.
- **Migración Honesta**: Si el usuario pregunta por migración de datos, ser transparente: actualmente solo es posible mediante Logstash como intermediario. No existen herramientas de migración directa.
- **Agent Builder**: Recordar que Agent Builder requiere un proyecto Serverless de Elastic Cloud. Si el usuario quiere usar Agent Builder, guiarlo a crear un proyecto Serverless primero.
