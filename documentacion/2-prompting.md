# 🧠 Guía de Prompts para Aprender con IA — Síntesis para Principiantes

> Basada en el artículo *"37 AI Prompts for Academic Success"* de AI Agents Kit  
> Adaptada para alguien con perfil DAW/DAM que empieza a aprender IA

---

## ¿Por qué los prompts importan?

La mayoría de la gente usa la IA como un buscador glorificado: pregunta algo genérico y obtiene respuestas genéricas.

La diferencia entre un prompt malo y uno bueno es enorme:

| Prompt malo | Prompt bueno |
|---|---|
| "Ayúdame a escribir un ensayo" | "Actúa como tutor de escritura y guíame paso a paso en desarrollar mi tesis sobre X, dándome feedback específico sobre mi estructura de argumento" |

Los prompts bien estructurados usan un framework llamado **CO-STAR-I**, que básicamente es decirle a la IA:
- **Quién es** (rol)
- **Qué tiene que hacer** (objetivo)
- **Contexto** relevante
- **Cómo pensar** el problema (paso a paso)
- **Qué NO hacer** (restricciones)
- **En qué formato** responder

---

## Las 6 categorías de prompts (37 en total)

| Categoría | Prompts | Para qué sirve |
|---|---|---|
| Escritura académica e investigación | 8 | Ensayos, informes de laboratorio, revisiones literarias |
| Habilidades de estudio y aprendizaje | 5 | Memorización, comprensión lectora, repaso activo |
| Solicitudes y admisiones | 9 | Cartas de motivación, becas, internships |
| Comunicación y colaboración | 5 | Emails, trabajo en grupo, foros |
| Desarrollo personal | 5 | Productividad, estrés, finanzas, redes |
| Planificación y decisiones | 5 | Horarios, elección de carrera, presentaciones |

---

## CATEGORÍA 1: Escritura Académica e Investigación

### Prompt 1 — Asistente de Escritura de Ensayos

**Para qué sirve:** Te guía para estructurar un ensayo, desarrollar una tesis y mejorar tu argumento. Piensa en él como un tutor de redacción personal.

**Ejemplo de uso:** Tienes que escribir sobre las ventajas de React frente a otros frameworks y no sabes cómo estructurarlo.

```
# Asistente de Escritura de Ensayos

# Rol
Educador experimentado y diseñador de currículo

# Objetivo
Guíame paso a paso para escribir un ensayo bien estructurado sobre [TU TEMA].

# Proceso de pensamiento
1. Analiza el tema y el nivel del texto
2. Crea un esquema con introducción, desarrollo y conclusión
3. Ayúdame a desarrollar una tesis sólida
4. Revisa que los argumentos sean coherentes

# Restricciones
- No escribas el ensayo por mí, guíame
- Usa un lenguaje accesible, soy principiante
- Si mi argumento es débil, dímelo directamente

# Input del usuario
[PEGA TU BORRADOR O IDEA AQUÍ]
```

---

### Prompt 2 — Asistente para Papers de Investigación

**Para qué sirve:** Te acompaña en todo el proceso de un trabajo de investigación, desde elegir el tema hasta citar las fuentes.

**Lo importante:** La IA **no escribe el trabajo por ti**, sino que te enseña el proceso. Esto es clave para aprender de verdad.

```
# Input del usuario — rellena esto
- Asignatura/campo:
- Longitud requerida:
- Fase actual (elección de tema / investigando / redactando):
- Pregunta de investigación o área temática:
- Estilo de cita requerido (APA, MLA...):
- Deadline:
```

---

### Prompt 3 — Redactor de Informes de Laboratorio

**Para qué sirve:** Ciencias e ingeniería. Sigue la estructura IMRAD (Introducción, Métodos, Resultados, Análisis y Discusión).

**Restricción importante del prompt:** No inventar datos. Si el experimento falla, documentar por qué.

---

### Prompt 4 — Organizador de Revisión Bibliográfica

**Para qué sirve:** Nivel máster/postgrado. Te ayuda a sintetizar múltiples fuentes académicas y encontrar huecos en la literatura.

**Diferencia clave:** No es un resumen de fuentes, es un análisis de patrones y contradicciones entre ellas.

---

### Prompt 5 — Analizador de Casos de Estudio

**Para qué sirve:** Negocios, derecho, medicina. Aplica frameworks como SWOT (Fortalezas, Debilidades, Oportunidades, Amenazas) o IRAC (Issue, Rule, Application, Conclusion).

```
# Input del usuario
- Texto del caso:
- Disciplina (negocios / derecho / medicina):
- Preguntas específicas a responder:
- Framework analítico a usar (si te lo indican):
```

---

### Prompt 6 — Creador de Bibliografías Anotadas

**Para qué sirve:** Cada fuente necesita resumen + evaluación crítica + reflexión sobre su relevancia. Este prompt asegura que no te quedes solo en resumir.

**Usa el test CRAAP para evaluar fuentes:**
- **C**urrency (actualidad)
- **R**elevance (relevancia)
- **A**uthority (autoridad del autor)
- **A**ccuracy (precisión)
- **P**urpose (propósito / posible sesgo)

---

### Prompt 7 — Planificador de Tesis o TFM/TFG

**Para qué sirve:** Divide un proyecto de meses/años en fases manejables con hitos y fechas.

```
# Input del usuario
- Tipo de titulación:
- Disciplina:
- Tema de investigación:
- Fase actual (propuesta / investigando / redactando):
- Fecha esperada de entrega:
- Metodología (cualitativa / cuantitativa / mixta):
- Principales dificultades encontradas hasta ahora:
```

---

### Prompt 8 — Organizador de Apuntes

**Para qué sirve:** Te ayuda a dejar de transcribir pasivamente y empezar a tomar apuntes que realmente sirvan para estudiar.

**Métodos que puede recomendarte:**
- **Cornell:** Divide la hoja en columnas (notas, preguntas clave, resumen)
- **Mind Mapping:** Mapas mentales para conceptos relacionados
- **Charting:** Tablas para comparar información
- **Outline:** Estructura jerárquica de puntos principales

---

## CATEGORÍA 2: Habilidades de Estudio y Aprendizaje

### Prompt 9 — Generador de Preguntas de Repaso Activo

**Concepto clave:** El **repaso activo** (intentar recordar sin mirar los apuntes) es mucho más efectivo que releer.

Este prompt genera preguntas a distintos niveles según la **Taxonomía de Bloom**:

| Nivel | Ejemplo de pregunta |
|---|---|
| Recordar | ¿Qué es un componente en React? |
| Entender | ¿Por qué React usa un DOM virtual? |
| Aplicar | ¿Cómo crearías un componente que muestre una lista de tareas? |
| Analizar | ¿En qué se diferencia el estado local del estado global? |
| Evaluar | ¿Cuándo usarías Redux frente a Context API? |
| Crear | Diseña la arquitectura de una app de notas con React |

```
# Input del usuario
- Temas a repasar:
- Material de base (apuntes, capítulos del libro...):
- Formato del examen (tipo test, desarrollo, problemas...):
- Número de preguntas que necesitas:
```

---

### Prompt 10 — Creador de Técnicas de Memoria y Mnemotecnia

**Para qué sirve:** Vocabulario técnico, fechas, fórmulas, procesos. La IA crea recursos mnemotécnicos personalizados para tu contenido.

**Técnicas que puede sugerirte:**
- **Acrónimos** (CRUD = Create, Read, Update, Delete)
- **Palacio de la memoria** (asociar conceptos a lugares físicos)
- **Chunking** (agrupar información en bloques)
- **Repetición espaciada** (repasar con intervalos crecientes)

---

### Prompt 11 — Guía de Comprensión y Análisis de Textos

**Para qué sirve:** Cuando un texto académico se te atraganta. Te enseña a leerlo de forma activa: identificar el argumento central, evaluar evidencias y reconocer sesgos.

**Estrategia sugerida: SQ3R**
1. **S**urvey — escanea primero (títulos, resumen, conclusión)
2. **Q**uestion — formula preguntas antes de leer
3. **R**ead — lee buscando respuestas
4. **R**ecite — explica lo que leíste con tus palabras
5. **R**eview — repasa y conecta con lo que ya sabías

---

### Prompt 12 — Simplificador de Conceptos Complejos

**Basado en la Técnica Feynman:** Si no puedes explicar algo con palabras simples, es que no lo entiendes del todo.

El prompt genera **3 niveles de explicación** del mismo concepto:
- Explicación para un niño de 5 años (ELI5)
- Nivel bachillerato
- Nivel universitario

**Ejemplo aplicado a tu perfil:** Si no entiendes qué es una red neuronal, puedes pedirle que te lo explique primero como si fuera un juego, luego como matemáticas básicas, y finalmente con la notación técnica real.

---

### Prompt 13 — Analizador de Estilo de Aprendizaje y Estrategias Personalizadas

**Importante matiz del artículo:** Los "estilos de aprendizaje" visuales/auditivos/kinestésicos como categorías rígidas no tienen mucho respaldo científico. Lo que sí funciona es adaptar las estrategias al **tipo de contenido** y a tus **patrones personales** de concentración.

```
# Input del usuario
- Asignaturas que estudias más:
- Métodos actuales y su efectividad:
- Cuándo rindes mejor (hora del día, entorno):
- Qué ha funcionado bien antes:
- Qué no funciona nunca:
- Preferencia: solo o en grupo:
```

---

## CATEGORÍA 3: Solicitudes y Admisiones

### Prompt 14 — Ayuda para la Carta de Motivación (Personal Statement)

**Restricción clave:** La IA no escribe la carta por ti. Te guía para encontrar tu propia historia auténtica.

**Lo que funciona:** Una historia específica y real > una lista genérica de logros.

**Lo que NO funciona:** Temas trillados sin ángulo nuevo (victorias deportivas, viajes solidarios, el abuelo que falleció).

---

### Prompt 15 — Ayuda para Ensayos de Becas

**Diferencia con la carta de motivación:** Aquí debes demostrar alineación con los valores específicos de la beca, no solo contar tu historia.

```
# Input del usuario
- Nombre y organización de la beca:
- Pregunta/prompt del ensayo:
- Límite de palabras:
- Tus logros académicos:
- Actividades extracurriculares y liderazgo:
- Objetivos profesionales:
- Por qué necesitas esta beca:
```

---

### Prompt 16 — Guía de Solicitud a Posgrado

**Para másters y doctorados.** El Statement of Purpose es distinto a la carta de motivación: debe articular intereses de investigación concretos y alinearse con el trabajo de profesores específicos del programa.

---

### Prompt 17 — Asistente de Solicitud de Prácticas (Internship)

**Clave:** Personalizar cada aplicación. Nunca enviar la misma carta a todas las empresas.

```
# Input del usuario
- Sector/campo objetivo:
- Empresas o roles específicos de interés:
- Asignaturas y proyectos relevantes:
- Habilidades técnicas y blandas:
- Objetivos profesionales que cubriría esta práctica:
```

---

### Prompt 18 — Escritor de Solicitudes Erasmus / Estudios en el Extranjero

**Importante:** No presentar el intercambio como "turismo". Hay que justificar el encaje académico y la preparación cultural.

---

### Prompt 19 — Plantilla para Pedir Cartas de Recomendación

**Buenas prácticas:**
- Pedir con mínimo 3-4 semanas de antelación
- Proporcionar tu CV y un resumen de lo que quieres destacar
- Incluir los plazos exactos de cada solicitud
- Facilitar siempre la opción de que digan que no

---

### Prompt 20 — Ayuda para Solicitudes de Traslado de Universidad

**Lo que nunca se hace:** Hablar mal de la institución actual. La narrativa debe ser siempre hacia adelante: "busco X que aquí no puedo encontrar".

---

### Prompt 21 — Guía de Preparación para Exámenes

**Plan en fases (no todo el día antes):**
1. Revisión de contenido
2. Práctica activa (generar preguntas, resolver sin mirar)
3. Simulacro en condiciones reales
4. Repaso final

```
# Input del usuario
- Asignatura(s):
- Fecha del examen:
- Formato (tipo test, desarrollo, problemas...):
- Nivel actual de conocimiento (1-10):
- Horas disponibles por día:
- Temas/capítulos que cubre:
```

---

## CATEGORÍA 4: Comunicación y Colaboración

### Prompt 22 — Coordinador de Trabajos en Grupo y Resolución de Conflictos

**Herramienta clave: matriz RACI**
- **R**esponsible — quien hace la tarea
- **A**ccountable — quien responde si falla
- **C**onsulted — a quien se pide opinión
- **I**nformed — quien recibe actualizaciones

---

### Prompt 23 — Proveedor de Feedback para Revisión por Pares

**Framework de feedback constructivo:**
1. Señala algo específico que funciona bien (con ejemplo concreto)
2. Indica el problema principal (priorizado, no una lista infinita)
3. Da una sugerencia accionable

**Lo que nunca se hace:** "Está bien" sin más, ni centrarse solo en la gramática cuando el contenido tiene problemas de fondo.

---

### Prompt 24 — Escritor de Emails Académicos

**Reglas básicas:**
- Sin abreviaturas tipo chat ni emojis con profesores
- Asunto descriptivo y concreto
- Saludo formal ("Estimado/a Dr./Prof. [Apellido]")
- Ve al grano en el primer párrafo
- No exijas respuesta inmediata

```
# Input del usuario
- Destinatario (nombre, cargo, relación contigo):
- Propósito del email:
- Urgencia/plazo:
- Petición específica o información necesaria:
- Contexto relevante (asignatura, trabajo, reunión):
- Borrador que ya hayas escrito (si lo tienes):
```

---

### Prompt 25 — Respondedor de Foros y Debates Online

**Lo que marca la diferencia:** No escribir "Estoy de acuerdo" sin argumentar. Hay que extender la conversación: añadir evidencia, contraejemplo, pregunta que profundice.

---

### Prompt 26 — Preparación para Tutorías con Profesores

**Por qué ir preparado:**
- Los profesores recuerdan a los estudiantes que vienen con preguntas específicas
- Es la puerta de entrada a TFGs, cartas de recomendación y proyectos de investigación

**Lo que llevar siempre:**
- Lista de preguntas priorizadas
- Trabajo o ejercicio en el que estás atascado
- Lo que ya has intentado para resolverlo

---

## CATEGORÍA 5: Desarrollo Personal

### Prompt 27 — Coach Anti-Procrastinación y Productividad

**Insight clave del artículo:** La procrastinación no es vagancia. Es un problema de regulación emocional: evitamos tareas que generan ansiedad, aburrimiento o sensación de agobio.

**Técnicas concretas que puede sugerirte:**
- **Pomodoro:** 25 min de trabajo + 5 min de descanso
- **Timeboxing:** Asignar bloques de tiempo fijos a tareas
- **Implementation intentions:** "Si X ocurre, entonces haré Y" (planes concretos ante obstáculos)
- **Temptation bundling:** Asociar una tarea difícil con algo que te guste

---

### Prompt 28 — Guía de Gestión del Estrés y Bienestar

**Importante:** Este prompt **no reemplaza atención psicológica profesional**. Si el estrés interfiere seriamente con tu funcionamiento, el prompt lo indica y recomienda buscar ayuda.

**Técnicas inmediatas:**
- Técnica 5-4-3-2-1 (grounding: nombra 5 cosas que ves, 4 que tocas...)
- Respiración diafragmática
- Higiene del sueño

---

### Prompt 29 — Educación Financiera para Estudiantes

**Temas que cubre:** Presupuesto mensual, préstamos estudiantiles, tarjetas de crédito, ahorro de emergencia, maximizar becas.

```
# Input del usuario
- Ingresos mensuales:
- Gastos fijos:
- Deudas actuales (préstamo estudiantil, tarjetas...):
- Ahorros actuales:
- Objetivos a corto plazo (1-2 años):
```

---

### Prompt 30 — Construcción de Red Profesional (Networking)

**Cambio de mentalidad:** No es "conseguir contactos útiles". Es construir relaciones auténticas donde tú también aportas valor.

**Acción concreta para empezar:** La entrevista informacional — contactar a alguien para preguntarle sobre su experiencia profesional, sin pedir trabajo directamente.

---

### Prompt 31 — Estrategias para el Síndrome del Impostor

**Qué es:** Sentirte un fraude a pesar de tus logros reales. Es extremadamente común entre estudiantes de alto rendimiento.

**Ejercicio concreto del prompt:** Listar evidencias reales que contradicen los pensamientos de fraude. El cerebro tiende a ignorarlas; escribirlas ayuda.

---

## CATEGORÍA 6: Planificación y Toma de Decisiones

### Prompt 32 — Planificador de Horario de Estudio

**Principios basados en ciencia:**
- Bloques de estudio de máximo 90 minutos seguidos
- Mínimo 7-8 horas de sueño (no negociable para el rendimiento cognitivo)
- Respetar el cronotipo (si rindes mejor por la mañana o por la noche)

```
# Input del usuario
- Asignaturas y dificultad de cada una (1-5):
- Horario de clases:
- Trabajo o compromisos adicionales:
- Horas de máxima productividad:
- Próximas entregas y exámenes:
```

---

### Prompt 33 — Asesor de Elección de Carrera y Asignaturas

**Matiz importante:** El prompt no elige por ti. Te da un framework para que tú decidas con más información, considerando intereses, competencias, salidas y posgrado.

---

### Prompt 34 — Equilibrador de Actividades Extracurriculares

**Regla de oro:** Calidad sobre cantidad. Una actividad donde tienes responsabilidad real vale más que cinco donde eres un miembro pasivo.

**Framework de decisión:** Para cada actividad evalúa:
- ¿Qué objetivo sirve? (aprendizaje, red, disfrute, CV)
- ¿Cuántas horas reales requiere?
- ¿Cómo afecta a tu rendimiento académico y bienestar?

---

### Prompt 35 — Planificador de Año Sabático (Gap Year)

**Premisa:** Un año sabático bien planificado puede ser transformador. Uno sin estructura puede ser simplemente tiempo perdido.

**Componentes clave del plan:**
- Objetivos claros (¿qué quieres aprender o experimentar?)
- Presupuesto realista
- Gestión de la reserva de plaza universitaria
- Plan de reincorporación

---

### Prompt 36 — Guía de Planificación de Carrera Profesional

```
# Input del usuario
- Curso actual:
- Carrera o campo de estudio:
- Campos profesionales de interés:
- Valores y prioridades (salario, impacto, creatividad, conciliación...):
- Habilidades actuales:
- Experiencia previa (prácticas, proyectos...):
- Objetivos próximos 6-12 meses:
```

---

### Prompt 37 — Creador de Presentaciones

**Regla de diseño 6x6:** Máximo 6 líneas por diapositiva, máximo 6 palabras por línea. Si hay más texto que eso, el público lo lee en lugar de escucharte.

**Estructura narrativa efectiva:**
1. Hook (gancho inicial)
2. Problema que vas a abordar
3. Tu solución o argumento
4. Evidencia
5. Conclusión con llamada a la acción

---

## Buenas Prácticas Generales

### Qué hacer siempre

- **Sé específico.** Cuanto más contexto das, mejor respuesta obtienes
- **Indica tu nivel.** "Explícamelo como si fuera principiante en React" es mucho más útil que solo preguntar
- **Itera.** Si la primera respuesta no es perfecta, pide ajustes concretos
- **Usa ejemplos.** "Dame un ejemplo como el que aparece en el ejercicio 3 del tema 4"
- **Pide el razonamiento.** "Explícame por qué, no solo el resultado"

### Qué nunca hacer

- Pedir que haga el trabajo **por** ti en lugar de **contigo** (no aprendes nada y es fácil de detectar)
- Aceptar la primera respuesta sin verificarla
- Dar por válida información sin contrastar (la IA se equivoca)
- Usar un prompt genérico cuando puedes dar contexto específico

### El error más común

Preguntar "Explícame React" en lugar de "Explícame qué es el estado en React comparado con las props, usando un ejemplo de un formulario de login".

---

## Ejemplo Real: De Perdido a Productivo

> *Situación:* Estudiante con dificultades en biología molecular.  
> **Semana 1:** Usa el Prompt 12 (Simplificador de Conceptos) para entender la transcripción del ADN con analogías visuales  
> **Semana 2:** Usa el Prompt 9 (Generador de Preguntas) para crear 30 preguntas de repaso activo  
> **Semana 3:** Usa el Prompt 10 (Mnemotecnia) para memorizar las fases del ciclo celular  
> **Semana 4:** Usa el Prompt 21 (Preparación para Examen) para crear un plan de estudio de 5 días  
> **Resultado:** Pasa de suspender a obtener notable

---

## Adaptación a tu perfil DAW/DAM + Web

Con tu base de React, Vike y Tailwind, estos prompts te serán especialmente útiles para aprender IA:

| Situación | Prompt recomendado |
|---|---|
| Leer un paper técnico sobre redes neuronales y no entender nada | Prompt 11 (Comprensión de textos) + Prompt 12 (Simplificador) |
| Aprender conceptos como backpropagation o gradient descent | Prompt 12 con nivel "tengo base de JavaScript pero no de cálculo" |
| Memorizar terminología de ML | Prompt 10 (Mnemotecnia) |
| Preparar presentación sobre IA para clase | Prompt 37 (Presentaciones) |
| Organizarte para estudiar IA de forma autodidacta | Prompt 32 (Horario) + Prompt 27 (Productividad) |
| Escribir sobre un proyecto de IA | Prompt 1 (Escritura de ensayos) |

---

*Síntesis elaborada a partir del artículo original disponible en: https://aiagentskit.com/blog/student-prompts/*
