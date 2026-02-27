# Ejemplo: Flujo Arquitectónico (Three-Way Call)

## Contexto
Cuando diseñes sistemas SOAR (Security Orchestration, Automation and Response) o AI DevOps usando Elastic, debes entender por qué el Agente no le dispara un evento REST a tu computadora. Los servidores detrás de firewalls (NAT) no pueden recibir entrada.

## Paso a Paso del Polling Asíncrono
1. **User a Kibana (Agent)**: "Detén el contenedor sospechoso".
2. El Agente nota que tiene disponible la herramienta externa `call_ai_runner_tool`.
3. El Agente dispara la herramienta pasando el JSON: `{ "runner_id": "host-1", "action": "docker_stop..." }`.
4. El backend de Elastic ejecuta un **Kibana Workflow** que guarda ese JSON en un índice de Elasticearch llamado `distributed-tool-requests` y retorna al Agente un `exec_id`.
5. El **Runner Local** (que hace polling cada 0.5s a Elasticsearch) ve el documento en el índice para él. Lo descarga, corre MCP localmente y apaga el docker.
6. El **Runner Local** vuelve a conectarse y hace GET con el resultado, guardándolo en `distributed-tool-results`.
7. El **Kibana Agent**, que no paró de hacer polling local preguntando por su `exec_id`, lee el resultado y te contesta: "Contenedor apagado, señor".
