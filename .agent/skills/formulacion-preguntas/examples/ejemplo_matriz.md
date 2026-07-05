# Ejemplo: Matriz de Inventario (Problem Framing)

**Contexto**: El usuario pide una "matriz dinámica" para ropa y calzado.

### Problema Refinado
Se busca crear un sistema de visualización y edición bidimensional de stock de productos con variantes complejas (ropa con tallas S, M, L y calzado con tallas 38-42).

### Supuestos Detectados
- Se asume que el usuario quiere ver ropa y calzado en una única interfaz unificada.
- Se asume que la interfaz debe permitir la edición directa en la misma cuadrícula.
- Se asume que la estructura de variantes es estática o de baja volatilidad.

### Posibles Enfoques

**Enfoque A (Idea original: Matriz Unificada Dinámica)**
- **Idea principal**: Diseñar una sola matriz genérica que se adapte en caliente a las columnas requeridas por el tipo de producto seleccionado.
- **Principal ventaja**: Vista consolidada en una sola pantalla.
- **Principal riesgo**: Matrices vacías cruzadas (sparse matrix) muy complejas de gestionar visualmente.
- **Cuándo tendría sentido elegirlo**: Si el inventario es pequeño y la experiencia tipo hoja de cálculo es prioritaria.

**Enfoque B (Vistas Separadas por Categoría)**
- **Idea principal**: Dividir la UI en pestañas o secciones independientes para Ropa y Calzado, optimizando cada matriz para su tipo de variante específico.
- **Principal ventaja**: Diseño visual limpio, columnas fijas y predecibles.
- **Principal riesgo**: Pérdida de la vista global del catálogo.
- **Cuándo tendría sentido elegirlo**: Si las categorías tienen variantes muy dispares y no hay necesidad de editarlas simultáneamente.

**Enfoque C (Listado Plano con Filtros Multidimensionales)**
- **Idea principal**: Abandonar la matriz y usar una tabla plana de productos con filtros rápidos y un modal o panel lateral para editar las variantes de stock.
- **Principal ventaja**: Escalabilidad infinita ante nuevos tipos de variantes.
- **Principal riesgo**: Experiencia de usuario menos "directa" que una matriz tipo Excel.
- **Cuándo tendría sentido elegirlo**: Si el catálogo crece constantemente con nuevas categorías y atributos dinámicos.

### Recomendación para la Etapa 0
Cualquiera de estos enfoques se puede seleccionar como base para la arquitectura y definir en la Etapa 0.
