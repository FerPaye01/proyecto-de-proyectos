# Ejemplo de API Import (Bash + Node)

## Contexto
Instrucción técnica obligatoria: Para importar a Kibana, el engine rechaza archivos YAML en RAW. Siempre debe encapsularse en la key `yaml` dentro de un cuerpo `application/json`. 

## En Bash via cURL + JQ
```bash
cat my_workflow.yaml | \
  jq -Rs '{yaml: .}' | \
  curl -X POST "https://TU_KIBANA/api/workflows" \
    -H "kbn-xsrf: true" \
    -H "x-elastic-internal-origin: Kibana" \
    -H "Content-Type: application/json" \
    -H "Authorization: ApiKey TU_KEY" \
    -d @-
```

## En Node.js (Fetch)
```javascript
const fs = require('fs');

async function importWorkflow(filepath) {
  const yaml = fs.readFileSync(filepath, 'utf-8');
  
  const response = await fetch('https://TU_KIBANA/api/workflows', {
    method: 'POST',
    headers: {
      'kbn-xsrf': 'true',
      'x-elastic-internal-origin': 'Kibana',
      'Content-Type': 'application/json',
      'Authorization': `ApiKey TU_API_KEY`
    },
    body: JSON.stringify({ yaml })
  });
  
  if (response.ok) {
     const data = await response.json();
     console.log(`✅ Workflow importado ID: ${data.id}`);
  }
}
```
