# Modelo ASA: Agentes Sistémicos Adaptativos — Documento Conceptual

**Propósito:** Este documento desarrolla los fundamentos teóricos y conceptuales del Modelo ASA (Agentes Sistémicos Adaptativos). Complementa la especificación ejecutable (`spec/asa.yaml`) con la narrativa que explica el "por qué" y el "cómo" de cada decisión de diseño. Está dirigido a investigadores, arquitectos de agentes y cualquier persona interesada en comprender los pilares teóricos sobre los que se construye AOF‑S.

**Versión:** 1.1
**Fecha:** 13 de junio de 2026
**Parte del estándar:** AOF‑S v1.0

---

## Tabla de Contenidos

1. [Fundamentos Teóricos](#1-fundamentos-teóricos)
2. [Principio de Intervención Reflexiva Causal](#2-principio-de-intervención-reflexiva-causal)
3. [Los Tres Niveles de Agencia Progresiva](#3-los-tres-niveles-de-agencia-progresiva)
4. [Cuándo Usar Cada Nivel: Complejidad Justificada](#4-cuándo-usar-cada-nivel-complejidad-justificada)
5. [El Ciclo ASA de 7 Fases](#5-el-ciclo-asa-de-7-fases)
6. [ASA en el Contexto de otros Paradigmas](#6-asa-en-el-contexto-de-otros-paradigmas)
7. [Memoria Cuádruple](#7-memoria-cuádruple)
8. [Meta Cognición y Razonamiento Neuro Simbólico Híbrido](#8-meta-cognición-y-razonamiento-neuro-simbólico-híbrido)
9. [Grafo Causal Probabilístico](#9-grafo-causal-probabilístico)
10. [Estándares y Herramientas Abiertas](#10-estándares-y-herramientas-abiertas)
11. [Modos de Operación del N3: Degradación Graciosa y Delegación Inteligente](#11-modos-de-operación-del-n3-degradación-graciosa-y-delegación-inteligente)
12. [Gobernanza Algorítmica y Seguridad](#12-gobernanza-algorítmica-y-seguridad)
13. [Principio Local‑First](#13-principio-local-first)
14. [Relación con AOF‑S y el Noosistema](#14-relación-con-aof-s-y-el-noosistema)
15. [Limitaciones y Hoja de Ruta](#15-limitaciones-y-hoja-de-ruta)

---

## 1. Fundamentos Teóricos

El Modelo ASA no surge en el vacío. Se fundamenta en tres pilares teóricos consolidados y los extiende al dominio de los agentes de IA autónomos:

### 1.1 Cibernética de Segundo Orden

La cibernética de segundo orden, formulada por Heinz von Foerster [1], reconoce que el observador forma parte del sistema que observa. A diferencia de la cibernética clásica (primer orden), que trata al sistema como un objeto externo que puede ser controlado, la cibernética de segundo orden incorpora al agente como un componente más del bucle de retroalimentación.

**Implicación para ASA:** Un agente no puede limitarse a "ejecutar acciones" sobre un sistema externo. Debe reconocer que sus propias intervenciones modifican el sistema y que, a su vez, el sistema modificado influye en sus futuras percepciones y decisiones. El ciclo ASA materializa este principio: cada intervención genera una observación que realimenta el modelo del mundo del agente.

### 1.2 Dinámica de Sistemas

La dinámica de sistemas, desarrollada por Jay Forrester [2] y popularizada por Donella Meadows [3], modela el comportamiento de sistemas complejos mediante bucles de realimentación (refuerzo y equilibrio), retardos temporales y relaciones no lineales.

**Implicación para ASA:** Los agentes deben modelar explícitamente los bucles de refuerzo (que amplifican los cambios) y los bucles de equilibrio (que estabilizan el sistema). El Grafo Causal Probabilístico (`causal_graph.proto`) es la materialización de este principio: captura las relaciones causales, los retardos asociados y la incertidumbre inherente a cada arista causal.

### 1.3 Arquitecturas Cognitivas

La arquitectura cognitiva ACT‑R (Adaptive Control of Thought–Rational), desarrollada por John Anderson [4], distingue entre memoria declarativa (hechos y reglas) y memoria procedural (habilidades y políticas de acción). Esta distinción ha demostrado ser efectiva para modelar el aprendizaje humano.

**Implicación para ASA:** ASA extiende esta distinción a cuatro tipos de memoria (episódica, semántica, procedural y meta‑cognitiva), permitiendo que el agente no solo recuerde hechos, sino que refine sus propias estrategias de razonamiento a lo largo del tiempo.

### 1.4 Causalidad Probabilística

El marco de inferencia causal desarrollado por Judea Pearl [5] introduce el operador `do(X)` para representar intervenciones y el cálculo de probabilidades condicionales `P(Y | do(X))` para predecir el efecto de una intervención sobre una variable de interés.

**Implicación para ASA:** El Grafo Causal Probabilístico modela exactamente esta semántica: cada arista causal almacena una distribución de probabilidad condicional que el agente actualiza mediante inferencia bayesiana durante la fase de reflexión. Esto permite cuantificar la incertidumbre y mejorar las predicciones con la experiencia.

---

## 2. Principio de Intervención Reflexiva Causal

El principio rector del Modelo ASA es la **Intervención Reflexiva Causal**, que supera el paradigma newtoniano de Acción‑Reacción y establece un nuevo estándar para la agencia autónoma:

> *"Toda intervención se formula como hipótesis de sus efectos, se ejecuta bajo políticas de seguridad, se evalúa y se contrasta con la realidad, actualizando el modelo del mundo y, en niveles superiores, las propias estrategias de razonamiento."*

### 2.1 Desglose del Principio

| Componente | Significado | Materialización en ASA |
|------------|-------------|------------------------|
| **Hipótesis** | Antes de actuar, el agente predice los efectos de 1º, 2º y 3º orden de su intervención. | Fase 3: Hipótesis. Archivo `hypothesis.md`. |
| **Ejecución controlada** | La acción se ejecuta bajo políticas de seguridad verificables. | Fase 4: Verificar (N3). Políticas OPA en `Noosfile.yaml`. |
| **Evaluación** | El resultado real se compara con la predicción. | Fase 6: Evaluar. `observations/feedback_log.yaml`. |
| **Reflexión** | Las discrepancias actualizan el modelo del mundo y, en niveles superiores, las estrategias de razonamiento. | Fase 7: Reflexionar. `causal_graph.proto`, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml`. |

### 2.2 Diferencia con el Paradigma Reactivo

| Paradigma Reactivo (ReAct, Plan‑and‑Solve) | Paradigma ASA |
|---------------------------------------------|---------------|
| El agente actúa y luego observa. | El agente formula una hipótesis causal antes de actuar. |
| Sin modelo del mundo explícito. | Grafo Causal Probabilístico actualizado continuamente. |
| Sin distinción de niveles de agencia. | Tres niveles progresivos (Reactivo, Sistémico, Orquestado). |
| La memoria es un log plano o inexistente. | Memoria cuádruple con mecanismos de olvido y fortalecimiento. |

## 3. Los Tres Niveles de Agencia Progresiva

Uno de los principios fundacionales del Modelo ASA es la **agencia progresiva**: la complejidad se activa solo en el nivel que la requiere. Un agente no nace complejo; evoluciona desde una configuración mínima (2 archivos, 5 minutos) hasta un orquestador de ecosistemas completo.

Esta progresividad responde a tres necesidades prácticas:

1. **Barrera de entrada mínima:** Un desarrollador que solo necesita un agente reactivo no debería verse obligado a configurar memoria, grafos causales o políticas de seguridad.
2. **Eficiencia de recursos:** No todas las tareas requieren simulación contrafactual o meta‑cognición. Un agente de Nivel 1 consume ~2‑4k tokens; uno de Nivel 3, ~7‑12k.
3. **Especialización del ecosistema:** En un Noosistema conviven agentes de distintos niveles. Los N1 ejecutan tareas de alta frecuencia; los N2 se especializan en dominios; los N3 orquestan y gobiernan.

### 3.1 Visión General

| Nivel | Nombre | Memoria | Modelo del Mundo | Fases del Ciclo | Cierre de Ciclo | Contexto | Despliegue |
|-------|--------|---------|------------------|-----------------|-----------------|----------|------------|
| **1** | **Reactivo** | Sin memoria persistente | No | Arranque → Razonar → Ejecutar → Evaluar | Sistémico (vía memoria colectiva) | ~2‑4k tokens | CPU, Raspberry Pi, llama.cpp |
| **2** | **Sistémico** | Tricameral (Episódica + Semántica + Procedural) | Variables, estado, Grafo Causal (1º, 2º orden) | Arranque → Razonar → Hipótesis → Ejecutar → Evaluar → Reflexionar | Local | ~4‑7k tokens | GPU 8‑12GB VRAM, Ollama |
| **3** | **Orquestado** | Cuádruple (+ Meta‑cognitiva) | Grafo Causal Probabilístico (1º, 2º, 3º orden) | Arranque → Razonar → Hipótesis → Verificar → Ejecutar → Evaluar → Reflexionar | Local + Sistémico | ~7‑12k tokens | GPU 18GB+, Clúster, vLLM |

### 3.2 Nivel 1 — Reactivo

El nivel más simple. Un agente Reactivo ejecuta tareas atómicas de alta frecuencia sin memoria persistente. No formula hipótesis ni reflexiona sobre sus acciones. Sin embargo, **no es un agente aislado**: sus evaluaciones se persisten en una **memoria colectiva del Noosistema**, accesible para agentes de niveles superiores.

**Características:**
- **Sin memoria persistente local:** Cada invocación es independiente. El agente no recuerda lo que hizo ayer.
- **Ciclo de 4 fases:** Arranque → Razonar → Ejecutar → Evaluar. Sin hipótesis, sin verificación, sin reflexión propia.
- **Cierre de ciclo sistémico:** Aunque el agente no aprende localmente, sus evaluaciones alimentan la memoria colectiva del ecosistema. Un agente N2 o N3 puede consumir esas evaluaciones durante su fase de reflexión para mejorar sus propios modelos.
- **Modo ultra‑light:** 2 archivos, 5 minutos para arrancar. El resto de artefactos se generan bajo demanda al subir de nivel.

**Ejemplos de uso:**
- Agregación de logs (log‑aggregator).
- Precalentamiento de cachés (cache‑warmer).
- Health checks periódicos.
- Detección de anomalías simples por umbral.

### 3.3 Nivel 2 — Sistémico

El nivel donde el agente **empieza a aprender**. Incorpora memoria tricameral, un modelo del mundo básico y un ciclo de aprendizaje local.

**Características:**
- **Memoria tricameral:** Episódica (registro cronológico de intervenciones), semántica (reglas y hechos extraídos) y procedural (políticas de acción automatizadas).
- **Modelo del mundo:** Variables observables y un estado inferido del sistema. Grafo Causal Probabilístico para efectos de 1º y 2º orden.
- **Ciclo de 6 fases:** Añade Hipótesis y Reflexionar al ciclo del N1. El agente formula predicciones antes de actuar y compara resultados después.
- **Cierre de ciclo local:** El agente actualiza su propio estado (memoria, modelo causal) tras cada reflexión. No puede modificar reglas globales del ecosistema.

**Ejemplos de uso:**
- Optimizador de bases de datos (db‑provisioner).
- Monitorización de signos vitales con alertas (clinical‑agent).
- Optimización energética predictiva (energy‑agent).
- Escaneo de vulnerabilidades (security‑scanner).

### 3.4 Nivel 3 — Orquestado

El nivel más avanzado. Incorpora memoria cuádruple con meta‑cognición, razonamiento neuro‑simbólico híbrido y capacidad de orquestar a otros agentes.

**Características:**
- **Memoria cuádruple:** Añade memoria meta‑cognitiva a las tres del N2. Esta memoria almacena el rendimiento histórico de las estrategias de razonamiento (CoT, ToT, simulación causal) con métricas de éxito, latencia y dominios de aplicación.
- **Fase Verificar:** Exclusiva de N3. Un motor de reglas lógicas (CLIPS/Drools) valida la hipótesis generada por el LLM contra contratos semánticos y políticas de seguridad. Es la materialización del razonamiento neuro‑simbólico híbrido.
- **Cierre de ciclo local + sistémico:** Además de actualizar su propio estado, puede reescribir reglas globales del ecosistema (políticas OPA, umbrales de confianza en el `registry.yaml`).
- **Orquestación multi‑agente:** Coordina a agentes N2 y N1 mediante los protocolos A2A, MCP y NSL. Mantiene el `registry.yaml` con los trust scores de todos los agentes.
- **Simulación contrafactual completa:** Modela efectos de 1º, 2º y 3º orden usando Monte Carlo Tree Search (MCTS) sobre el Grafo Causal Probabilístico.

**Ejemplos de uso:**
- Orquestador de e‑commerce (ecommerce‑orchestrator).
- Orquestador hospitalario (hospital‑orchestrator).
- Coordinador de centro de datos (dc‑orchestrator).

### 3.5 Progresión entre Niveles

Un agente no nace en Nivel 3. La progresión sigue un flujo declarativo gestionado por `noctl`:

1. **Nivel 1 → Nivel 2:** El agente acumula suficientes episodios y detecta patrones recurrentes. El usuario u orquestador autoriza la migración con `noctl upgrade --level 2`. Se añaden los directorios `world_model/`, `memory/` y el archivo `hypothesis.md`.
2. **Nivel 2 → Nivel 3:** El agente demuestra capacidad de modelado causal (hipótesis validadas > umbral) y recibe autorización para orquestar. Se añaden `ASA.yaml`, `expert_corpus/`, `registry.yaml`, `causal_graph.proto` y skills avanzadas.

## 4. Cuándo Usar Cada Nivel: Complejidad Justificada

ASA no obliga a usar el N3 en todos los casos. El ciclo de 7 fases está diseñado para dominios donde la causalidad, la verificación simbólica y el aprendizaje acumulativo son necesarios. Para el resto, existen N1 y N2.

### 4.1 Escenarios recomendados por nivel

| Escenario | Nivel ASA recomendado | Razón |
|:---|:---|:---|
| Chatbot simple con herramientas | N1 (Reactivo) | 4 fases, sin sobrecarga. |
| Tarea de una sola llamada a API | N1 (Reactivo) | Sin hipótesis ni reflexión. |
| Hardware edge (RPi, móvil) | N1 (Reactivo) o N2 ligero | Sin motor simbólico ni meta‑cognición. |
| Dominio sin causalidad clara | N2 (Sistémico) | Grafo causal opcional; el agente puede operar solo con memoria. |
| Sistema con efectos de orden superior | **N3 (Orquestado)** | Aquí ASA justifica su complejidad. |
| Verificación lógica obligatoria (salud, finanzas) | **N3 (Orquestado)** | La capa simbólica es necesaria. |

### 4.2 Ejemplo de despliegue en fase Enjambre

La tabla siguiente ilustra cómo los tres niveles de agencia coexisten en un Noosistema maduro (fase Enjambre), distribuyéndose en las capas de despliegue según su función:

| Cantidad | Nivel | Rol | Capa de despliegue | Notas |
|:---|:---|:---|:---|:---|
| 1 | N3 | Director de orquesta (completo) | C5 (Cloud) | Ciclo de 7 fases. Gobernanza global. |
| 1 | N3 | Orquestador Edge (degradado) | C3 (Edge) | Ciclo de 5 fases. Gobernanza local. Escala a C5 cuando es necesario. |
| 10 | N2 | Especialistas (DB, energía, seguridad, clínica...) | C4 (Cloud) o C3 (Edge) | Sin capacidad de orquestación. |
| 30 | N1 | Ejecutores | C3 (Edge) | Tareas de frecuencia media. |
| 5 | N1 | Sensores y actuadores | C1 (Campo) | Tareas de alta frecuencia. Ultra‑light. |

---

## 5. El Ciclo ASA de 7 Fases

El ciclo ASA es el corazón operativo del modelo. Define cómo un agente procesa una necesidad, formula una intervención y aprende de sus consecuencias.

### 5.1 Visión General

| # | Fase | N1 | N2 | N3 | Descripción |
|---|------|----|----|----|-------------|
| 1 | **Arranque** | ✅ | ✅ | ✅ | Carga de artefactos AOF‑S, misión y estado del modelo del mundo. |
| 2 | **Razonar** | ✅ | ✅ | ✅ | Contextualización + inferencia según nivel. |
| 3 | **Hipótesis** | — | ✅ | ✅ | Formulación de predicciones causales con efectos de orden superior. |
| 4 | **Verificar** | — | — | ✅ | Validación neuro‑simbólica de la hipótesis (motor de reglas lógicas). |
| 5 | **Ejecutar** | ✅ | ✅ | ✅ | Implementación monitorizada de los pasos con watchdog de bucles. |
| 6 | **Evaluar** | ✅ | ✅ | ✅ | Registro de métricas, detección de anomalías y comparación con la hipótesis. |
| 7 | **Reflexionar** | — | ✅ | ✅ | Actualización del modelo del mundo, memorias y (en N3) estrategias de razonamiento. |

### 5.2 Fase 1: Arranque

El agente carga los artefactos mínimos necesarios para comprender quién es, qué debe hacer y en qué contexto opera.

**Artefactos cargados (según nivel):**
- **N1:** `00_manifest.yaml`, `01_context_static.md`, `02_mission.yaml`.
- **N2:** Los anteriores + `05_world_model/state.md`, `07_memory/procedural/policies.yaml`.
- **N3:** Los anteriores + `ASA.yaml`, `09_values/constitution.md`, `04_knowledge/registry.yaml`.

**Equivalencia con el ciclo cerrado del Holosistema TIC:** Esta fase corresponde a la **Percepción** inicial del sistema: el agente se sincroniza con el gemelo digital y el bus de eventos para obtener el estado actual.

### 5.3 Fase 2: Razonar

El agente contextualiza la tarea y genera inferencias iniciales. La profundidad de esta fase varía según el nivel:

- **N1 (Básica):** El agente analiza la tarea a la luz del contexto estático y la misión. Sin recuperación de memoria.
- **N2 (Estándar):** El agente recupera de la memoria episódica los episodios similares (top‑N) y de la memoria procedural las políticas activas. Consulta el estado actual del sistema (`state.md`, `variables.yaml`).
- **N3 (Avanzada):** Además de lo anterior, el agente consulta su memoria meta‑cognitiva para seleccionar la estrategia de razonamiento más adecuada para el dominio de la tarea (CoT, ToT, simulación causal, etc.). También puede ejecutar búsquedas RAG sobre el `expert_corpus/` para enriquecer el contexto.

**Fundamento teórico:** Esta fase materializa la distinción de Kahneman entre Sistema 1 (respuestas rápidas, procedurales) y Sistema 2 (razonamiento deliberado, meta‑cognitivo). En N1 predomina el Sistema 1; en N3 se activa el Sistema 2 con selección estratégica.

### 5.4 Fase 3: Hipótesis

El agente formula una hipótesis de intervención con predicciones cuantitativas de sus efectos. Esta fase es el núcleo del principio de Intervención Reflexiva Causal.

**Profundidad por nivel:**
- **N2:** Formula una hipótesis con efectos de 1º y 2º orden, utilizando el Grafo Causal Probabilístico básico y la memoria semántica (`learned_rules.yaml`). La predicción es puntual con un margen de confianza.
- **N3:** Formula una hipótesis con efectos de 1º, 2º y 3º orden. Ejecuta simulación contrafactual mediante Monte Carlo Tree Search (MCTS) sobre el Grafo Causal Probabilístico, generando intervalos de confianza al 95% para cada efecto predicho. La hipótesis se registra en `hypotheses/active/`.

**Ejemplo (N3, e‑commerce):**
```markdown
## Hipótesis H012-2026-06-07
- Acción: añadir índice parcial en orders(status).
- Efecto 1º: P(latencia | do(índice)) ~ N(480ms, 25ms) → IC 95%: [430ms, 530ms].
- Efecto 2º: P(aumento_escrituras | do(índice)) ~ N(+5ms, 2ms) → probabilidad de >10ms: 2%.
- Efecto 3º: con 15% de probabilidad, el aumento de velocidad atrae más tráfico
  (bucle de refuerzo), causando presión adicional en DB en ~3h.
```

### 5.5 Fase 4: Verificar (Exclusiva de N3)

Esta fase es la materialización del **razonamiento neuro‑simbólico híbrido**. Un motor de reglas lógicas (CLIPS, Drools) valida la hipótesis generada por el LLM contra:

- **Contratos semánticos:** JSON Schema declarados en `ASA.yaml` para las capacidades del agente.
- **Políticas de seguridad:** OPA declaradas en `Noosfile.yaml`.
- **Restricciones constitucionales:** `constitution.md`.

Si la validación falla, la hipótesis se rechaza antes de la ejecución y se registra en el ledger de auditoría. Si se aprueba, se procede a la fase de ejecución.

**Fundamento teórico:** Los LLM son potentes pero no deterministas. El motor de reglas lógicas actúa como un guardarraíl que asegura que las hipótesis cumplan las políticas de seguridad y los contratos semánticos antes de ejecutarse. Esta combinación de razonamiento neuronal (LLM) y simbólico (motor de reglas) es lo que denominamos razonamiento neuro‑simbólico híbrido.

### 5.6 Fase 5: Ejecutar

El agente ejecuta los pasos derivados de la hipótesis. La ejecución es monitorizada:

- **Watchdog de bucles:** Si se detecta un bucle de realimentación no previsto (ej: un aumento de tráfico que dispara la latencia), el watchdog puede abortar la ejecución y notificar al orquestador.
- **Registro de auditoría:** Cada acción ejecutada se registra en el ledger con su hipótesis asociada y timestamp.

**Profundidad por nivel:**
- **N1:** Ejecuta tareas atómicas (health check, ingesta de logs) sin monitorización avanzada.
- **N2/N3:** Ejecutan pasos complejos con monitorización de métricas y watchdog.

### 5.7 Fase 6: Evaluar

El agente recolecta métricas, detecta anomalías y compara el resultado real con la hipótesis formulada en la fase 3.

**Artefactos generados:**
- `observations/feedback_log.yaml`: Métricas cuantitativas con su hipótesis asociada.
- `observations/anomaly_notes.md`: Desviaciones no predichas.

**Ejemplo (N2, hospital):**
```yaml
- timestamp: "2026-06-07 14:30:00Z"
  metric: spo2_trend_uci_3
  value: 87
  unit: percent
  hypothesis: "H017-2026-06-07"
  result: "validated"
  deviation: "-1% (predicho 88%)"
```

### 5.8 Fase 7: Reflexionar

El agente cierra el ciclo de aprendizaje. Compara los resultados reales con la hipótesis y actualiza su conocimiento:

**N2 (Cierre local):**
- Actualiza el Grafo Causal Probabilístico (distribuciones de probabilidad).
- Extrae o refina reglas semánticas en `learned_rules.yaml`.
- Refina políticas procedurales en `policies.yaml`.
- Archiva la hipótesis en `hypotheses/validated/` o `hypotheses/falsified/`.
- Registra el episodio en `memory/episodic/`.

**N3 (Cierre local + sistémico):**
- Todo lo anterior.
- Actualiza la memoria meta‑cognitiva con el rendimiento de la estrategia de razonamiento utilizada (`meta_strategies.yaml`).
- Puede reescribir reglas globales del ecosistema (políticas OPA, umbrales de confianza en `registry.yaml`).
- Puede disparar la creación autónoma de una nueva skill si la tarea fue novedosa y exitosa.

**N1 (Cierre sistémico):**
- El agente N1 no reflexiona localmente, pero sus evaluaciones se persisten en la **memoria colectiva del Noosistema**. Los agentes N2 y N3 pueden consumir esas evaluaciones durante sus propias fases de reflexión para mejorar sus modelos.

## 6. ASA en el Contexto de otros Paradigmas

Para comprender el posicionamiento de ASA, se presenta una comparación con los paradigmas de razonamiento más extendidos en 2026.

### 6.1 Tabla Comparativa de Bucles de Razonamiento

| Fase ASA | N1 | N2 | N3 | 🔄 ReAct | 🔁 Reflexion | 📋 Plan‑and‑Execute | 🌳 ToT / GoT |
|:---|:---:|:---:|:---:|:---|:---|:---|:---|
| **1. Arranque** | ✅ | ✅ | ✅ | — (No tiene fase de inicialización explícita; comienza directamente con Pensar). | — (Comienza directamente con Actuar). | — (Comienza directamente con Planificar). | — (Comienza directamente con la descomposición del problema). |
| **2. Razonar** | ✅ | ✅ | ✅ | **Pensar (Thought)**: El LLM analiza la situación y decide el siguiente paso. | — (No tiene fase de razonamiento explícita; va directo a la acción). | **Planificar (Plan)**: Un agente descompone el objetivo en una lista de pasos. | **Descomposición / Generación de Pensamientos**: El problema se divide en múltiples ideas o caminos potenciales. |
| **3. Hipótesis** | — | ✅ | ✅ | — | — | — | — |
| **4. Verificar** | — | — | ✅ | — | — | — | — |
| **5. Ejecutar** | ✅ | ✅ | ✅ | **Actuar (Act)**: Ejecuta la acción planificada (API, búsqueda, herramienta). | **Actuar (Act)**: Ejecuta una acción para resolver la tarea. | **Ejecutar (Execute)**: Un ejecutor sigue los pasos del plan predefinido. | — (La ejecución no es el foco; se centra en explorar caminos de razonamiento). |
| **6. Evaluar** | ✅ | ✅ | ✅ | **Observar (Observe)**: Incorpora el resultado de la acción al contexto para la siguiente iteración. | **Evaluar (Evaluate)**: Un componente crítico evalúa la calidad de la respuesta generada. | — (La evaluación es implícita; el planificador puede revisar los pasos restantes según los resultados). | **Evaluación de Pensamientos**: Se puntúa o selecciona qué caminos de razonamiento son más prometedores. |
| **7. Reflexionar** | — | ✅ | ✅ | — (No tiene fase de reflexión; simplemente itera). | **Reflexionar (Reflect)**: Genera una reflexión sobre qué salió mal y qué se puede aprender del error. | — (No tiene fase de reflexión; el bucle termina al ejecutar todos los pasos). | **Selección / Estrategia**: Se decide qué camino seguir o se fusionan y retroalimentan ideas entre diferentes partes de la estructura. |
| **Cierre de ciclo** | Sistémico (memoria colectiva) | Local | Local + Sistémico | **Iteración**: Repite Pensar → Actuar → Observar hasta alcanzar el objetivo. | **Memoria**: Las reflexiones se almacenan como memoria episódica para mejorar en futuros intentos. | **Finalización**: El bucle termina una vez que todos los pasos del plan han sido ejecutados. | **Iteración**: El proceso de generación y evaluación se repite hasta converger en una solución. |

### 6.2 Conclusión Comparativa

ASA no compite con estos paradigmas en todos los frentes. En tareas simples, ReAct o Plan‑and‑Execute son más eficientes. ASA justifica su complejidad en dominios con causalidad no trivial, necesidad de verificación formal y aprendizaje acumulativo. La tabla "Cuándo Usar Cada Nivel" guía al adoptante hacia la configuración adecuada.

## 7. Memoria Cuádruple

La memoria es uno de los pilares diferenciales del Modelo ASA. Mientras que la mayoría de los frameworks de agentes tratan la memoria como un simple almacén de logs o un vector store indiferenciado, ASA distingue cuatro tipos de memoria con funciones cognitivas específicas, inspiradas en la arquitectura ACT‑R [4] y extendidas con mecanismos de olvido, fortalecimiento y meta‑cognición.

### 7.1 Fundamento Teórico: Más Allá de ACT‑R

La arquitectura cognitiva ACT‑R distingue entre:

- **Memoria declarativa:** Hechos y reglas que pueden ser verbalizados (equivalente a nuestra memoria semántica).
- **Memoria procedural:** Habilidades y políticas de acción que se ejecutan automáticamente (equivalente a nuestra memoria procedural).

ASA extiende este modelo con dos aportaciones originales:

1. **Memoria episódica explícita:** Mientras que ACT‑R modela la memoria episódica como parte de la declarativa, ASA la separa para darle un rol específico: almacenar la secuencia temporal de intervenciones con su contexto completo, permitiendo la recuperación por similitud de situaciones pasadas.

2. **Memoria meta‑cognitiva:** Esta es una aportación enteramente original de ASA. No existe en ACT‑R ni en la mayoría de arquitecturas cognitivas artificiales. Permite que el agente no solo aprenda sobre el mundo, sino que aprenda sobre sus propias estrategias de razonamiento: cuál funciona mejor en cada dominio, con qué latencia y con qué tasa de éxito.

### 7.2 Los Cuatro Tipos de Memoria

#### 7.2.1 Memoria Episódica

Almacena reglas generales, hechos del dominio y patrones causales extraídos de la experiencia. Cada regla incluye parámetros de ciclo de vida (`confidence`, `last_accessed`, `access_count`, `decay_rate`) que permiten el olvido por desuso y el fortalecimiento por uso.

**Función:** Almacenar la secuencia temporal de intervenciones del agente, con su contexto completo, la hipótesis formulada, la acción tomada y el resultado observado.

**Inspiración cognitiva:** La memoria episódica humana, que nos permite recordar eventos específicos con su contexto temporal y emocional.

**Disponible en:** N2 y N3.

**Formato:** Archivos Markdown organizados cronológicamente (`YYYY-MM-DD_NN_descripcion.md`).

**Recuperación:** El agente recupera los episodios más similares (top‑N) durante la fase de Razonar, utilizando búsqueda semántica sobre un backend vectorial (LanceDB) si está disponible, o mediante búsqueda por palabras clave sobre los archivos.

**Ejemplo de entrada:**
```markdown
# Episodio: Pico de latencia en /search
## Timestamp
2026-06-07 09:00 UTC

## Observación
El endpoint `/search` alcanzó una latencia de 2s.

## Hipótesis formulada
H000-2026-06-07: falta índice en products(name).

## Acción tomada
Se añadió índice idx_products_name.

## Resultado
Latencia volvió a 50ms en p95.

## Lección aprendida
Verificar índices antes de añadir funcionalidades de búsqueda.
```

#### 7.2.2 Memoria Semántica
Almacena reglas generales, hechos del dominio y patrones causales extraídos de la experiencia. Cada regla incluye parámetros de ciclo de vida (`confidence`, `last_accessed`, `access_count`, `decay_rate`) que permiten el olvido por desuso y el fortalecimiento por uso.

**Función:** Almacenar reglas generales, hechos del dominio y patrones causales extraídos de la experiencia. A diferencia de la memoria episódica (que almacena eventos concretos), la semántica almacena conocimiento abstracto y generalizable.

**Inspiración cognitiva:** La memoria semántica humana, que almacena conocimiento sobre el mundo independientemente del contexto en que fue aprendido.

**Disponible en:** N2 y N3.

**Formato:** Archivos YAML estructurados (`learned_rules.yaml`, `domain_facts.md`).

**Mecanismos de olvido y fortalecimiento:** Cada regla incluye parámetros de ciclo de vida que permiten la aplicación de dinámicas cognitivas:

| Campo | Función |
|-------|---------|
| `confidence` | Grado de certeza del agente en la regla (0.0 a 1.0). Se incrementa cuando la regla predice correctamente; se reduce cuando falla. |
| `last_accessed` | Timestamp del último acceso. Las reglas no utilizadas durante largos períodos decaen. |
| `access_count` | Número de veces que la regla ha sido consultada. Las reglas muy utilizadas se fortalecen. |
| `decay_rate` | Tasa de decaimiento exponencial. Las reglas con `confidence` por debajo de un umbral pueden ser eliminadas. |

**Ejemplo de entrada:**
```yaml
- rule: "Mejoras de rendimiento en checkout aumentan tráfico ~15% en la primera hora."
  confidence: 0.88
  last_accessed: "2026-06-07T09:30:00Z"
  access_count: 143
  decay_rate: 0.05
  source: "H008-2026-05-04 (validated)"
  tags: ["checkout", "performance", "traffic", "reinforcing_loop"]
```

#### 7.2.3 Memoria Procedural
Almacena políticas de acción que automatizan respuestas rápidas ante situaciones recurrentes. 

**Función:** Almacenar políticas de acción que automatizan respuestas rápidas ante situaciones recurrentes. Es el equivalente al Sistema 1 de Kahneman: respuestas automáticas, de baja latencia, que no requieren razonamiento deliberado.

**Inspiración cognitiva:** La memoria procedural humana, que nos permite ejecutar habilidades (conducir, escribir, etc.) sin esfuerzo consciente.

**Disponible en:** N2 y N3.

**Formato:** Archivo YAML (`policies.yaml`) con reglas de producción "si condición entonces acción".

**Ejemplo de entrada:**
```yaml
policies:
  - name: "check_index_on_latency"
    condition: "db_query_latency_p95 > 500ms"
    action: "revisar si falta índice en la tabla principal consultada"
    confidence: 0.95
    source: "H000-2026-06-07 (validated)"
```

#### 7.2.4 Memoria Meta‑Cognitiva (Exclusiva de N3)
Almacena el rendimiento histórico de las estrategias de razonamiento utilizadas, permitiendo la selección dinámica de la más adecuada para cada tarea.

**Función:** Almacenar el rendimiento histórico de las estrategias de razonamiento utilizadas por el agente. Permite la selección dinámica de la estrategia más adecuada para cada tarea.

**Inspiración cognitiva:** La meta‑cognición humana: la capacidad de reflexionar sobre nuestros propios procesos de pensamiento y seleccionar la estrategia más adecuada para cada problema.

**Disponible en:** Solo N3.

**Formato:** Archivo YAML (`meta_strategies.yaml`).

**Campos:**
| Campo | Función |
|-------|---------|
| `name` | Nombre de la estrategia (chain_of_thought, causal_simulation, tree_of_thoughts, etc.). |
| `success_rate` | Tasa de éxito histórica (hipótesis validadas / total). |
| `avg_latency_ms` | Latencia promedio de la estrategia. |
| `domains` | Dominios donde la estrategia ha demostrado ser efectiva. |
| `last_used` | Timestamp del último uso. |

**Ejemplo de entrada:**
```yaml
meta_cognitive:
  strategies:
    - name: "chain_of_thought"
      success_rate: 0.87
      avg_latency_ms: 3400
      domains: ["code_generation", "debugging"]
      last_used: "2026-06-07T10:00:00Z"
    - name: "causal_simulation"
      success_rate: 0.92
      avg_latency_ms: 8200
      domains: ["infrastructure_optimization", "incident_response"]
      last_used: "2026-06-07T09:30:00Z"
```

### 7.3 Mecanismos de Olvido y Fortalecimiento

La memoria del ASA no es estática: evoluciona con la experiencia mediante mecanismos matemáticos de olvido y fortalecimiento, inspirados en la psicología cognitiva y la neurociencia.

**Fortalecimiento por uso (Ley de Hebb):** Las reglas semánticas y políticas procedurales que son consultadas con frecuencia refuerzan su `confidence` y su peso en el proceso de decisión. Cada vez que una regla predice correctamente un resultado, su `confidence` se incrementa.

**Olvido por desuso (Decaimiento Exponencial):** Las reglas que no son consultadas durante largos períodos decaen. El campo `decay_rate` controla la velocidad de este decaimiento. Cuando la `confidence` de una regla cae por debajo de 0.5, se marca para revisión y puede ser eliminada.

**Compresión por inactividad:** Las memorias episódicas no críticas (episodios antiguos sin lecciones aprendidas significativas) se comprimen a representaciones más densas o se consolidan en reglas semánticas.

**Consolidación:** Los patrones recurrentes detectados en la memoria episódica se consolidan en reglas semánticas y políticas procedurales. Por ejemplo, si el agente observa repetidamente que "los picos de latencia fuera de hora pico correlacionan con campañas de marketing", esta observación se promueve de episodio a regla semántica.

## 8. Meta Cognición y Razonamiento Neuro Simbólico Híbrido

El Nivel 3 (Orquestado) incorpora dos capacidades que lo distinguen radicalmente de los niveles inferiores y de la mayoría de los frameworks de agentes existentes: la **meta‑cognición** y el **razonamiento neuro‑simbólico híbrido**. Ambas trabajan en conjunto: la meta‑cognición selecciona la mejor estrategia para razonar; el motor neuro‑simbólico verifica que la hipótesis generada cumple todas las políticas antes de ejecutarse.

### 8.1 Meta‑Cognición: Aprender a Razonar Mejor

#### 8.1.1 Fundamento Teórico

La meta‑cognición es la capacidad de reflexionar sobre los propios procesos de pensamiento. En psicología cognitiva, se distingue entre:

- **Conocimiento meta‑cognitivo:** Saber qué estrategias de razonamiento existen y en qué contextos funcionan mejor.
- **Regulación meta‑cognitiva:** Seleccionar y ajustar dinámicamente la estrategia más adecuada para cada tarea.

ASA implementa ambas dimensiones en su memoria meta‑cognitiva.

#### 8.1.2 Estrategias de Razonamiento como Objetos de Primera Clase

En el ASA, las estrategias de razonamiento no están hardcodeadas. Son objetos de primera clase almacenados en la memoria meta‑cognitiva con métricas de rendimiento:

| Estrategia | Descripción | Latencia típica | Mejor dominio |
|------------|-------------|-----------------|---------------|
| **Chain‑of‑Thought (CoT)** | Razonamiento paso a paso en lenguaje natural. | 3‑5 segundos | Generación de código, debugging, documentación. |
| **Tree‑of‑Thoughts (ToT)** | Exploración de múltiples ramas de razonamiento, con poda y selección. | 10‑15 segundos | Planificación arquitectónica, coordinación multi‑agente, evaluación de riesgos. |
| **Causal Simulation** | Simulación contrafactual con MCTS sobre el Grafo Causal Probabilístico. | 8‑12 segundos | Optimización de infraestructura, respuesta a incidentes, planificación de capacidad. |

Cada estrategia registra:
- **success_rate:** Proporción de hipótesis validadas vs. falsadas cuando se usó esta estrategia.
- **avg_latency_ms:** Tiempo promedio de ejecución.
- **domains:** Lista de dominios donde ha demostrado ser efectiva.
- **last_used:** Timestamp del último uso.

#### 8.1.3 Selección Dinámica de Estrategia

Durante la fase de Razonar (fase 2), el agente N3 consulta su memoria meta‑cognitiva y selecciona la estrategia más adecuada para la tarea actual:

1. **Identificar el dominio de la tarea** (ej: "infraestructura", "código", "planificación").

2. **Filtrar estrategias** que hayan demostrado efectividad en ese dominio.

3. **Seleccionar la de mayor success_rate**, ponderada por latencia (si la tarea es urgente, se penalizan las estrategias lentas).

4. **Ejecutar el razonamiento** con la estrategia seleccionada.

5. **Registrar el rendimiento** en la memoria meta‑cognitiva tras la reflexión (fase 7).

**Ejemplo:**
```
Agente N3, fase Razonar:
  Tarea: "Optimizar la latencia del checkout en producción".
  Dominio identificado: infrastructure_optimization.
  
  Consulta a memoria meta‑cognitiva:
    - CoT: success_rate 0.87, domains: ["code_generation", "debugging"]
    - Causal Simulation: success_rate 0.92, domains: ["infrastructure_optimization", "incident_response"]
    - ToT: success_rate 0.78, domains: ["architectural_planning", "multi_agent_coordination"]
  
  Selección: Causal Simulation (mayor success_rate en el dominio, 0.92).
  Decisión: "Usaré simulación causal para predecir los efectos de la intervención."
```

### 8.2 Razonamiento Neuro‑Simbólico Híbrido: La Fase Verificar

#### 8.2.1 Fundamento Teórico

Los LLM son potentes pero no deterministas. Pueden generar hipótesis creativas, pero también pueden alucinar, violar restricciones de seguridad o proponer acciones que incumplen contratos semánticos. El razonamiento neuro‑simbólico híbrido combina:

- **Razonamiento neuronal (LLM):** Flexibilidad, creatividad, comprensión del lenguaje natural, capacidad de generalización.
- **Razonamiento simbólico (Motor de reglas):** Determinismo, verificabilidad, cumplimiento estricto de políticas, trazabilidad.

La fase Verificar (fase 4, exclusiva de N3) es la materialización de esta combinación: el LLM genera la hipótesis; el motor de reglas la valida antes de ejecutarla.

#### 8.2.2 Componentes del Motor de Verificación

El motor de verificación se apoya en tres fuentes de reglas:

1. **Contratos semánticos (JSON Schema):** Declarados en `ASA.yaml` para cada capacidad del agente. Especifican el formato y las restricciones de las entradas y salidas.

2. **Políticas de seguridad (OPA):** Declaradas en `Noosfile.yaml`. Definen qué acciones están permitidas para cada nivel de agencia y en qué condiciones.

3. **Restricciones constitucionales:** Declaradas en `constitution.md`. Principios inviolables que el agente no puede transgredir bajo ninguna circunstancia.

#### 8.2.3 Flujo de Verificación

1. **Entrada:** La hipótesis generada por el LLM en la fase 3.

2. **Validación de contratos semánticos:** El motor verifica que los parámetros de la acción propuesta cumplen los JSON Schema declarados en `ASA.yaml`. Por ejemplo: ¿el nombre de la base de datos cumple el patrón `^[a-z][a-z0-9_]*$`? ¿El tamaño está entre 1 y 1000 GB?

3. **Validación de políticas de seguridad:** El motor consulta las políticas OPA declaradas en `Noosfile.yaml`. Por ejemplo: ¿el agente que propone la acción tiene nivel suficiente? ¿La acción está permitida para ese nivel?

4. **Validación constitucional:** El motor verifica que la acción no viola ningún principio de `constitution.md`. Por ejemplo: ¿la intervención afecta a zonas críticas (UCI, quirófanos)? ¿Se compromete la autonomía de baterías para equipos de soporte vital?

5. **Resultado:**
   - **Autorizado:** La hipótesis pasa a la fase de Ejecutar (fase 5).
   - **Rechazado:** La hipótesis se bloquea, se registra en el ledger de auditoría con el motivo del rechazo y se notifica al orquestador.

#### 8.2.4 Ejemplo de Verificación

```
Agente N3 (hospital), fase Verificar:
  Hipótesis H018: "Pre‑enfriar consultas externas (22→20°C) entre 13:00‑14:00."

  Validación de contratos semánticos:
    ✅ Zona "consultas_externas" existe en el modelo del mundo.
    ✅ Temperatura objetivo (20°C) está dentro del rango permitido (18‑26°C).

  Validación de políticas OPA:
    ✅ Agente energy‑agent (N2) está autorizado para modificar HVAC.
    ✅ La zona "consultas_externas" no es una zona crítica (UCI, quirófanos).

  Validación constitucional:
    ✅ No se compromete la autonomía de baterías (SOC mínimo garantizado).
    ✅ Las temperaturas previstas cumplen la norma (máx 24°C transitorio).

  Resultado: AUTORIZADO. Hipótesis H018 pasa a Ejecutar.
```

### 8.3 Sinergia entre Meta‑Cognición y Verificación Neuro‑Simbólica

La combinación de meta‑cognición y verificación neuro‑simbólica crea un ciclo virtuoso en el N3:

1. **La meta‑cognición** selecciona la mejor estrategia de razonamiento para la tarea.
2. **El LLM** (razonamiento neuronal) genera una hipótesis creativa y contextualizada.
3. **El motor de reglas** (razonamiento simbólico) verifica que la hipótesis cumple todas las políticas.
4. **La reflexión** (fase 7) actualiza la memoria meta‑cognitiva con el rendimiento real de la estrategia utilizada.

Este ciclo permite que el agente no solo aprenda sobre el mundo (a través del Grafo Causal y la memoria semántica), sino que **aprenda a aprender**: mejora sus propias estrategias de razonamiento con la experiencia.

## 9. Grafo Causal Probabilístico

El Grafo Causal Probabilístico es el núcleo del modelo del mundo del ASA. A diferencia de otros frameworks que modelan relaciones causales de forma determinista ("X causa Y"), ASA modela la incertidumbre inherente a cualquier intervención mediante distribuciones de probabilidad condicional P(Y | do(X)). Esto permite al agente cuantificar su incertidumbre, simular contrafactuales con intervalos de confianza y refinar sus predicciones a medida que acumula experiencia.

### 9.1 Fundamento Teórico: Causalidad Probabilística

El marco de inferencia causal desarrollado por Judea Pearl [5] introdujo el operador `do(X)` para distinguir entre:

- **P(Y | X):** Probabilidad de Y dado que *observo* X. Es una asociación estadística, no necesariamente causal.
- **P(Y | do(X)):** Probabilidad de Y dado que *intervengo* sobre X. Es una relación causal: ¿qué le pasaría a Y si fuerzo X a tomar un valor determinado?

La diferencia es fundamental. Observar que los pacientes que toman un medicamento se curan más rápido (P(curación | medicamento)) no implica que el medicamento cause la curación; podría ser que los pacientes más sanos tienden a tomar el medicamento. Solo una intervención controlada (do(medicamento)) puede revelar la relación causal.

**Implicación para ASA:** Los agentes no son observadores pasivos; son interventores activos. Cuando un agente decide "añadir un índice en la base de datos", está ejecutando `do(índice)`. El Grafo Causal Probabilístico modela exactamente esta semántica: cada arista almacena P(efecto | do(causa)), y el agente actualiza estas distribuciones tras cada intervención real.

### 9.2 Estructura del Grafo

El Grafo Causal Probabilístico se materializa en el archivo `causal_graph.proto`, un esquema protobuf con tres componentes principales:

#### 9.2.1 Nodos (CausalNode)

Cada nodo representa una variable del sistema. Los nodos se clasifican en cuatro tipos:

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **OBSERVABLE** | Variable que puede medirse directamente. | `db_query_latency_p95` |
| **CONTROLLABLE** | Variable sobre la que el agente puede intervenir. | `index_presence` (el agente decide si añade un índice). |
| **LATENT** | Variable no observable directamente, debe inferirse. | `user_frustration` (no se mide, pero se infiere de la tasa de abandono). |
| **OBJECTIVE** | Variable objetivo que el agente busca optimizar. | `checkout_latency_p95` (el SLO a cumplir). |

Cada nodo incluye su valor actual, su unidad de medida y una referencia al Gemelo Digital Unificado (Tejido T3) para su sincronización con el estado real del sistema.

#### 9.2.2 Aristas (CausalEdge)

Cada arista representa una relación causal dirigida desde un nodo causa hasta un nodo efecto. Las aristas se clasifican en tres tipos:

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **DIRECT** | Relación causal directa. | `do(índice) → db_query_latency_p95` |
| **REINFORCING_LOOP** | Parte de un bucle de refuerzo (amplifica cambios). | `más tráfico → más carga DB → más latencia → auto‑scaling → más capacidad → más tráfico` |
| **BALANCING_LOOP** | Parte de un bucle de equilibrio (estabiliza). | `más latencia → auto‑scaling añade nodos → menos latencia` |

Cada arista especifica el orden del efecto (1º, 2º o 3º orden) y una referencia a la distribución de retardo correspondiente en `delays.yaml`.

#### 9.2.3 Distribuciones de Probabilidad (ProbabilityDistribution)

Cada arista tiene asociada una distribución de probabilidad condicional que modela P(efecto | do(causa)). ASA soporta cinco tipos de distribuciones:

| Tipo | Parámetros | Cuándo usarla |
|------|------------|---------------|
| **GAUSSIAN** | [media, desviación estándar] | Efectos continuos con variabilidad simétrica (ej: latencia). |
| **BETA** | [alpha, beta] | Efectos acotados entre 0 y 1 (ej: tasa de conversión). |
| **EMPIRICAL** | [bin_edges..., counts...] | Cuando no se ajusta a una distribución paramétrica conocida. |
| **POISSON** | [lambda] | Efectos de conteo (ej: número de errores por hora). |
| **UNIFORM** | [min, max] | Cuando no hay información suficiente para preferir un valor sobre otro. |

Cada distribución incluye:

- **confidence:** Confianza del agente en esta estimación (0.0 a 1.0). Se actualiza tras cada ciclo de reflexión.
- **evidence_source:** Hipótesis o episodio que proporcionó la evidencia para esta estimación.
- **sample_size:** Número de observaciones utilizadas para estimar los parámetros.
- **last_updated:** Timestamp de la última actualización.

### 9.3 Actualización por Reflexión

El Grafo Causal Probabilístico no es estático. Se actualiza en cada ciclo de reflexión (fase 7) mediante inferencia bayesiana:

1. **Recolección de evidencia:** Tras ejecutar una intervención, el agente observa el resultado real (fase 6: Evaluar).

2. **Comparación con la predicción:** El resultado real se compara con la distribución predicha en la hipótesis (fase 3).

3. **Actualización de parámetros:** Si la distribución es Gaussiana, por ejemplo, la media y la desviación estándar se actualizan combinando la estimación previa con la nueva observación, ponderadas por el `sample_size`.

4. **Incremento de la confianza:** Si el resultado real está dentro del intervalo de confianza predicho, la `confidence` de la arista se incrementa. Si no, se reduce.

5. **Detección de nuevas relaciones:** Si el resultado muestra un efecto no predicho (ej: una variable que no estaba en el grafo resultó afectada), se añade un nuevo nodo y una nueva arista.

**Ejemplo concreto (e‑commerce, N3):**
```
Antes de la intervención (hipótesis H012):
  P(checkout_latency_p95 | do(índice)) ~ N(480ms, 25ms)
  confidence: 0.89, sample_size: 22

Después de la intervención (resultado real):
  checkout_latency_p95 = 510ms

Actualización por reflexión:
  - La nueva media se recalcula: (480 * 22 + 510) / 23 = 481.3ms
  - La nueva desviación estándar se ajusta ligeramente: ~24ms
  - La confidence sube de 0.89 a 0.91 (el resultado real estuvo dentro del IC 95%)
  - evidence_source: "H012-2026-06-07 (validated, minor deviation)"
```

### 9.4 Diferencia con Modelos Deterministas

La mayoría de los frameworks de agentes que modelan causalidad lo hacen de forma determinista: "si añado un índice, la latencia bajará a 20ms". ASA adopta un enfoque probabilístico por tres razones:

| Modelo Determinista | Modelo Probabilístico (ASA) |
|---------------------|-----------------------------|
| Predice un valor puntual (ej: latencia = 20ms). | Predice una distribución (ej: latencia ~ N(480ms, 25ms)). |
| No cuantifica la incertidumbre. | Proporciona intervalos de confianza (ej: IC 95%: [430ms, 530ms]). |
| No aprende de las desviaciones. | Actualiza sus parámetros y su confianza tras cada intervención. |
| Frágil ante situaciones no previstas. | Robusto: si el resultado real se desvía, el modelo se ajusta. |

La evidencia empírica respalda este enfoque. El estudio MAGMA (abril 2026) demostró que eliminar los grafos causales en agentes reduce la precisión en un 24.6%. ASA ya incorpora esta lección desde su diseño fundacional.

### 9.5 Integración con la Simulación Contrafactual

Durante la fase de Hipótesis (fase 3), el agente N3 utiliza el Grafo Causal Probabilístico para ejecutar simulaciones contrafactuales mediante Monte Carlo Tree Search (MCTS):

1. **Selección de la intervención:** El agente identifica qué variable va a intervenir (`do(X)`).

2. **Propagación probabilística:** Para cada nodo hijo de la variable intervenida, se muestrea de la distribución P(hijo | do(padre)). Este proceso se repite recursivamente (N=1000 iteraciones por defecto) para generar una distribución de resultados posibles.

3. **Evaluación de escenarios:** Para cada escenario simulado, se calcula el valor esperado del objetivo, el intervalo de confianza al 95% y la probabilidad de violar restricciones (ej: conversión < línea base).

4. **Selección de la mejor acción:** El agente elige la intervención con mayor valor esperado que cumpla todas las restricciones.

Esta capacidad de simular contrafactuales distingue al ASA de los agentes reactivos: no solo reacciona al estado actual del sistema, sino que explora proactivamente escenarios alternativos antes de comprometerse con una acción.

## 10. Estándares y Herramientas Abiertas

La arquitectura ASA se construye sobre un ecosistema de **estándares abiertos y herramientas de código abierto**, garantizando la interoperabilidad, la portabilidad y la ausencia de dependencias de software propietario.

### 10.1 Principio de no dependencia propietaria

ASA se adhiere al principio de que **ningún componente esencial del ecosistema debe depender de software propietario o de un proveedor específico**. Las únicas menciones a servicios comerciales (como OpenAI o Anthropic) son como ejemplos de fallback opcional para la inferencia en la nube, configurable y desactivado por defecto.

### 10.2 Estándares y protocolos adoptados

| Estándar | Uso en ASA | Gobernanza | Licencia |
| :--- | :--- | :--- | :--- |
| **A2A (Agent-to-Agent)** | Comunicación y delegación de tareas entre agentes. | Linux Foundation | Especificación abierta |
| **MCP (Model Context Protocol)** | Conexión de agentes a herramientas y fuentes de datos. | Anthropic (estándar abierto) | Especificación abierta |
| **SLIM / AGNTCY** | Capa de mensajería segura y de baja latencia. | Cisco / Linux Foundation | Especificación abierta |
| **SPIFFE** | Identidad verificable para cargas de trabajo (agentes). | CNCF (Graduated) | Especificación abierta |
| **OPA (Open Policy Agent)** | Motor de políticas de autorización declarativas. | CNCF (Graduated) | Apache 2.0 |
| **mTLS** | Cifrado extremo a extremo en las comunicaciones. | IETF | Estándar abierto |
| **JSON Schema / Protobuf** | Definición de contratos semánticos y estructura de datos. | OpenJS / Google | Open source |

### 10.3 Herramientas open source de la arquitectura

| Herramienta | Función | Licencia |
| :--- | :--- | :--- |
| **Ollama** | Inferencia local simplificada (desarrollo y prototipado). | MIT |
| **llama.cpp** | Inferencia local con control fino y portabilidad extrema. | MIT |
| **vLLM** | Inferencia local de alto rendimiento para cargas concurrentes. | Apache 2.0 |
| **CLIPS** | Motor de reglas simbólicas para la fase "Verificar". | Dominio público |
| **Drools** | Motor de reglas simbólicas alternativo. | Apache 2.0 |
| **LanceDB / FAISS / SQLite-vec** | Base de datos vectorial para búsqueda semántica (memoria a largo plazo). | Apache 2.0 / MIT |
| **Kubernetes / K3s** | Orquestación de contenedores para el despliegue de agentes. | Apache 2.0 |
| **Istio** | Service mesh para la gestión del tráfico y seguridad mTLS. | Apache 2.0 |
| **Kafka / NATS** | Sistema de mensajería para el bus de eventos del ecosistema. | Apache 2.0 / MIT |
| **Prometheus / Grafana / Loki** | Monitorización, visualización y agregación de logs. | Apache 2.0 / AGPLv3 |
| **Neo4j Community Edition** | Base de datos de grafos para el conocimiento semántico. | GPLv3 |

### 10.4 El estándar AOF-S y herramientas propias

| Componente | Función | Licencia |
| :--- | :--- | :--- |
| **AOF-S** | Estándar de empaquetado y orquestación de agentes (este proyecto). | Apache 2.0 |
| **noctl** | CLI para la gestión del ciclo de vida del Noosistema (en desarrollo). | Apache 2.0 |
| **NooKepler** | Runtime de ejecución de agentes ASA (en desarrollo). | Apache 2.0 |


## 11. Modos de Operación del N3: Degradación Graciosa y Delegación Inteligente

### 11.1 Definición

La **degradación graciosa** es la capacidad del N3 de omitir fases de su ciclo operativo o delegar tareas a agentes de menor nivel (N2/N1) cuando la complejidad de la tarea no justifica la ejecución del ciclo completo. Esta capacidad se deriva del principio de agencia progresiva y de la meta‑cognición del N3.

### 11.2 Modos de Operación

| Modo | Ciclo | Contexto de Uso |
| :--- | :--- | :--- |
| **N3 Completo** | 7 fases | Capa C5 (Orquestación). Tareas que requieren simulación contrafactual, verificación neuro‑simbólica o coordinación multi‑agente. |
| **N3 Degradado** | 5 fases | Capa C3 (Edge). Tareas locales que no requieren razonamiento causal profundo ni verificación formal. |
| **N3 Delegando** | 4 fases | Tareas rutinarias donde existe un N2 o N1 especializado disponible. El N3 actúa como enrutador inteligente. |

### 11.3 Principio de no orquestación para N2

La capacidad de orquestación (coordinación de otros agentes, mantenimiento del `registry.yaml`, aplicación de políticas globales) es **exclusiva del N3**. Un agente N2 no puede asumir funciones de orquestación, ni siquiera en modo local. La orquestación local en edge (C3) debe ser asumida por un N3 que opere en modo degradado.

## 12. Gobernanza Algorítmica y Seguridad

La gobernanza algorítmica es la capacidad del ecosistema de agentes para garantizar que cada acción autónoma sea trazable, auditable y conforme a las políticas declaradas. En el Modelo ASA, esta gobernanza no es una capa externa añadida a posteriori, sino un principio estructural integrado en el propio ciclo operativo.

### 12.1 Fundamento Teórico: Gobernanza por Diseño

La mayoría de los frameworks de agentes tratan la gobernanza como una preocupación secundaria: se desarrolla el agente y luego se añaden guardarraíles, logs o mecanismos de auditoría. ASA invierte este enfoque: la gobernanza está embebida en el ciclo de razonamiento desde la fase de Hipótesis hasta la fase de Reflexionar. Cada acción autónoma deja una traza completa que permite reconstruir:

- **Qué** se hizo.
- **Por qué** se hizo (hipótesis que la motivó).
- **Qué se predijo** que ocurriría (efectos de 1º, 2º y 3º orden).
- **Qué ocurrió realmente** (resultado observado).
- **Qué se aprendió** (lecciones extraídas y cambios en el modelo).

### 12.2 Los Tres Puntos de Control

El ciclo ASA implementa tres puntos de control que actúan como compuertas de verificación en tiempo de ejecución:

| Punto de Control | Fase ASA | ¿Qué verifica? | ¿Quién lo ejecuta? |
|:---|:---|:---|:---|
| **Puerta de pre‑acción** | Fase 3 (Hipótesis) / Fase 4 (Verificar) | ¿Tiene la acción una hipótesis registrada? ¿Cumple las políticas OPA? ¿Respeta los contratos semánticos? ¿Viola la constitución? | Motor neuro‑simbólico (N3) o validación declarativa (N2). |
| **Monitor de tiempo de ejecución** | Fase 5 (Ejecutar) | ¿Está apareciendo un bucle de realimentación no previsto? ¿Se ha superado algún umbral de seguridad? | Watchdog de bucles (sidecar PEP en el runtime NooKepler). |
| **Auditor post‑acción** | Fase 6 (Evaluar) / Fase 7 (Reflexionar) | ¿El resultado real coincide con la hipótesis? ¿Hay anomalías no predichas? ¿Se han actualizado el modelo y las memorias? | El propio agente (N2/N3) o la memoria colectiva (para N1). |

### 12.3 Trazabilidad Completa: El Ledger de Auditoría

Cada decisión autónoma queda registrada en un ledger de auditoría inmutable. Este ledger no es un simple archivo de logs: es un registro estructurado que vincula cada acción con su hipótesis, su verificación y su resultado.

**Formato de una entrada en el ledger:**
```yaml
- timestamp: "2026-06-07T14:30:00Z"
  agent_id: "agent-clinical-002"
  agent_level: 2
  action: "send_alert"
  hypothesis_id: "H017-2026-06-07"
  hypothesis_summary: "Paciente cama 3 UCI: descenso SpO2 96→91% en 45 min. Predicción: SpO2 < 88% en ~30 min."
  verification:
    result: "authorized"
    checks: ["hipótesis registrada", "umbral WARNING alcanzado", "HIPAA compliant"]
  execution:
    channel: "slack#enfermeria-uci"
    timestamp: "2026-06-07T14:30:05Z"
  observation:
    metric: "spo2_trend_uci_3"
    predicted_value: "88%"
    actual_value: "87%"
    deviation: "-1%"
  hypothesis_result: "validated"
```

### 12.4 Alineación con OWASP Top 10 para Aplicaciones Agénticas (2026)

El Modelo ASA incorpora mitigaciones específicas para los diez riesgos catalogados por OWASP:

| Riesgo OWASP | Mitigación en ASA |
|--------------|-------------------|
| **Inyección de Prompts** | Validación estructural de intenciones: toda acción requiere una hipótesis registrada. Sin hipótesis, la acción se bloquea. |
| **Envenenamiento de Datos** | Sandboxing (gVisor) y versionado del corpus experto (`expert_corpus/`). Las escrituras en memoria pasan por defensa bayesiana. |
| **Denial of Wallet** | Circuit breaker en `Noosfile.yaml`, carga progresiva AOF‑S, compresión autónoma de contexto. |
| **Agencia Excesiva** | Puertas de capacidad por nivel de agencia (N1 solo ejecuta su tarea; N3 requiere aprobación humana para acciones críticas). |
| **Fuga de Datos entre Agentes** | Segmentación por namespaces (`noos://dominio`), mTLS obligatorio, políticas OPA granulares. |
| **Supply Chain del Agente** | Identidades SPIFFE verificables para todos los componentes. Skills versionadas y firmadas. |
| **Deriva de Contexto** | Anclaje al contexto mediante especificaciones ejecutables (AOF‑S como SDD). Verificación de contratos semánticos. |
| **Alucinación de Herramientas** | Motor neuro‑simbólico (fase Verificar) que valida las hipótesis contra contratos JSON Schema antes de ejecutar. |
| **Bucle de Consumo Descontrolado** | Watchdog de bucles en la fase Ejecutar. Límites de tokens por sesión. Compresión autónoma. |
| **Ingeniería Social sobre el Agente** | Constitución (`constitution.md`) con principios inviolables. Aprobación humana para acciones críticas. |

### 12.5 Alineación con NIST AI RMF 1.0

El ciclo ASA implementa los cuatro pilares del marco de gestión de riesgos de IA del NIST:

| Pilar NIST AI RMF | Implementación en ASA |
|--------------------|-----------------------|
| **Map (Mapear)** | El agente mantiene un modelo del mundo (`world_model/`) y un Grafo Causal Probabilístico que mapea las variables del sistema, sus relaciones y los riesgos asociados. |
| **Measure (Medir)** | Las métricas de evaluación (`evaluation` en `ASA.yaml`), la tasa de validación de hipótesis y el `trust_score` proporcionan mediciones continuas del rendimiento y la fiabilidad del agente. |
| **Manage (Gestionar)** | Las políticas procedurales (`policies.yaml`), las políticas OPA declarativas y los guardarraíles (Tejido T1) gestionan activamente los riesgos en tiempo de ejecución. |
| **Govern (Gobernar)** | La constitución (`constitution.md`), el `Noosfile` y el ledger de auditoría proporcionan un marco de gobernanza trazable y auditable. |

## 13. Principio Local First

El Principio 0 del Modelo ASA establece que la ejecución local es el camino predeterminado para todo agente. La nube se degrada explícitamente a un respaldo opcional, configurable bajo demanda.

### 13.1 Asociación de Niveles a Configuraciones Hardware

| Nivel | Hardware Recomendado | VRAM/RAM | Motores de Inferencia | Modelos de Ejemplo |
|-------|---------------------|----------|----------------------|-------------------|
| **N1 (Reactivo)** | CPU, Raspberry Pi 5, laptops | 4‑8 GB RAM | llama.cpp (CPU), Ollama (CPU) | Llama 3.2 3B, Qwen 2.5 Coder 7B (Q4_K_M) |
| **N2 (Sistémico)** | GPU 8‑12 GB VRAM, Apple Silicon 16‑32 GB | 8‑12 GB VRAM | Ollama, llama.cpp | Llama 3.1 8B, Qwen 2.5 14B |
| **N3 (Orquestado)** | GPU 18 GB+ VRAM, DGX Spark, Clúster | 18‑80 GB VRAM | Ollama, llama.cpp, vLLM | Llama 3.1 70B, Qwen 2.5 32B |

### 13.2 Configuración Declarativa

La sección `inference` en `ASA.yaml` permite declarar los motores locales, los modelos asignados y las restricciones de recursos.

---

## 14. Relación con AOF S y el Noosistema

El Modelo ASA no existe en el vacío. Es el pilar central de un ecosistema más amplio compuesto por tres elementos interdependientes: el **Noosistema** (el ecosistema donde los agentes colaboran), **ASA** (la arquitectura del agente) y **AOF‑S** (el estándar de archivos que materializa ASA).

### 14.1 El Triángulo del Noosistema

| Pilar | ¿Qué es? | ¿Qué problema resuelve? |
|-------|----------|------------------------|
| **Noosistema** | El ecosistema cognitivo digital estratificado. | Define el escenario: los agentes no trabajan aislados, sino en una red estratificada con memoria colectiva, cierre de ciclo diferencial y gobernanza compartida. |
| **ASA** | La arquitectura del Agente Sistémico Adaptativo. | Resuelve la amnesia, la falta de modelo causal y la ausencia de niveles de agencia progresiva. |
| **AOF‑S** | El estándar abierto de archivos que materializa ASA en el Noosistema. | Resuelve la fragmentación: un formato versionable, abierto y portable que cualquier LLM puede leer y escribir. |

### 14.2 Cómo ASA se Materializa en AOF‑S

Cada concepto de ASA tiene su correspondencia directa en un artefacto AOF‑S:

| Concepto ASA | Materialización en AOF‑S |
|--------------|--------------------------|
| **Principio de Intervención Reflexiva Causal** | Ciclo de 7 fases documentado en `spec/asa.yaml`. El agente lee y escribe `hypothesis.md`, `observations/` y `policies.yaml`. |
| **Tres niveles de agencia** | Plantillas `templates/reactive/`, `templates/systemic/`, `templates/orchestrated/`. |
| **Ciclo ASA de 7 fases** | Matriz de carga progresiva en `spec/aof-s.yaml`. Cada fase carga un subconjunto de artefactos. |
| **Memoria cuádruple** | Directorios `06_memory/episodic/`, `06_memory/semantic/`, `06_memory/procedural/` y archivo `meta_strategies.yaml`. |
| **Grafo Causal Probabilístico** | Archivo `04_knowledge/world_model/causal_graph.proto`. |
| **Verificación neuro‑simbólica** | Fase Verificar documentada en `spec/asa.yaml` y motor de reglas CLIPS/Drools configurado en `ASA.yaml`. |
| **Principio Local‑First** | Sección `inference` en `ASA.yaml` y `03_agent_spec.yaml`. |
| **Seguridad Zero Trust** | Sección `security` en `Noosfile.yaml` y `ASA.yaml`. Identidades SPIFFE, mTLS y políticas OPA. |
| **Gobernanza algorítmica** | `hypothesis.md` (puerta pre‑acción), watchdog de bucles (monitor de ejecución), `observations/` y ledger (auditor post‑acción). |
| **Evaluabilidad** | Sección `evaluation` en `ASA.yaml` y `03_agent_spec.yaml`. Comando `noctl evaluate`. |

## 15. Limitaciones y Hoja de Ruta

ASA es una arquitectura ambiciosa y, como tal, presenta limitaciones que deben ser reconocidas explícitamente. Esta sección las enumera junto con las mitigaciones actuales y los planes para resolverlas.

### 15.1 Limitaciones reconocidas

| Limitación | Descripción | Mitigación actual | Plan de resolución |
|:---|:---|:---|:---|
| **Complejidad computacional del N3** | El ciclo de 7 fases añade latencia (especialmente Verificar y Reflexionar). En edge o tareas simples, el overhead puede ser >500ms. | N1 y N2 para edge y tareas simples. Modo degradado del N3 (5 fases). | Implementar degradación graciosa automática basada en meta‑cognición. |
| **Curva de aprendizaje alta** | ASA requiere comprender artefactos `.aof`, motores simbólicos, bases vectoriales y memoria cuádruple. | Modo ultra‑light (2 archivos, 5 minutos). Documentación extensa y guías paso a paso. | Tutoriales interactivos, plantillas sectoriales pre‑configuradas. |
| **Dependencia de un modelo causal inicial** | El grafo causal requiere definición previa de relaciones P(Y do(X)). En dominios nuevos, el grafo estará vacío o erróneo. | El N2 construye el grafo incrementalmente desde la experiencia. | Herramientas de bootstrap causal desde datos históricos. |
| **Consumo de memoria y almacenamiento** | La memoria cuádruple crece linealmente con el tiempo de operación. En edge o IoT, la huella puede ser prohibitiva. | N1 no persiste nada. N2 persiste solo lo esencial. Compresión autónoma de contexto. | Implementar `noctl memory compact` para consolidación periódica. |
| **Falta de implementación de referencia** | ASA existe como especificación y plantillas, pero no hay un runtime funcional que valide las hipótesis arquitectónicas. | Plantillas AOF‑S utilizables con cualquier LLM. CLI `noctl` en desarrollo. | NooKepler (runtime Rust) y runtime Python de referencia. |
| **Ecosistema inmaduro** | No hay foros, tutoriales extensivos ni comunidad activa. La integración con motores simbólicos (CLIPS/Drools) no está estandarizada. | Repositorio público, documentación extensa, ejemplos sectoriales. | Benchmarks comparativos, workshop en conferencias, propuesta de estandarización en Linux Foundation. |
| **Orquestación multiagente no validada** | La NSL y la federación de agentes no tienen una implementación funcional que demuestre su viabilidad en entornos reales. | Especificación detallada de NSL, A2A y MCP. | Pilotos sectoriales (Hospital, Datacenter) como validación empírica. |

### 15.2 Hoja de Ruta

| Horizonte | Hito | Estado |
|:---|:---|:---|
| **Corto plazo** | Implementación de referencia en Python del ciclo ASA. | Pendiente |
| **Corto plazo** | Benchmarks comparativos ASA vs. ReAct vs. Plan‑and‑Execute. | Pendiente |
| **Medio plazo** | Runtime NooKepler en Rust con soporte nativo para degradación graciosa. | Pendiente |
| **Medio plazo** | Pilotos sectoriales (Hospital, Datacenter). | Pendiente |
| **Largo plazo** | Estandarización bajo Linux Foundation (AAIF) o CNCF. | Pendiente |

## Conclusión

El Modelo ASA representa una evolución fundamental en la arquitectura de agentes de IA. Frente a los paradigmas reactivos dominantes (ReAct, Plan‑and‑Solve), ASA introduce la intervención reflexiva causal, tres niveles de agencia progresiva, memoria cuádruple con meta‑cognición, un Grafo Causal Probabilístico actualizable, gobernanza algorítmica por diseño y un principio Local‑First que garantiza la soberanía de datos.

ASA no es una propuesta teórica: está materializado en el estándar abierto AOF‑S, que proporciona plantillas listas para usar, una CLI de orquestación (`noctl`) y una pila de protocolos (A2A, MCP, NSL) que lo integran con el ecosistema de agentes existente.

---

**Versión:** 2.0
**Fecha:** 13 de junio de 2026
**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek (DeepThink) como asistente de diseño sistémico.
**Parte del estándar:** AOF‑S v1.0
```