---
name: Detector de Edge Cases
description: Auditor para identificar casos límite, estados inconsistentes, entradas atípicas y fallos potenciales en funciones o componentes.
---

# Skill: Detector de Edge Cases (Casos Límite)

## Propósito
Analizar una función, módulo o requerimiento para anticipar y documentar fallos en casos borde (edge cases) antes de que lleguen a producción.

## Cuándo Usar
- **Invocación manual**: Mediante el comando `/edge-cases` o cuando el usuario pregunte "¿qué casos límite se me escapan?".
- **Al diseñar lógica crítica**: Algoritmos de cálculo, pagos, procesamiento de archivos, autenticación o manejo de estado.

---

## Checklist de Análisis de Edge Cases

### 1. Entradas y Tipos de Datos (Input Boundaries)
- [ ] **Nulos / Undefined / Vacíos**: ¿Qué pasa si el parámetro es `null`, `undefined`, `""` o `[]`?
- [ ] **Valores fuera de rango**: Números negativos, cero, números gigantes (`MAX_SAFE_INTEGER`), decimales inesperados.
- [ ] **Formatos extremos**: Strings extremadamente largos, caracteres especiales (UTF-8, emojis, SQL injection, scripts).
- [ ] **Colecciones**: Listas de 0 elementos, 1 elemento, o > 1,000,000 elementos (memoria/paginación).

### 2. Estado y Concurrencia
- [ ] **Fallos parciales**: ¿Qué pasa si falla la llamada de red a mitad de un proceso multi-paso?
- [ ] **Race Conditions**: Solicitudes concurrentes disparadas al mismo tiempo (doble clic en guardar).
- [ ] **Estado previo invalido**: La función asume que un estado local existe pero fue limpiado/resetear.

### 3. Entorno y Persistencia
- [ ] **Husos horarios y fechas**: Transiciones UTC, años bisiestos, formato ISO local.
- [ ] **Cuotas y Límites**: Disco lleno, localStorage agotado, token expirado justo en el envío.

---

## Formato de Salida

```markdown
### 🛡️ Reporte de Edge Cases Detectados

#### 1. Casos Críticos (Pueden romper la aplicación)
- ⚠️ **[Caso]**: [Descripción del escenario]
  - **Consecuencia**: [Crash / Corrupción de datos]
  - **Mitigación Sugerida**: [Código / Validación defensiva]

#### 2. Casos Moderados (Inconsistencias de UI o UX)
- 🟡 **[Caso]**: [Descripción]
  - **Mitigación**: [Manejo de valor por defecto o mensaje]

#### 3. snippet de Código Defensivo Recomendado
```[lenguaje]
// Ejemplo de validación para prevenir los edge cases principales
```
```

## Logs
- `LOG_INFO: "detector-edge-cases: Analizando casos borde para [nombre_funcion]"`
- `LOG_INFO: "detector-edge-cases: Identificados X casos límite críticos"`
