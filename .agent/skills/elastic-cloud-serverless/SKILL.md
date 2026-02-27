---
name: Elastic Cloud Serverless
description: Guía técnica sobre Elastic Cloud Serverless, incluyendo arquitectura, tipos de proyecto, API Keys, facturación y diferencias con Elastic Cloud Hosted.
---

# Skill: Elastic Cloud Serverless

## Propósito
Proporcionar al agente conocimiento profundo sobre el modelo Serverless de Elastic Cloud para guiar a usuarios en la creación de proyectos, configuración de API Keys, comprensión de la facturación y las diferencias arquitectónicas con el modelo tradicional de clústeres gestionados (Hosted).

## Cuándo Usar
- Cuando un usuario pregunte "¿Qué es Elastic Cloud Serverless?" o "¿Cuál es la diferencia con Elastic Cloud Hosted?".
- Al configurar el entorno base para ejecutar Agent Builder (requiere un proyecto serverless).
- Al crear API Keys para acceso programático a proyectos serverless.
- Cuando se discutan costos, escalado automático o regiones disponibles.

## Conceptos Clave

### 1. Arquitectura Serverless
- **Compute/Storage Decoupling**: Búsqueda e indexación escalan de forma independiente.
- **Auto-escalado**: Sin intervención manual. Los recursos se adaptan a picos en tiempo real.
- **Cache Layer**: Capa de caché para datos recientes sobre almacenamiento general eficiente.

### 2. Tres Tipos de Proyecto
- **Elasticsearch Serverless**: Para búsqueda vectorial, APIs y aplicaciones.
- **Elastic Observability Serverless**: Para logs, métricas, traces y APM.
- **Elastic Security Serverless**: Para SIEM, protección de endpoints y analytics con IA.

### 3. API Keys
- Dos tipos: **Personal** (acceso externo) y **Managed** (tareas internas de Kibana).
- Control granular de privilegios mediante JSON `role_descriptors`.
- Autenticación: `Authorization: ApiKey <encoded_key>`.

### 4. Limitaciones Importantes
- **No hay conversión** entre proyectos Serverless y despliegues Hosted.
- **No hay backups** bajo demanda (aún en desarrollo).
- **Migración** solo vía Logstash como intermediario.

## Instrucciones
- SIEMPRE diferencia claramente entre "Serverless" y "Hosted" cuando el usuario pregunte sobre Elastic Cloud. Son arquitecturas incompatibles.
- Al recomendar Serverless, menciona el trial gratuito de 14 días (`cloud.elastic.co/serverless-registration`).
- Para acceso programático, guía al usuario a crear una API Key con `role_descriptors` restringidos al mínimo privilegio necesario.

## Ejemplos
- **Crear API Key con privilegios**: `examples/crear_api_key.md`
- **Comparativa Serverless vs Hosted**: `examples/comparativa_serverless_hosted.md`

## Referencias
Revisa `resources/knowledge-source.md` para los detalles completos de la arquitectura y FAQ.
