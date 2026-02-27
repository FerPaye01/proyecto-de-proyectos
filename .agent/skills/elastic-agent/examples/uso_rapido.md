# Ejemplo: Uso Rápido (Snippet Básico API Agent)

## Contexto
Snippet de código mínimo para probar la conectividad e instanciar una pregunta al modelo de IA en Elastic usando la API de Agent Builder.

## Código
```python
import requests

KB_URL = "https://TU_KIBANA_URL"
HEADERS = {"Authorization": "ApiKey TU_KEY"}

chat_response = requests.post(
    f"{KB_URL}/api/agent_builder/converse",
    headers=HEADERS,
    json={
        "agent_id": "financial_assistant", # o default
        "messages": [
            {"role": "user", "content": "¿Qué es el riesgo actual?"}
        ]
    }
)

print(chat_response.json())
```
