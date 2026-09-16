# Semana 7 - Modelos de clasificación y regresión

Esta semana se centró en la comparación de modelos de aprendizaje supervisado para resolver problemas de clasificación y regresión. A través de tres ejercicios prácticos, se exploraron algoritmos fundamentales, sus diferencias conceptuales, sus métricas de desempeño y su capacidad para generalizar en distintos tipos de datos.

## Objetivo general

Aprender a:

- seleccionar modelos adecuados según el tipo de problema,
- comparar algoritmos con métricas reales,
- interpretar resultados de precisión y error,
- entender cuándo un modelo funciona bien y cuándo no,
- reconocer la importancia de la validación y la visualización de resultados.

## Actividades desarrolladas

### 1. Comparación de clasificadores sobre el conjunto Iris

Se trabajó con el dataset Iris para comparar tres modelos de clasificación:

- LDA (Linear Discriminant Analysis)
- K-NN (K-Nearest Neighbors)
- Árbol de Decisión

El objetivo fue observar cómo cada algoritmo separa las clases en un espacio bidimensional usando dos características del dataset: longitud y ancho del sépalo.

Se evaluaron métricas como:

- exactitud (accuracy),
- matriz de confusión,
- reporte de clasificación por clase,
- fronteras de decisión visualizadas en 2D.

### 2. Comparación de modelos de regresión sobre California Housing

Se compararon varios modelos de regresión para predecir el valor medio de la vivienda:

- regresión lineal,
- regresión polinomial de grado 2,
- regresión polinomial de grado 3,
- regresión logarítmica.

Se utilizó validación cruzada con 5 folds para evaluar el rendimiento de cada modelo mediante:

- R²,
- MAE,
- RMSE.

Además, se analizaron gráficas de ajuste para ver cómo cada modelo representa la relación entre la variable predictora y la variable objetivo.

### 3. Clasificadores sobre varios datasets

Este ejercicio comparó varios clasificadores sobre cuatro bases de datos diferentes:

- MNIST Digits
- Fashion-MNIST
- Wine Dataset
- Breast Cancer Wisconsin

Los modelos evaluados fueron:

- Regresión Logística
- K-NN
- SVM
- Árbol de Decisión
- Random Forest

La actividad tuvo dos objetivos clave:

1. estudiar cómo cambia el rendimiento de los clasificadores según el tipo de dato,
2. analizar la relación entre complejidad del problema, representación del dato y elección del modelo.

---

## Qué se quiere aprender y qué se aprendió

### Lo que se quería aprender

- comprender la diferencia entre clasificación y regresión,
- entender cómo los modelos construyen decisiones a partir de datos,
- reconocer cuándo un modelo es más apropiado que otro,
- aprender a comparar resultados con métricas objetivas,
- interpretar visualmente la frontera de decisión o la tendencia de ajuste,
- aplicar validación para evitar conclusiones engañosas.

### Lo que se aprendió

- que un modelo “buen” depende del problema y del conjunto de datos,
- que no siempre el modelo más complejo es el mejor,
- que los datos pueden requerir escalamiento, transformación o selección de variables,
- que la validación cruzada ayuda a medir la generalización,
- que la visualización es una herramienta clave para comprender comportamientos del modelo,
- que la elección del algoritmo influye directamente en precisión, estabilidad y capacidad de interpretación.

---

## Casos de uso

### Clasificación

Los clasificadores son útiles cuando se desea asignar una etiqueta o categoría a una entrada.

Ejemplos:

- detección de spam en correos electrónicos,
- diagnóstico médico (enfermo / sano),
- reconocimiento de letras o dígitos escritos a mano,
- segmentación de clientes por comportamiento,
- identificación de especies según características físicas,
- análisis de sentimientos en textos.

### Regresión

Los modelos de regresión son útiles cuando se quiere predecir un valor numérico continuo.

Ejemplos:

- precio de una vivienda,
- demanda futura de un producto,
- temperatura esperada,
- riesgo financiero,
- ventas a lo largo del tiempo,
- consumo energético de un edificio.

---

## Casos de no uso

Aunque estos modelos son poderosos, no siempre son la mejor opción.

### Cuando no conviene usar clasificación o regresión simple

- cuando los datos tienen muchos valores faltantes y faltan estrategias de imputación,
- cuando la variable objetivo depende de relaciones temporales complejas, como series de tiempo con tendencia estacional fuerte,
- cuando hay datos no estructurados muy complejos, como texto o imágenes con representaciones crudas y muy altas dimensiones,
- cuando los datos están altamente desbalanceados y la métrica de precisión no es suficiente,
- cuando se requiere explicabilidad profunda en dominios regulados y se necesitan modelos probabilísticos más especializados,
- cuando la cantidad de datos es insuficiente para entrenar modelos robustos.

### Ejemplos concretos de no uso

- usar un árbol de decisión simple para tareas con cientos de miles de atributos sin preprocesamiento,
- usar regresión lineal para relaciones altamente no lineales sin transformar las variables,
- usar K-NN en datasets muy grandes y de alta dimensión, donde la distancia entre ejemplos puede volverse costosa.

---

## Ejemplos de aplicabilidad

### Ejemplo 1: diagnóstico médico

Un hospital podría usar un clasificador para decidir si un paciente tiene una enfermedad basada en signos vitales y resultados de laboratorio. Un modelo como SVM o Regresión Logística puede ayudar a apoyar la clasificación.

### Ejemplo 2: recomendación de precios

Una empresa inmobiliaria puede usar regresión para estimar el valor aproximado de una propiedad con base en ubicación, tamaño, antigüedad y otras variables.

### Ejemplo 3: reconocimiento de prendas

Fashion-MNIST es un caso de uso típico para clasificación de imágenes. Un sistema puede reconocer si una imagen corresponde a una camisa, un zapato, una mochila o otra categoría.

### Ejemplo 4: evaluación de riesgo financiero

Se pueden usar modelos de regresión o clasificación para estimar la probabilidad de incumplimiento crediticio o predecir la pérdida esperada de un préstamo.

---

## Analogías para entender mejor los modelos

### LDA: el detective que dibuja líneas rectas

LDA actúa como un detective que intenta separar grupos usando líneas claras y directas. Si las clases son fáciles de separar y siguen una estructura relativamente lineal, funciona muy bien.

### K-NN: el vecino que pide ayuda a los que viven cerca

K-NN toma una decisión mirando a los ejemplos más cercanos. Es como preguntar “¿a quién se parece más esta persona?” y decidir según sus vecinos más cercanos.

### Árbol de Decisión: el cuestionario paso a paso

Un árbol toma decisiones en forma de preguntas secuenciales: si la medida supera cierto valor, va por una rama; si no, por otra. Es fácil de interpretar, aunque puede sobreajustarse si se vuelve demasiado complejo.

### Regresión lineal: una línea que intenta resumir la tendencia

Es como intentar dibujar una línea sobre un conjunto de puntos para representar la tendencia general. Funciona bien cuando la relación es aproximadamente lineal.

### Regresión polinomial: una curva más flexible

Cuando los datos siguen una forma curvada, una línea recta no alcanza. La regresión polinomial intenta adaptar una curva más compleja para seguir la estructura de los datos.

### SVM: el modelo que busca la frontera más clara

SVM intenta encontrar la línea o frontera que mejor separa las clases, maximizando la distancia entre grupos. Es particularmente útil cuando la separación entre clases es difícil y deseamos una frontera robusta.

---

## Conclusión

La semana 7 permitió explorar de manera práctica la diferencia entre varios enfoques de aprendizaje supervisado. El trabajo no se centró solo en ejecutar algoritmos, sino en comprender cómo cada modelo toma decisiones, qué tipo de problemas resuelve y cuándo su uso es adecuado.

La gran lección fue que la inteligencia artificial no consiste en elegir un algoritmo al azar, sino en seleccionar el modelo correcto según la naturaleza de los datos y la tarea a resolver. Este principio es la base para construir sistemas inteligentes que sean útiles, robustos y explicables.

---

## Bibliografía y referencia conceptual

- scikit-learn documentation
- datasets clásicos de clasificación y regresión
- conceptos de aprendizaje supervisado y validación cruzada
- modelos de clasificación y regresión aplicados a datos tabulares e imágenes
