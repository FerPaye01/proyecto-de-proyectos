---
name: Problem Framing
description: Refina una idea antes de la Etapa 0 generando posibles enfoques de solución sin decidir cuál es el correcto.
---

# Skill: Problem Framing (Etapa -1)

## Propósito
Refinar la idea inicial del usuario para alimentar la Etapa 0.

No investigar.
No validar.
No diseñar la solución.
No elegir una alternativa.
Solo ampliar el espacio de posibles enfoques desde la experiencia de un ingeniero senior.

## Cuándo Usar (Bajo Demanda Únicamente)
- **Inicio de Proyecto sin Plan (Etapa -1)**: Únicamente cuando el usuario exprese una idea vaga desde cero y no tenga ningún plan o investigación previa.
- **Invocación Manual / Sesión de Ideación**: Cuando el usuario pida explícitamente "ayúdame a explorar opciones" o use comandos de lluvia de ideas.
- **NO USAR**: Cuando el usuario ya traiga un plan, requerimiento estructurado, investigación o instrucción directa de construcción.

---

## Instrucciones

Antes de formular la respuesta, **leer obligatoriamente el archivo personal [perfil.md](file:///data/proyectos/proyecto_de_proyectos/.agent/perfil/perfil.md) del usuario**:
- Identificar el nivel de expertise en las tecnologías mencionadas.
- Calibrar la jerga técnica y la complejidad del planteamiento de alternativas al nivel del usuario.

A partir de la idea del usuario, ejecuta los siguientes pasos:

### 1. Reformula la intención
Resume en una o dos frases qué problema parece querer resolver.

### 2. Detecta supuestos
Identifica los supuestos implícitos de la idea.

### 3. Genera enfoques alternativos
Propón entre **3 y 7 enfoques** que podrían resolver el mismo problema.
Los enfoques deben ser conceptualmente distintos (no pequeñas variantes).
No descartes la idea del usuario: inclúyela como una alternativa si corresponde.

### 4. Señala diferencias
Para cada enfoque indica brevemente:
- Idea principal
- Principal ventaja
- Principal riesgo
- Cuándo tendría sentido elegirlo

---

## Formato de Salida

### Problema Refinado
[Una o dos frases sobre el problema real a resolver]

### Supuestos Detectados
- [Supuesto implícito 1]
- [Supuesto implícito 2]
- ...

### Posibles Enfoques

**Enfoque A (Idea del usuario / Enfoque base)**
- **Idea principal**: ...
- **Principal ventaja**: ...
- **Principal riesgo**: ...
- **Cuándo tendría sentido elegirlo**: ...

**Enfoque B**
- **Idea principal**: ...
- **Principal ventaja**: ...
- **Principal riesgo**: ...
- **Cuándo tendría sentido elegirlo**: ...

**Enfoque C**
- ...

### Recomendación para la Etapa 0
Estas alternativas pueden convertirse en hipótesis independientes para ser investigadas mediante el metaprompt de la Etapa 0.

---

## Logs
- `LOG_INFO: "problem-framing: Calibrando jerga con perfil del usuario"`
- `LOG_INFO: "problem-framing: Reformulando intención"`
- `LOG_INFO: "problem-framing: Detectando supuestos implícitos"`
- `LOG_INFO: "problem-framing: Generando enfoques alternativos de solución"`
