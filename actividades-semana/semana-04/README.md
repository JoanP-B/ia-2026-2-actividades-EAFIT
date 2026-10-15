# Semana 4 — Búsqueda local y algoritmos evolutivos

En esta semana se estudiaron diferentes técnicas de inteligencia artificial para buscar buenas soluciones dentro de espacios de búsqueda grandes. Se trabajaron métodos de búsqueda local y algoritmos evolutivos, observando cómo cada uno explora el espacio de posibles soluciones.

## Actividades

### 01 — Hill Climbing

**Archivo:** `01_hill_climbing.ipynb`

Hill Climbing es un algoritmo de búsqueda local que comienza con una solución inicial y, en cada paso, busca una solución vecina que mejore el resultado actual.

El objetivo es encontrar una buena solución sin tener que explorar todo el espacio de búsqueda. Es especialmente útil cuando existen demasiadas soluciones posibles para evaluarlas todas.

En la actividad se utilizó el problema de **ubicación de hospitales**, donde cada estado representa una posible ubicación de los hospitales y el costo representa la distancia total que deben recorrer las casas hasta el hospital más cercano.

También se experimentó con **Random Restart**, ejecutando Hill Climbing desde diferentes estados iniciales para reducir el riesgo de quedar atrapado en un óptimo local.

**Analogía:** es como estar caminando por una montaña con los ojos vendados y, en cada paso, elegir la dirección que parece llevarte más arriba. El problema es que puedes llegar a una pequeña cima y pensar que es la montaña más alta, aunque exista una montaña mucho más alta en otra parte.

---

### 02 — Simulated Annealing

**Archivo:** `02_simulated_annealing_hospitales.ipynb`

Simulated Annealing es un algoritmo de búsqueda local inspirado en el proceso de enfriamiento de los materiales. A diferencia de Hill Climbing, puede aceptar temporalmente soluciones peores para explorar otras regiones del espacio de búsqueda.

El objetivo es evitar quedar atrapado en óptimos locales y aumentar las posibilidades de encontrar soluciones de mejor calidad.

En la actividad se aplicó nuevamente al problema de **ubicación de hospitales** y se estudiaron parámetros como la temperatura inicial y la tasa de enfriamiento. También se comparó su comportamiento con Hill Climbing.

**Analogía:** es como buscar el mejor restaurante de una ciudad. Al principio estás dispuesto a probar lugares que parecen peores porque quieres explorar diferentes zonas. Conforme pasa el tiempo, te vuelves más exigente y te concentras en las mejores opciones que has encontrado.

---

### 03 — Algoritmos Genéticos

**Archivo:** `03_algoritmos_geneticos.ipynb`

Los algoritmos genéticos utilizan una población de posibles soluciones que evoluciona mediante procesos inspirados en la selección natural.

Cada individuo representa una solución y tiene un **fitness** que indica qué tan buena es. En cada generación se seleccionan individuos, se combinan mediante cruce y se introducen cambios mediante mutación para producir nuevas soluciones.

El objetivo es explorar un espacio de búsqueda mediante la evolución de múltiples soluciones en lugar de trabajar únicamente con una solución a la vez.

En la actividad se implementaron:

- Selección por ruleta.
- Cruce uniforme.
- Mutación.
- Evolución de una frase objetivo.
- Comparación con y sin elitismo.
- Análisis de convergencia prematura.

**Analogía:** es como una población de personas intentando construir una frase correcta. Las personas con las frases más parecidas al objetivo tienen mayor posibilidad de "reproducirse". Sus frases se combinan y algunas letras cambian aleatoriamente. Después de muchas generaciones, la población puede evolucionar hasta alcanzar la frase objetivo.

---

## Comparación general

Los tres enfoques buscan encontrar buenas soluciones, pero utilizan estrategias diferentes:


| Algoritmo | Idea principal | ¿Cuándo usarlo? | ¿Cuándo no usarlo? | Mayor problema | Analogía |
|---|---|---|---|---|---|
| **Hill Climbing** | Mejorar continuamente la solución actual escogiendo un vecino mejor | Cuando se necesita una solución rápidamente y el espacio de búsqueda es grande pero se puede definir fácilmente un vecindario | Cuando existen muchos óptimos locales o se necesita garantizar la solución óptima | Puede quedar atrapado en un **óptimo local** y depende mucho del estado inicial | Subir una montaña siempre tomando el camino que parece subir más |
| **Simulated Annealing** | Explorar soluciones y aceptar temporalmente soluciones peores para escapar de óptimos locales | Cuando Hill Climbing se queda atrapado en óptimos locales y se necesita explorar más el espacio de búsqueda | Cuando se necesita una respuesta rápida, determinista o una garantía de optimalidad | El resultado depende mucho de parámetros como la **temperatura y el enfriamiento** | Explorar una ciudad mientras al principio estás dispuesto a visitar lugares peores antes de decidir cuál es el mejor |
| **Algoritmos Genéticos** | Evolucionar una población de soluciones mediante selección, cruce y mutación | Cuando el espacio de búsqueda es enorme, complejo o difícil de explorar con métodos tradicionales y se pueden evaluar muchas soluciones | Cuando se necesita una solución exacta, determinista o el costo de evaluar cada individuo es muy alto | Puede sufrir **convergencia prematura** y requiere ajustar varios parámetros | Una población que evoluciona generación tras generación mediante selección y reproducción |

### ¿Por qué no usamos siempre el algoritmo "mejor"?

No existe un algoritmo universalmente mejor. Cada método hace diferentes compromisos entre **calidad de la solución, tiempo de ejecución, memoria, exploración y garantía de optimalidad**.

Por ejemplo:

- **Hill Climbing** puede ser muy rápido, pero puede quedarse atrapado en una mala solución.
- **Simulated Annealing** puede escapar de algunos óptimos locales, pero requiere ajustar cuidadosamente su temperatura y puede necesitar muchas iteraciones.
- **Algoritmos Genéticos** pueden explorar muchas regiones simultáneamente, pero requieren mantener una población y ajustar parámetros como la mutación, el cruce y la selección.

Por esta razón, la elección del algoritmo depende de las características del problema y de lo que se necesite priorizar: **rapidez, calidad, diversidad de soluciones o garantía de encontrar el óptimo**.

En conjunto, las actividades muestran diferentes formas de enfrentar problemas donde **probar todas las soluciones posibles resulta demasiado costoso**. Cada algoritmo utiliza una estrategia diferente para equilibrar la exploración del espacio de búsqueda y la búsqueda de soluciones de buena calidad.