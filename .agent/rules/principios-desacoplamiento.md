---
trigger: always_on
---

# 🔗 Principios de Desacoplamiento y Modularidad

Directrices obligatorias para estructurar el código garantizando bajo acoplamiento y alta cohesión.

---

## 1. Regla de Oro: Separación de Capas
Cualquier módulo del proyecto debe respetar la separación en tres capas distintas:

1. **Capa de Presentación (UI)**: Maneja vista, layout, eventos de usuario. **Prohibido** poner consultas de API, manipulación directa de storage o lógica pesada aquí.
2. **Capa de Dominio / Negocio**: Funciones puras con reglas de negocio. **Prohibido** importar elementos de UI o librerías externas de infraestructura directamente.
3. **Capa de Infraestructura / Adaptadores**: Llamadas HTTP, localStorage, cliente de base de datos. Se consumen desde la capa de dominio a través de abstracciones.

---

## 2. Reglas Prácticas de Código Desacoplado
- **Cero Imports Cruzados Circulares**: Un módulo B no debe importar un módulo A si A ya importa B.
- **Interfaces / Proveedores Agnósticos**: Los servicios deben aceptar contratos genéricos (ej. `guardar_Datos(clave, valor)`), sin importar si por debajo usa `IndexedDB`, `LocalStorage` o `Fetch`.
- **Máximo 150 líneas por archivo**: Si un archivo empieza a crecer, separar responsabilidades en utilidades o adaptadores independientes.
- **Inyección por Parámetros**: Las funciones deben recibir sus dependencias o datos como argumentos en lugar de leerlos de variables globales implícitas.
