---
name: Código Desacoplado & Clean Architecture
description: Auditor y refactorizador para garantizar bajo acoplamiento, alta cohesión y separación estricta de responsabilidades.
---

# Skill: Código Desacoplado (Bajo Acoplamiento)

## Propósito
Garantizar que los módulos, clases y componentes del sistema mantengan bajo acoplamiento y alta cohesión, permitiendo reutilización, facilidad de refactorización y pruebas sin dependencias cruzadas rígidas.

## Cuándo Usar
- **Invocación manual**: Mediante `/desacoplar` o cuando el usuario pida "desacopla este módulo".
- **Al crear componentes nuevos**: Para definir interfaces o contratos claros entre servicios.
- **Refactorización**: Al detectar que un componente conoce demasiado sobre la implementación de otro.

---

## Directrices de Desacoplamiento (Patrones Clave)

### 1. Principio de Responsabilidad Única (SRP)
- Un archivo/módulo solo debe tener **una razón para cambiar**.
- La UI solo renderiza y captura eventos; no procesa cálculos complejos de negocio ni peticiones HTTP directas.

### 2. Inversión de Dependencias (DIP)
- Los módulos de alto nivel no deben depender de módulos de bajo nivel; ambos deben depender de **abstracciones/interfaces**.
- Ejemplo: Depender de una interfaz `Repositorio_Datos` en lugar de importar directamente la instancia de `LocalStorage` o `Axios`.

### 3. Inyección de Dependencias / Inyección de Configuración
- Pasar servicios o configuraciones como parámetros o propiedades, en lugar de instanciarlos internamente con `new` o imports duros dentro de la función.

---

## Checklist de Verificación de Acoplamiento

- [ ] ¿Un cambio en la base de datos o API obligaría a cambiar componentes visuales (HTML/CSS/JSX)?
- [ ] ¿El módulo importa librerías externas específicas directamente en la lógica de negocio sin una capa adaptadora?
- [ ] ¿Es posible reemplazar un servicio (ej. cambiar localStorage por API REST) cambiando solo 1 archivo de configuración?
- [ ] ¿El archivo supera las ~150 líneas debido a que realiza múltiples tareas no relacionadas?

---

## Formato de Refactorización

```markdown
### 🔗 Diagnóstico de Acoplamiento

#### 1. Dependencias Rígidas Detectadas
- ⚠️ **[Componente A]** depende directamente de **[Componente B / Librería X]**.

#### 2. Propuesta de Desacoplamiento
- Crear contrato/interfaz `[Nombre_Contrato]`.
- Mover la implementación específica a un adaptador `[adaptador_X.js]`.

#### 3. Código Refactorizado
```[lenguaje]
// Interfaz / Adaptador limpio
```
```

## Logs
- `LOG_INFO: "codigo-desacoplado: Evaluando nivel de acoplamiento en [modulo]"`
- `LOG_INFO: "codigo-desacoplado: Separando responsabilidades en capas distintas"`
