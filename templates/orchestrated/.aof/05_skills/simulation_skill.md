---
skill_name: simulation_skill
version: "1.0.0"
sector: "e-commerce"
description: >
  Habilidad para simular intervenciones contrafactuales usando el Grafo Causal
  Probabilístico. Permite al agente orquestador predecir efectos de 1º, 2º y 3º
  orden con intervalos de confianza, y seleccionar la mejor acción antes de ejecutarla.
triggers:
  - "Simula los efectos de esta intervención"
  - "¿Qué pasaría si...?"
  - "Evalúa escenarios alternativos"
  - "Predice el impacto de un cambio en producción"
dependencies:
  causal_graph: "./04_knowledge/world_model/causal_graph.proto"
  delays: "./04_knowledge/world_model/delays.yaml"
  variables: "./04_knowledge/world_model/variables.yaml"
  state: "./04_knowledge/world_model/state.md"
tools_required:
  - read_file
  - write_to_file
  - execute_command
---

# Simulation Skill: Simulación Contrafactual Probabilística

## Propósito

Permitir al agente orquestador:
- Simular los efectos de una intervención antes de ejecutarla.
- Generar predicciones probabilísticas (con intervalos de confianza) en lugar de estimaciones puntuales.
- Evaluar múltiples escenarios y seleccionar el de mayor valor esperado.
- Anticipar efectos de 2º y 3º orden con sus probabilidades asociadas.

## Flujo de simulación

### 1. Carga del modelo (fase Razonar)
- Leer `causal_graph.proto` para obtener el grafo causal actual.
- Leer `delays.yaml` para obtener las distribuciones de retardo.
- Leer `state.md` y `variables.yaml` para conocer el estado actual del ecosistema.

### 2. Definición de la intervención (do(X))
- Especificar qué variable(s) se van a intervenir y su nuevo valor.
- Ejemplo: `do(db_query_latency_p95 = 20)` mediante la acción "añadir índice parcial".

### 3. Propagación probabilística
- Para cada nodo hijo de la variable intervenida:
  - Obtener `P(hijo | do(padre))` del grafo causal.
  - Muestrear de esa distribución para obtener un valor predicho.
  - Repetir N veces (N=1000 por defecto) para obtener una distribución de resultados.
- Aplicar retardos (con sus distribuciones) para efectos de 2º y 3º orden.
- Propagar recursivamente hasta cubrir todos los efectos en cascada.

### 4. Evaluación de escenarios
- Para cada escenario simulado, calcular:
  - Valor esperado del objetivo (ej: checkout_latency_p95).
  - Intervalo de confianza al 95%.
  - Probabilidad de violar restricciones (ej: conversión < línea base * 0.99).
- Seleccionar la intervención con mayor valor esperado que cumpla las restricciones.

### 5. Registro de la hipótesis (fase Hipótesis)
- Escribir la hipótesis en `hypotheses/active/` con:
  - Efectos de 1er orden: valor esperado + intervalo de confianza.
  - Efectos de 2º orden: probabilidad de ocurrencia + magnitud esperada.
  - Efectos de 3º orden: escenarios cualitativos con probabilidad estimada.

## Ejemplo de uso

```
Agente orquestador (N3, fase Hipótesis):
  Intervención propuesta: añadir índice parcial en tabla orders.
  Simulación:
    - Efecto 1º: P(checkout_latency_p95 | do(índice)) ~ N(480ms, 25ms) → IC 95%: [430ms, 530ms]
    - Efecto 2º: P(aumento_escrituras | do(índice)) ~ N(+5ms, 2ms) → probabilidad de >10ms: 2%
    - Efecto 3º: con 15% de probabilidad, el aumento de velocidad atrae más tráfico
                  (bucle de refuerzo), causando presión adicional en la DB en ~3h.
  Decisión: Proceder, con monitorización extra del bucle de refuerzo.
```