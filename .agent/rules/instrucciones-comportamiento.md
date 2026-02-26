---
trigger: always_on
---

# 📜 Instrucciones de Comportamiento

Patrones de trabajo y convenciones del proyecto.

---

## Naming Conventions
- Todos los nombres de **variables, funciones, clases, archivos y directorios** deben estar en **INGLÉS**.
- El texto visible para el usuario final (UI) debe estar en **ESPAÑOL**.

### Variables y Funciones
- Variables: `camelCase` → `userData`, `flashcardList`
- Funciones: `camelCase` → `getFlashcard()`, `saveData()`
- Clases: `PascalCase` → `FlashcardManager`, `UserService`
- Constantes: `SCREAMING_SNAKE_CASE` → `MAX_RETRIES`, `API_URL`

### Archivos y Directorios
- Directorios: `kebab-case` o minúsculas → `components/`, `pages/`, `utils/`, `styles/`
- Componentes: `camelCase` con extensión → `flashcardList.js`
- Estilos: `camelCase` con extensión → `flashcardList.css`
- Utilidades: `camelCase` con extensión → `logger.js`

### Comentarios
- En **español** (para legibilidad del equipo)
- Breves y directos
- Un comentario por función explicando el propósito

---

## Logging Obligatorio

Usar el sistema de logs en `src/utils/logger.js`:

```javascript
import { 
  logDebug, 
  logInfo, 
  logSequence, 
  logWarn, 
  logError 
} from './utils/logger.js'
```

| Función | Uso | Ejemplo |
|---------|-----|---------|
| `logDebug()` | Detalles técnicos internos | `logDebug('Valor de x', x)` |
| `logInfo()` | Información general | `logInfo('Proceso iniciado')` |
| `logSequence()` | Flujo de ejecución | `logSequence('Cargando datos', 'API')` |
| `logWarn()` | Situaciones inesperadas | `logWarn('Dato faltante')` |
| `logError()` | Errores críticos | `logError('Fallo', error)` |

---

## Sin Tests Formales

- ❌ No crear archivos de test
- ✅ Validar funcionalidad directamente en desarrollo
- ✅ Usar logs para debugging
- ✅ Probar manualmente en navegador

---

## Estructura de Código

### Imports
```javascript
// 1. Logger siempre primero
import { logInfo, logSequence } from './utils/logger.js'

// 2. Dependencias externas
import { algo } from 'libreria'

// 3. Componentes locales
import { myComponent } from './components/myComponent.js'
```

### Funciones
```javascript
// Comentario breve del propósito
function myFunction(parameterOne) {
  logSequence('Ejecutando myFunction', parameterOne)
  
  // Lógica simple y directa
  const result = parameterOne * 2
  
  logInfo('Resultado obtenido')
  return result
}
```
