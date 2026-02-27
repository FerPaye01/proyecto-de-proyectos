# Ejemplo: Script Completo para Despliegue Config as Code

## Contexto
No uses el GUI de Kibana cuando administres un proyecto serio. Aplica tus Agentes de forma programática usando Python.

```python
import requests
import json
import os

KIBANA = "http://localhost:5601"
SPACE = "secops"
HEADERS = {
    "Authorization": "ApiKey " + os.environ["KIBANA_KEY"],
    "kbn-xsrf": "true"
}
BASE_URL = f"{KIBANA}/s/{SPACE}/api/agent_builder"

def deploy_agent(agent_json_path):
    with open(agent_json_path, 'r') as f:
        payload = json.load(f)
        
    try:
        r = requests.post(f"{BASE_URL}/agents", headers=HEADERS, json=payload)
        r.raise_for_status()
        print("✅ Agente", payload['id'], "desplegado.")
    except Exception as e:
         print(f"❌ Error desplegando: {r.text if 'r' in locals() else e}")

if __name__ == "__main__":
    deploy_agent("agent-secops.json")
```
