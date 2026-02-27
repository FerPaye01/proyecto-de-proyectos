# Ejemplo: Crear API Key con Privilegios Restringidos

## Contexto
Cuando necesites acceso programático a un proyecto serverless (por ejemplo, para que un Runner de Augmented Infrastructure se conecte), debes crear una API Key con los privilegios mínimos necesarios.

## Paso 1: Crear la API Key (vía UI)
1. Ir a **Project Settings → Management → API keys**.
2. Clic en **Create API key**.
3. En **Control security privileges**, pegar el siguiente JSON de `role_descriptors`:

```json
{
  "agent-runner-role": {
    "cluster": ["monitor"],
    "indices": [
      {
        "names": ["distributed-*"],
        "privileges": ["read", "write", "create_index"]
      }
    ],
    "applications": [],
    "run_as": [],
    "metadata": {},
    "transient_metadata": { "enabled": true }
  }
}
```

## Paso 2: Usar la API Key
```bash
ES_URL="https://tu-proyecto.es.us-east-1.aws.elastic.cloud"
API_KEY="tu_api_key_encoded"

curl "${ES_URL}/_cluster/health" \
  -H "Authorization: ApiKey ${API_KEY}"
```

> **Nota**: Las API Keys son para acceso programático exclusivamente. No usarlas para autenticación en navegador web.
