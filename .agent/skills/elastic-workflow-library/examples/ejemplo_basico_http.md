# Ejemplo: Caso Básico HTTP y Variables

## Contexto
Flujo manual que toma una IP por `inputs`, usa la acción `http` para consultarla en un servicio externo de inteligencia de amenazas, y luego registra en consola.

## Código (YAML)
```yaml
name: "Validar IP en VT"
triggers:
  - type: manual

inputs:
  - name: target_ip
    type: string
    required: true

consts:
  vt_api_url: "https://www.virustotal.com/api/v3/ip_addresses"
  vt_api_key: "MASCARA_EN_SECRETS_KIBANA"

steps:
  - name: fetch_reputation
    type: http
    with:
      url: "{{ consts.vt_api_url }}/{{ inputs.target_ip }}"
      method: GET
      headers:
        x-apikey: "{{ consts.vt_api_key }}"
  
  - name: check_result
    type: console
    with:
      message: "Análisis para {{ inputs.target_ip }} completado. Score malicioso: {{ steps.fetch_reputation.output.body.data.attributes.last_analysis_stats.malicious }}"
```
