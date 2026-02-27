# Ejemplo: Uso con Scripts (Automatización CRUD Agent Builder)

## Contexto
Script de Python para crear y mantener sincronizadas las herramientas de Agent Builder leyendo desde archivos locales, ideal para pipelines CI/CD que gestionan herramientas como "Código como Infraestructura".

## Script

```python
import os
import glob
import json
import requests

KB_URL = os.environ.get("KIBANA_URL")
HEADERS = {"Authorization": f"ApiKey {os.environ.get('KIBANA_API_KEY')}"}

def sync_tools():
    for filepath in glob.glob("elastic_agents/tools/*.json"):
        with open(filepath, 'r') as f:
            tool_data = json.load(f)
            
        tool_id = tool_data.get("id")
        
        try:
            res = requests.post(f"{KB_URL}/api/agent_builder/tools", headers=HEADERS, json=tool_data)
            res.raise_for_status()
            print(f"✅ Herramienta {tool_id} sincronizada correctamente")
        except requests.exceptions.RequestException as e:
            if e.response.status_code == 400 and "already exists" in e.response.text:
                print(f"⚠️ Tool {tool_id} ya existe.")
            else:
                print(f"❌ Fallo al sincronizar {tool_id}: {e.response.text}")

if __name__ == "__main__":
    sync_tools()
```
