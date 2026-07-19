---
trigger: always_on
---

# 🔧 Principios de Simplicidad

Reglas duras de código que NO se negocian.

---

## Cero Ambigüedad

- ❌ **Prohibido usar marcadores temporales**: `# TODO`, `// placeholder`, `# implementar luego`, `pass` sin razón
- ❌ **Prohibido dejar funciones vacías** o con lógica incompleta sin marcar explícitamente
- ✅ **Las implementaciones deben ser funcionales y completas** al momento de ser escritas
- ✅ **Type hints obligatorios en Python**: todas las variables y firmas de funciones deben tener anotaciones de tipo

---

## 🪜 Escalera de Simplicidad y Reuso (Destilado Ponytail)

Cuando el requerimiento o plan ya esté definido, antes de escribir código nuevo el agente **DEBE** recorrer obligatoriamente la siguiente escalera:

1. **1. ¿Es estrictamente necesario que exista?** $\rightarrow$ Si no impacta el valor directo del requerimiento, se omite (YAGNI).
2. **2. ¿Ya existe en este proyecto?** $\rightarrow$ Reutilizar funciones, helpers o utilidades existentes. No duplicar lógica.
3. **3. ¿La librería estándar o la plataforma nativa lo resuelve?** $\rightarrow$ Usar funciones nativas (ej. `<input type="date">`, `fetch`, `Math`, stdlib de Python/JS) antes de añadir paquetes externos.
4. **4. ¿Ya hay una dependencia instalada que lo resuelva?** $\rightarrow$ Reutilizar las librerías del `package.json` o `requirements.txt`.
5. **5. Solución Mínima Funcional**: Si los pasos 1-4 no aplican, escribir la mínima cantidad de código que cumpla el requerimiento sin sacrificar seguridad, accesibilidad ni validaciones de borde.

---

## Código Directo

- ❌ **Sin try-catch redundantes** - Solo manejar errores críticos
- ❌ **Sin abstracciones innecesarias** - No crear clases para todo
- ❌ **Sin sobreingeniería** - Resolver el problema actual, no el futuro
- ✅ **Código legible** - Si necesita comentario extenso, simplificar

### Ejemplo Correcto
```javascript
// Obtiene flashcard por ID
function getFlashcard(id) {
  logSequence('Buscando flashcard', id)
  return flashcards.find(f => f.id === id)
}
```

### Ejemplo Incorrecto
```javascript
// Demasiadas abstracciones innecesarias
class FlashcardRepository {
  constructor(dataSource) {
    this.dataSource = dataSource
  }
  
  async findById(id) {
    try {
      const result = await this.dataSource.query(...)
      if (!result) throw new NotFoundError(...)
      return new FlashcardEntity(result)
    } catch (error) {
      throw new RepositoryError(error)
    }
  }
}
```

---

## Archivos Pequeños

- **Una responsabilidad por archivo**
- **Máximo ~100-150 líneas** por archivo
- Si crece más → dividir en archivos más pequeños

### Estructura de Carpetas
```
src/components/
├── flashcard.js          # Componente de flashcard
├── flashcard.css         # Estilos de flashcard
├── flashcardList.js      # Lista de flashcards
└── flashcardList.css     # Estilos de lista
```

---

## Documentación Mínima

- ✅ **Un comentario breve por función** - Solo el propósito
- ❌ **No documentar lo obvio** - El código debe ser autoexplicativo
- ❌ **No JSDoc extenso** - Solo si es API pública

### Ejemplo
```javascript
// Guarda flashcard en localStorage
function saveFlashcard(flashcard) {
  // ... código simple
}
```

---

## Manejo de Errores Simple

Solo para errores **críticos** que el usuario debe ver:

```javascript
function loadData() {
  const data = localStorage.getItem('flashcards')
  
  if (!data) {
    logWarn('No hay datos guardados')
    return []  // Retornar valor por defecto, no lanzar error
  }
  
  return JSON.parse(data)
}
```

---

## Análisis de Producto como Sistema (Evaluación Silenciosa)

**Regla siempre activa (interna)**: Antes de implementar cualquier tarea, el agente evalúa mentalmente:

1. **Cuestionar la precisión del requerimiento** — ¿Es lo suficientemente específico?
2. **Identificar edge cases de negocio** — ¿Qué pasa si el dato llega vacío, duplicado o en formato inesperado?
3. **Señalar riesgos de diseño** — ¿Esta solución genera deuda técnica inmediata?
4. **Validar coherencia con el sistema** — ¿Esta pieza encaja sin romper lo existente?

### Modo de Aplicación
- **En Ejecución Directa (Plan Definido / Tarea Concreta)**: Se aplica de forma **100% SILENCIOSA** internamente. El agente escribe código defensivo y limpio directamente sin hacer cuestionarios ni demorar al usuario.
- **En Etapa -1 / Bajo Demanda**: Se activa el reporte público si el usuario pide explícitamente auditar o explorar alternativas antes de programar.
