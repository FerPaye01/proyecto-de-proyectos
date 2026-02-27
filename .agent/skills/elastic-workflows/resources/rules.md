# Reglas Técnicas para Elastic Workflows

- **Manejo de Errores por Defecto**: TODO workflow destinado a producción debe incluir al menos una política global de reintento en `settings` o un bloque `on-failure` específico en pasos críticos.
- **Seguridad de Datos**: Al usar `elasticsearch.request`, valida siempre el `method` y los `headers` (ej: `Content-Type: application/json` o `application/x-ndjson` para bulk).
- **Consumo de Salidas**: Para acceder a la salida de un paso anterior, utiliza el prefijo `steps.<nombre_del_paso>.output`.
- **IA de Confianza**: Al integrar `ai.prompt`, instruye al modelo a devolver JSON si el siguiente paso requiere un procesamiento programático, y utiliza `${{ steps.prompt.output }}` para preservar el tipo de dato.
- **Nomenclatura**: Los nombres de los pasos deben ser descriptivos y usar `snake_case` (ej: `search_threat_intel`).
