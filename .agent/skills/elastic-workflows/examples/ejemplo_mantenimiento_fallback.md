# Ejemplo Avanzado: Gestión de Índices con Fallback

## Contexto
Elimina un índice temporal y, si falla (por ejemplo, porque no existe), ejecuta un fallback para registrar el incidente en lugar de detener el flujo.

## YAML
```yaml
name: Maintenance Task with Fallback
enabled: true
consts:
  temp_index: "tmp-processing-index"

triggers:
  - type: scheduled
    with:
      every: 1d

steps:
  - name: delete_temp_index
    type: elasticsearch.indices.delete
    with:
      index: "{{ consts.temp_index }}"
    on-failure:
      fallback:
        - name: report_missing_index
          type: console
          with:
            message: "El índice {{ consts.temp_index }} no pudo ser borrado. Error: {{ steps.delete_temp_index.error }}"
        - name: continue_as_success
          type: console
          with:
            message: "Procediendo con el resto del mantenimiento..."

  - name: post_maintenance_check
    type: console
    with:
      message: "Mantenimiento diario finalizado."
```
