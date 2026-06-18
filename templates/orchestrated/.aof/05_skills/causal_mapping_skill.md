---
skill_name: causal_mapping_skill
version: "1.0.0"
sector: "e-commerce"
description: >
  Habilidad para identificar, modelar y actualizar el Grafo Causal
  Probabilístico (causal_graph.proto) del ecosistema. Permite al
  agente orquestador mantener y refinar su modelo del mundo.
triggers:
  - "Necesito actualizar el modelo causal"
  - "He detectado un nuevo bucle de retroalimentación"
  - "Actualiza las distribuciones de probabilidad tras la reflexión"
dependencies:
  proto_schema: "./04_knowledge/world_model/causal_graph.proto"
  delays: "./04_knowledge/world_model/delays.yaml"
  variables: "./04_knowledge/world_model/variables.yaml"
tools_required:
  - read_file
  - write_to_file
  - execute_command
---

# Causal Mapping Skill: Modelado Causal Probabilístico

## Propósito

Permitir al agente orquestador:
- Identificar variables relevantes y sus relaciones causales.
- Construir y mantener un Grafo Causal Probabilístico en formato protobuf.
- Actualizar distribuciones de probabilidad P(Y | do(X)) tras cada ciclo de reflexión.
- Detectar nuevos bucles de refuerzo o equilibrio y añadirlos al grafo.

## Flujo de actualización del grafo

### 1. Identificación de variables (fase Razonar)
- Analizar `state.md` y `variables.yaml`.
- Para cada variable nueva detectada, añadir un `CausalNode` al grafo.
- Clasificar el nodo como OBSERVABLE, CONTROLLABLE, LATENT u OBJECTIVE.
- Asignar una referencia al Gemelo Digital Unificado (Tejido T3).

### 2. Detección de relaciones causales (fase Reflexionar)
- Comparar la hipótesis activa con el resultado real.
- Si el resultado se desvía de la predicción:
  - ¿Faltaba una variable en el modelo? → Añadir nodo y arista.
  - ¿La relación no era lineal? → Ajustar el tipo de arista (REINFORCING_LOOP, BALANCING_LOOP).
  - ¿La magnitud fue distinta? → Actualizar la `ProbabilityDistribution` de la arista.
- Incrementar el campo `confidence` si la predicción fue precisa.

### 3. Actualización de distribuciones (fase Reflexionar)
- Para cada arista involucrada en la hipótesis validada/falsada:
  - Recolectar los datos observados en `observations/feedback_log.yaml`.
  - Recalcular los parámetros de la distribución (media, desviación estándar, etc.).
  - Actualizar `evidence_source` con el ID de la hipótesis.
- Si la confianza de una arista baja de 0.5, marcarla para revisión.

### 4. Detección de bucles
- Recorrer el grafo en busca de ciclos.
- Clasificar cada ciclo como REINFORCING_LOOP (amplifica cambios) o BALANCING_LOOP (estabiliza).
- Registrar los bucles detectados en el campo `type` de `CausalEdge`.
- Incorporar retardos asociados desde `delays.yaml`.

## Ejemplo de uso

```
Agente orquestador (N3, fase Reflexionar):
  Comparar H012 con resultado real:
    - Predicción: checkout_latency_p95 bajaría a 480ms tras añadir índice.
    - Realidad: bajó a 510ms (error del 6.25%).
  → Actualizar P(latencia | do(índice)) con distribución N(510, 25).
  → Aumentar confidence de 0.89 a 0.91.
  → Anotar evidence_source = "H012-2026-06-07 (validated, minor deviation)".