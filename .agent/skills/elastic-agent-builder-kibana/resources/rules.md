# Reglas Específicas de la API de Kibana Agent Builder

- **Headers Obligatorios**: NUNCA generes un script o ejemplo de consumo de esta API (si es mutación: POST/PUT/DELETE) sin incluir el header `kbn-xsrf: true`. Kibana rechazará las peticiones por protección CSRF sin él.
- **Espacios (Kibana Spaces)**: ASUME SIEMPRE que las APIs pueden correr en espacios que no sean el `default`. Si el usuario menciona un espacio, DEBES transmutar la URL base agregando: `/s/<nombre_espacio>/`.
- **JSON Payload Completo**: Al crear (POST) Agentes o Tools, respeta la anidación JSON oficial requerida (e.g. `configuration: { tools: [...] }`).
- **Autenticación**: Aconseja siempre el uso de `ApiKey` en el header `Authorization`, en minúsculas y capitalizado adecuadamente.
