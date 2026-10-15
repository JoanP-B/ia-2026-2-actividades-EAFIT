# Semana 5 — Procesos de Decisión de Markov (MDP)

En esta semana se trabajó un problema de toma de decisiones mediante un **Proceso de Decisión de Markov (MDP)**, modelando el comportamiento de un robot dentro de un almacén.

---

## 📋 Actividad

### 03 — Lab Warehouse MDP

**Archivo:** `03_lab_warehouse_mdp_estudiantes.ipynb`

En esta actividad se modeló un robot que debe desplazarse por un almacén tomando decisiones sobre qué dirección seguir.

#### Entorno

El entorno contiene diferentes tipos de estados:

- 🟩 Una posición inicial `START`
- 🔌 Una estación de carga con recompensa `+2`
- 📦 Una estación de entrega con recompensa `+10`
- 🛢️ Un piso resbaloso que introduce incertidumbre en los movimientos
- ⚠️ Un costo por cada paso realizado

**Objetivo:** Encontrar una **política óptima**, es decir, una estrategia que indique qué acción debería tomar el robot en cada estado para maximizar la recompensa esperada a largo plazo.

---

## 🎯 ¿Qué problema busca solucionar?

El problema representa situaciones donde un agente debe tomar **decisiones secuenciales bajo incertidumbre**.

El robot no siempre llega exactamente al lugar que intenta alcanzar, especialmente cuando se encuentra sobre el piso resbaloso. Por lo tanto, no basta con preguntarse:

> "¿Cuál es el camino más corto?"

También es necesario considerar:

- Las recompensas futuras
- Los costos de movimiento
- La incertidumbre de las acciones
- La posibilidad de alcanzar diferentes objetivos
- La importancia de las recompensas inmediatas frente a las futuras

---

## 🔧 ¿Cómo representa un MDP el problema?

Un MDP representa el problema mediante:

| Componente | Descripción |
|---|---|
| **Estados** | Posiciones posibles del robot |
| **Acciones** | Movimientos que puede realizar |
| **Transiciones** | Probabilidades de terminar en diferentes estados después de una acción |
| **Recompensas** | Ganancias o costos asociados a las decisiones |
| **Política** | Acción que se recomienda tomar en cada estado |
| **Factor de descuento (γ)** | Determina cuánto importan las recompensas futuras |

---

## 📚 Conceptos principales

- Estados
- Acciones
- Probabilidades de transición T(s, a, s')
- Recompensas R(s, a, s')
- Política π(s)
- Valor de un estado V(s)
- Factor de descuento γ
- Incertidumbre en la toma de decisiones
- Recompensas inmediatas frente a recompensas futuras

---

## 💡 Analogía

Un MDP puede imaginarse como un robot repartidor dentro de un almacén.

El robot está en una posición y debe decidir hacia dónde moverse:

- Si el piso es normal, probablemente llegue al lugar que esperaba
- Si entra en una zona resbalosa, puede desviarse

Además, tiene dos opciones:
1. Ir rápidamente a una estación que le da una recompensa pequeña
2. Asumir un recorrido más largo y arriesgado para intentar alcanzar una recompensa mucho mayor

Por lo tanto, el robot no busca simplemente el camino más corto. **Busca la estrategia que produzca el mayor beneficio esperado** teniendo en cuenta el riesgo, los costos y las recompensas futuras.

---

## 🚀 Idea principal

A diferencia de los algoritmos de búsqueda y optimización trabajados anteriormente, un MDP **no se centra únicamente en encontrar un camino o una solución concreta**.

El objetivo es **aprender qué decisión tomar en cada estado**, considerando que:

- Las acciones pueden ser inciertas
- Las consecuencias pueden extenderse hacia el futuro

Esto permite modelar problemas reales como:

- 🤖 Navegación de robots
- 📊 Planificación
- 🎮 Control
- 🧠 Toma de decisiones

Donde el agente debe actuar **repetidamente** en un entorno que **no es completamente determinista**.