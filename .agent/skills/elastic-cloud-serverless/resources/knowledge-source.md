# Knowledge Source: Elastic Cloud Serverless

- **URLs Originales**: 
  - https://www.elastic.co/docs/deploy-manage/deploy/elastic-cloud/serverless
  - https://www.elastic.co/docs/deploy-manage/api-keys/serverless-project-api-keys
  - https://www.elastic.co/docs/deploy-manage/deploy/elastic-cloud/create-serverless-project
- **Fecha de Captura**: 2026-02-26

## Resumen de Contenido
Elastic Cloud Serverless es una solución completamente gestionada que abstrae la infraestructura de clústeres, nodos y data tiers. En lugar de gestionar estas piezas manualmente, el usuario crea "proyectos serverless" que Elastic escala automáticamente.

## Investigación Profunda Extraída

### 1. Arquitectura
- **Desacoplamiento Compute/Storage**: Las operaciones de búsqueda e indexación están separadas, permitiendo escalar independientemente cada tipo de carga de trabajo.
- **Auto-escalado en tiempo real**: Los recursos se adaptan automáticamente a picos de ingesta o consultas sin intervención manual.
- **Almacenamiento optimizado**: Los datos residen en almacenamiento general eficiente en costos, con una capa de caché para datos recientes y frecuentemente consultados.

### 2. Tres Tipos de Proyecto Serverless
| Tipo | Caso de Uso | URL de Inicio |
|------|-------------|---------------|
| **Elasticsearch Serverless** | Búsqueda vectorial, APIs, aplicaciones | `/solutions/search/get-started` |
| **Elastic Observability Serverless** | Logs, métricas, traces, APM | `/solutions/observability/get-started` |
| **Elastic Security Serverless** | SIEM, endpoint protection, analytics | `/solutions/security/get-started` |

### 3. API Keys en Proyectos Serverless
- **Tipos**: Personal API Keys (acceso externo) y Managed API Keys (tareas internas de Kibana).
- **Autenticación**: `Authorization: ApiKey ${API_KEY}` en el header de cURL.
- **Control de privilegios** mediante `role_descriptors` JSON:
```json
{
  "books-read-only": {
    "cluster": [],
    "indices": [
      {
        "names": ["books"],
        "privileges": ["read"]
      }
    ],
    "applications": [],
    "run_as": [],
    "metadata": {},
    "transient_metadata": { "enabled": true }
  }
}
```

### 4. Creación de Proyectos
- **Nuevos usuarios**: Trial gratuito de 14 días en `cloud.elastic.co/serverless-registration`.
- **Usuarios existentes**: Login en `cloud.elastic.co/login`, requiere rol `admin` o equivalente.
- **Incompatibilidad**: NO es posible convertir un proyecto Serverless a un despliegue Hosted (ni viceversa). Son arquitecturas diferentes.

### 5. Diferencias Clave vs Elastic Cloud Hosted
| Aspecto | Serverless | Hosted |
|---------|-----------|--------|
| Gestión de infraestructura | Automática (Elastic) | Manual (usuario) |
| Escalado | Automático en tiempo real | Manual o auto-scaling configurado |
| Pricing | Pay-per-usage | Por capacidad reservada |
| Upgrades | Automáticos | Controlados por el usuario |
| Data tiers | Abstraídos | Configurables (hot/warm/cold/frozen) |
| Conversión | No convertible a Hosted | No convertible a Serverless |

### 6. Migración de Datos
- Actualmente NO hay herramientas de migración directa.
- **Workaround**: Usar Logstash con plugins de input/output de Elasticsearch.
- Backups/restores bajo demanda: NO soportados aún.

### 7. Regiones Cloud Soportadas
Disponible en regiones selectas de AWS, GCP y Azure, con planes de expansión.

### 8. Facturación
Cada tipo de proyecto tiene su propio modelo de facturación basado en uso. Dimensiones de facturación documentadas en `/deploy-manage/cloud-organization/billing/serverless-project-billing-dimensions`.
