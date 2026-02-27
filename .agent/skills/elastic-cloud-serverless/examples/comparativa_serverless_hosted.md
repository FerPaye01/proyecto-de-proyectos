# Ejemplo: Comparativa Serverless vs Hosted

## Contexto
Cuando un usuario pregunte "¿Qué modelo de Elastic Cloud me conviene?", usa esta tabla para orientarlo.

## Tabla Comparativa

| Aspecto | Serverless | Hosted |
|---------|-----------|--------|
| **Gestión** | Completamente automática | Manual (nodos, tiers, versiones) |
| **Escalado** | Automático en tiempo real | Auto-scaling configurable o manual |
| **Pricing** | Pay-per-usage | Capacidad reservada |
| **Upgrades** | Automáticos y transparentes | Controlados y programados por el usuario |
| **Data Tiers** | Abstraídos (caché + almacenamiento general) | Configurables (hot/warm/cold/frozen) |
| **Conversión** | ❌ No convertible a Hosted | ❌ No convertible a Serverless |
| **Backups** | Automáticos (sin restore manual aún) | Snapshots configurables |
| **Trial** | 14 días gratis | 14 días gratis |
| **Regiones** | AWS, GCP, Azure (selectas) | AWS, GCP, Azure (amplio) |

## Cuándo Recomendar Serverless
- El usuario NO quiere gestionar infraestructura.
- El caso de uso es nuevo y no se conoce la escala futura.
- Se necesita empezar rápido con un trial.
- Se usa Agent Builder (requiere Serverless).

## Cuándo Recomendar Hosted
- El usuario necesita control granular sobre nodos y data tiers.
- Hay requisitos de backup/restore manual.
- Se necesita compatibilidad con regiones no disponibles en Serverless.
