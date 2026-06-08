# AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual

**Propósito:** Este documento desarrolla los fundamentos conceptuales del estándar AOF‑S (AI Orchestration Files for Systemic Agents), el tercer pilar del Noosistema. Complementa la especificación ejecutable (`spec/aof-s.yaml`) con la narrativa que explica el "por qué" y el "cómo" de su diseño como estándar abierto para el empaquetado, la gobernanza y la orquestación de agentes ASA.

**Versión:** 1.0
**Fecha:** 8 de junio de 2026
**Parte del estándar:** AOF‑S v1.0

---

## Tabla de Contenidos

1. [Definición y Fundamentos](#1-definición-y-fundamentos)
2. [Principios de Diseño](#2-principios-de-diseño)
3. [Estructura de Archivos y Niveles](#3-estructura-de-archivos-y-niveles)
4. [Noosfile y Orquestación Declarativa](#4-noosfile-y-orquestación-declarativa)
5. [Capa Semántica NSL](#5-capa-semántica-nsl)
6. [Seguridad Zero Trust Declarativa](#6-seguridad-zero-trust-declarativa)
7. [Principio Local‑First en AOF‑S](#7-principio-local-first-en-aof-s)
8. [Configurabilidad Sectorial](#8-configurabilidad-sectorial)
9. [Relación con ASA y el Noosistema](#9-relación-con-asa-y-el-noosistema)

---

## 1. Definición y Fundamentos

### 1.1 Definición Canónica

**AOF‑S (AI Orchestration Files for Systemic Agents)** es un estándar abierto de archivos versionables (Markdown + YAML) que materializa los artefactos cognitivos del Modelo ASA. Define cómo se empaqueta, gobierna y orquesta un agente de IA, aplicando los principios de **Spec‑Driven Development (SDD)** y **Local‑First** al ecosistema de agentes. AOF‑S no es otro formato de configuración estática; es el "sistema operativo" y el "Dockerfile" del agente: un conjunto de especificaciones ejecutables que el agente lee y escribe a lo largo de su ciclo de vida, optimizando el consumo de tokens mediante carga progresiva y garantizando la trazabilidad completa de sus decisiones.

AOF‑S es el tercer pilar del triángulo del Noosistema: el Noosistema define el ecosistema donde los agentes colaboran; ASA define la arquitectura de cada agente; AOF‑S define el estándar que materializa ASA y gobierna el Noosistema.

### 1.2 El Problema que Resuelve AOF‑S

En 2026, el ecosistema de agentes de IA está fragmentado. Cada framework (LangChain, CrewAI, AutoGen), cada herramienta (Claude Code, Cursor, Copilot) y cada proyecto define su propia manera de proporcionar contexto a los agentes: `AGENTS.md`, `prompts.txt`, configuraciones propietarias. Esta fragmentación genera tres problemas fundamentales:

1. **Amnesia del agente:** Los archivos de contexto son estáticos. El agente los lee al inicio, pero no escribe en ellos. Su aprendizaje se pierde entre sesiones. No hay memoria persistente ni trazabilidad de decisiones.

2. **Vendor lock‑in:** Cada plataforma define sus propias estructuras. Un agente diseñado para LangChain no puede migrarse fácilmente a CrewAI. No hay un formato portable que cualquier LLM pueda leer y escribir.

3. **Falta de gobernanza:** No hay un mecanismo estándar para declarar políticas de seguridad, evaluar el rendimiento del agente o verificar que sus acciones cumplen las normas antes de ejecutarlas. La gobernanza se añade a posteriori, si es que se añade.

AOF‑S resuelve estos tres problemas mediante un estándar abierto, versionable y Spec‑Driven que cualquier LLM puede adoptar.

### 1.3 Spec‑Driven Development (SDD) Aplicado a Agentes

AOF‑S se fundamenta en el paradigma de **Spec‑Driven Development (SDD)** , que invierte el flujo de trabajo tradicional: la especificación es el origen de la verdad, y el comportamiento del agente es un artefacto verificado a partir de esa especificación.

En AOF‑S, cada archivo es una **especificación ejecutable** que el agente debe cumplir:

| Artefacto AOF‑S | Especificación que define |
|-----------------|--------------------------|
| `Noosfile.yaml` | El estado deseado del ecosistema (agentes, políticas, seguridad, evaluación). |
| `ASA.yaml` | La identidad, capacidades y perfil semántico de un agente. |
| `mission.yaml` | La necesidad de negocio a satisfacer, con restricciones y métricas de éxito. |
| `hypothesis.md` | La intervención prevista con efectos de orden superior y métricas de validación. |
| `causal_graph.proto` | El modelo causal del sistema, con distribuciones de probabilidad actualizables. |
| `constitution.md` | Los principios inviolables de gobernanza algorítmica. |

El ciclo ASA verifica continuamente que el comportamiento del agente se ajusta a su especificación. Si una acción no tiene hipótesis registrada, se bloquea. Si el resultado se desvía de la predicción, el modelo se actualiza. AOF‑S convierte la especificación de un documento descriptivo a un contrato ejecutable.

### 1.4 AOF‑S como Formato Vivo

A diferencia de un `AGENTS.md` estático, los artefactos AOF‑S son **leídos y escritos por el agente** como parte de su ciclo operativo:

- **Antes de actuar:** El agente escribe `hypothesis.md` con sus predicciones.
- **Después de actuar:** El agente escribe `observations/feedback_log.yaml` con los resultados.
- **Tras reflexionar:** El agente actualiza `learned_rules.yaml` con nuevas reglas semánticas y `policies.yaml` con nuevas políticas procedurales.
- **Periódicamente:** El agente actualiza `causal_graph.proto` con distribuciones de probabilidad refinadas.

Esto convierte a AOF‑S en un **formato vivo** que evoluciona con la experiencia del agente, manteniendo un registro versionable y auditable de todo el aprendizaje.

## 2. Principios de Diseño

Los ocho principios de diseño de AOF‑S no son convenciones arbitrarias ni buenas prácticas genéricas. Son la respuesta directa a las limitaciones identificadas en los enfoques actuales de agentes de IA: la amnesia entre sesiones, la fragmentación de formatos, la dependencia de la nube, la falta de gobernanza algorítmica y la ausencia de un modelo de aprendizaje continuo. Cada principio aborda una dimensión específica del problema y, juntos, definen la identidad del estándar.

### Principio 0: Local‑First por Diseño

La ejecución local es el camino predeterminado para todo agente AOF‑S. La nube se degrada explícitamente a un respaldo opcional, configurable bajo demanda.

**Qué significa en la práctica:**
- Los artefactos AOF‑S residen en el sistema de archivos local del dispositivo donde se ejecuta el agente. No se requiere conectividad a internet para que el agente opere.
- La sección `inference` en `ASA.yaml` declara motores de inferencia locales (Ollama, llama.cpp, vLLM) con prioridades y modelos asignados. El fallback a la nube se configura explícitamente y solo se activa bajo condiciones definidas (saturación de recursos, tarea que requiere un modelo no disponible localmente).
- Un agente Reactivo (N1) puede ejecutarse en una Raspberry Pi con un modelo cuantizado de 3B parámetros. Un agente Orquestado (N3) puede requerir una GPU de 18GB+ VRAM. En ambos casos, la nube es opcional.

**Por qué es necesario:**
La industria de agentes en 2026 está convergiendo hacia la ejecución local por tres razones: soberanía de datos (el contexto sensible no abandona el dispositivo), eliminación de costes por token (crítico para agentes autónomos de larga duración) y operación offline (esencial para edge computing, entornos hospitalarios o infraestructuras críticas aisladas). Proyectos como el Enterprise Local‑First Gateway (ELG), el Cognitive Loop Kernel (CLK) y SuperLocalMemory validan esta tendencia.

### Principio 1: Separación Cognitiva

Cada archivo en la estructura AOF‑S responde a una pregunta cognitiva precisa. La arquitectura del agente no se define en un monolito, sino en un conjunto de artefactos especializados que pueden ser leídos, escritos y versionados de forma independiente.

**Qué significa en la práctica:**
- `00_manifest.yaml` responde a "¿quién soy y en qué nivel opero?".
- `01_context_static.md` responde a "¿cuál es el stack, los comandos y las reglas inmutables de mi proyecto?".
- `02_mission.yaml` responde a "¿qué necesidad debo satisfacer y cómo se mide el éxito?".
- `hypothesis.md` responde a "¿qué voy a hacer, qué espero que ocurra y cómo lo validaré?".
- `learned_rules.yaml` responde a "¿qué he aprendido de mis experiencias pasadas?".

**Por qué es necesario:**
La separación cognitiva permite la carga progresiva (Principio 2), facilita la auditoría (cada decisión está vinculada a un artefacto concreto) y permite que diferentes componentes del ecosistema se actualicen sin afectar a los demás. Un cambio en la misión no requiere reescribir el modelo del mundo; una nueva regla semántica no obliga a modificar la constitución.

### Principio 2: Carga Progresiva

El agente no carga todos los artefactos AOF‑S al inicio de una sesión. Selecciona únicamente los archivos necesarios para la fase actual del ciclo ASA, optimizando el consumo de tokens y manteniendo la ventana de contexto bajo control.

**Qué significa en la práctica:**
La matriz de carga progresiva, documentada en `spec/aof-s.yaml`, define qué artefactos se cargan en cada fase:

| Fase | Artefactos cargados | Tokens aproximados |
|------|---------------------|--------------------|
| **Arranque** | `manifest.yaml`, `context_static.md`, `mission.yaml`, `constitution.md` | ~2‑5k |
| **Razonar** | `episodic/` (top‑N), `policies.yaml`, `variables.yaml` | +2‑3k |
| **Hipótesis** | `causal_graph.proto`, `delays.yaml`, `expert_corpus/` (RAG), `simulation_skill.md` | +3‑5k |
| **Verificar (N3)** | `verification_rules.clp`, contratos semánticos, políticas OPA | +1‑2k |
| **Ejecutar** | `current_steps.md`, skills específicas | variable |
| **Evaluar** | `feedback_log.yaml`, `anomaly_notes.md` | +1k |
| **Reflexionar** | Hipótesis activa, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml` (N3) | +2‑3k |

**Por qué es necesario:**
La ventana de contexto de los LLM es limitada y costosa. Cargar toda la memoria y el modelo del mundo en cada ciclo saturaría la ventana y degradaría el rendimiento. La carga progresiva permite que un agente Orquestado (N3) mantenga un pico máximo de ~16k tokens, muy por debajo del límite de 200k de los modelos actuales, sin sacrificar profundidad analítica.

### Principio 3: Intervención Reflexiva Causal

Toda acción del agente es una hipótesis que se verifica. El agente no se limita a ejecutar tareas; predice los efectos de sus intervenciones, los contrasta con la realidad y actualiza su modelo del mundo en función de la discrepancia.

**Qué significa en la práctica:**
- Antes de actuar, el agente escribe `hypothesis.md` con predicciones cuantitativas de efectos de 1º, 2º y 3º orden, métricas de validación y criterios de éxito.
- Después de actuar, el agente escribe `observations/feedback_log.yaml` con los resultados reales y los compara con la hipótesis.
- En la fase de reflexión, el agente actualiza el Grafo Causal Probabilístico (distribuciones de probabilidad), extrae reglas semánticas (`learned_rules.yaml`) y refina políticas procedurales (`policies.yaml`).
- Si una acción no tiene hipótesis registrada, el guardrail pre‑acción la bloquea.

**Por qué es necesario:**
Este principio es la antítesis del "vibe coding" aplicado a agentes. Sin hipótesis, no hay trazabilidad. Sin trazabilidad, no hay aprendizaje. Sin aprendizaje, el agente está condenado a repetir sus errores. La intervención reflexiva causal cierra el bucle de la gobernanza algorítmica (Tejido T1).

### Principio 4: Memoria con Propósito Cognitivo

La memoria del agente no es un log plano ni un vector store indiferenciado. Está organizada en tipos funcionales —episódica, semántica, procedural y meta‑cognitiva— inspirados en la arquitectura cognitiva humana (ACT‑R), con mecanismos matemáticos de olvido, fortalecimiento y consolidación.

**Qué significa en la práctica:**
- **Memoria episódica:** Registro cronológico de intervenciones con su contexto completo. Se consulta durante la fase de Razonar para recuperar experiencias similares.
- **Memoria semántica:** Reglas y hechos extraídos de la experiencia. Cada regla incluye campos de ciclo de vida (`confidence`, `last_accessed`, `access_count`, `decay_rate`) que permiten el olvido por desuso y el fortalecimiento por uso.
- **Memoria procedural:** Políticas de acción automatizadas ("si condición entonces acción"). Equivalen al Sistema 1 de Kahneman: respuestas rápidas y de bajo consumo cognitivo.
- **Memoria meta‑cognitiva (N3):** Rendimiento histórico de las estrategias de razonamiento. Permite al agente aprender a razonar mejor, seleccionando dinámicamente la estrategia más adecuada para cada tarea.

**Por qué es necesario:**
La memoria plana no escala. Un agente que acumula miles de episodios sin mecanismos de olvido se vuelve cada vez más lento e ineficiente. La memoria con propósito cognitivo asegura que el agente recuerde lo importante, olvide lo irrelevante y consolide patrones en conocimiento generalizable.

### Principio 5: Seguridad Zero Trust por Construcción

La seguridad no es una capa añadida a posteriori. Está integrada en el diseño del estándar desde el primer momento, con identidades verificables (SPIFFE), cifrado extremo a extremo (mTLS) y políticas de acceso declarativas (OPA) que se aplican de forma progresiva según el nivel de agencia.

**Qué significa en la práctica:**
- Cada agente recibe una identidad SPIFFE única (`spiffe://noos.dominio/agent/nombre`) declarada en `ASA.yaml`.
- Toda comunicación entre agentes utiliza mTLS gestionado por la service mesh (Istio).
- Las políticas de acceso se declaran en el `Noosfile.yaml` mediante OPA y se evalúan en tiempo real.
- La seguridad es progresiva: mínima en N1 (sandboxing del sistema operativo), automática en N2 (SPIFFE y mTLS auto‑configurados) y obligatoria en N3 (SPIFFE, mTLS y OPA requeridos).

**Por qué es necesario:**
En un ecosistema donde los agentes pueden crear otros agentes, compartir memoria y ejecutar acciones autónomas, la seguridad no puede ser opcional. El historial de vulnerabilidades en plataformas de agentes (como los CVSS 9.9 de OpenClaw) demuestra que la seguridad debe ser un pilar del diseño, no un parche posterior.

### Principio 6: Evaluabilidad Integrada

La evaluación del rendimiento del agente no es una auditoría externa que se realiza esporádicamente. Es una capacidad integrada en el ciclo de vida del agente, con métricas declaradas en su especificación, benchmarks estándar y comandos de evaluación continua.

**Qué significa en la práctica:**
- Cada agente declara sus métricas de evaluación en `ASA.yaml` y `03_agent_spec.yaml`: precisión de hipótesis, tasa de falsificación, latencia, consumo de tokens.
- El comando `noctl evaluate` ejecuta la evaluación bajo demanda, con soporte para benchmarks externos (MCP‑AgentBench).
- La evaluación es progresiva: métricas básicas en N1 (tasa de éxito, tiempo de ejecución), métricas de hipótesis en N2 (precisión, falsificación), benchmarks de ecosistema en N3.

**Por qué es necesario:**
Un agente que no se evalúa no puede mejorar. La evaluabilidad integrada cierra el ciclo de aprendizaje: el agente formula hipótesis, las contrasta con la realidad y mide su propio rendimiento. Esto permite la mejora continua sin intervención humana constante.

### Principio 7: Agencia Progresiva

La complejidad se activa solo cuando se necesita. Un agente no nace complejo; evoluciona desde una configuración mínima hasta un orquestador de ecosistemas completo, añadiendo capacidades bajo demanda.

**Qué significa en la práctica:**
- **Modo ultra‑light (N1):** 2 archivos, 5 minutos para arrancar. Sin memoria, sin modelo del mundo, sin seguridad compleja.
- **N1 completo:** 9 archivos. Añade misión, especificación del agente, skills y validación.
- **N2:** 16 archivos. Añade memoria tricameral, modelo del mundo, hipótesis y reflexión.
- **N3:** 30+ archivos. Añade memoria cuádruple, meta‑cognición, verificación neuro‑simbólica, orquestación y gobernanza.
- La CLI `noctl upgrade --level <n>` gestiona la progresión de forma declarativa, añadiendo los artefactos necesarios sin romper la configuración existente.

**Por qué es necesario:**
La agencia progresiva elimina la barrera de entrada. Un desarrollador que solo necesita un agente reactivo para health checks no debería verse obligado a configurar un Grafo Causal Probabilístico o políticas OPA. La complejidad se introduce cuando el ecosistema la requiere, y no antes.

## 3. Estructura de Archivos y Niveles

La materialización más visible de AOF‑S es su estructura de archivos. Un agente AOF‑S no es un código monolítico ni un prompt gigantesco; es un directorio `.aof/` compuesto por artefactos especializados que el agente lee, escribe y actualiza a lo largo de su ciclo de vida. Esta estructura se organiza en tres niveles de complejidad creciente, en cumplimiento del principio de agencia progresiva.

### 3.1 Los Tres Niveles de Complejidad

| Nivel | Nombre | Archivos | Token Inicial | Memoria | Modelo del Mundo | ¿Aprende solo? | ¿Orquesta? |
|-------|--------|----------|---------------|---------|------------------|----------------|------------|
| **1** | **Reactivo** | 2‑9 | ~2‑4k | No (sus evaluaciones alimentan la memoria colectiva) | No | No (cierre sistémico) | No |
| **2** | **Sistémico** | ~16 | ~4‑7k | Episódica + Semántica + Procedural | Variables, estado, Grafo Causal (1º/2º orden) | Sí (localmente) | No |
| **3** | **Orquestado** | 30+ | ~7‑12k | Cuádruple (+ Meta‑cognitiva) | Grafo Causal Probabilístico completo (1º/2º/3º orden) | Sí (local y sistémico) | Sí, el ecosistema |

### 3.2 El Modo Ultra‑Light: Agentes en 5 Minutos

El modo ultra‑light materializa la promesa de la agencia progresiva: un agente funcional en minutos, sin renunciar a la escalabilidad futura. Es el equivalente AOF‑S a un "Hello World" en programación.

**Inicialización:**
```bash
noctl init --level 1 --ultra-light
```

**Estructura resultante:**
```
.aof/
├── 00_manifest.yaml          # Identidad y nivel del agente
└── 01_context_static.md      # Stack, comandos y reglas del proyecto
```

**Artefactos del Nivel 1 (Completo):**
```
.aof/
├── 00_manifest.yaml          # Metadatos del agente (framework, nivel, proyecto)
├── 01_context_static.md      # Información inmutable del proyecto (stack, comandos, reglas)
├── 02_mission.yaml           # Qué necesidad debe resolver el agente y cómo se mide el éxito
├── 03_agent_spec.yaml        # Rol, herramientas, modelo, restricciones y evaluación
├── 04_tasks/
│   └── current_plan.md       # Plan de acción actual
├── 05_skills/                # Habilidades modulares (testing, deployment…)
├── 06_validation_spec.md     # Criterios de aceptación
└── 07_observations.log       # Registro de métricas y eventos
```

**Artefactos Añadidos en el Nivel 2 (Sistémico):**
```
.aof/
├── 04_tasks/
│   ├── hypothesis.md         # Hipótesis de intervención con efectos 1º/2º
│   └── steps.md              # Pasos derivados de la hipótesis
├── 05_world_model/           # Modelo del mundo (variables, estado)
├── 07_memory/                # Memoria episódica y procedural
├── 09_observations/          # Métricas y anomalías estructuradas
└── 10_values/                # Constitución y preferencias
```

**Artefactos Añadidos en el Nivel 3 (Orquestado):**
```
.aof/
├── ASA.yaml                  # Identidad completa del agente noosistémico
├── 04_knowledge/             # Conocimiento del ecosistema
│   ├── world_model/          # Grafo Causal, retardos, estado global
│   ├── expert_corpus/        # Corpus de conocimiento indexado para RAG
│   ├── registry.yaml         # Tejido conectivo del ecosistema
│   └── hypotheses/           # Historial completo de hipótesis
├── 05_skills/                # Skills avanzadas: simulación, mapeo causal, RAG
├── 06_memory/                # Memoria cuádruple (incluye meta‑cognitiva)
└── 08_observations/          # Logs y anomalías con trazabilidad completa
```

### 3.3 Carga Progresiva: El Arte de la Eficiencia

La carga progresiva es el mecanismo que hace viable la estructura AOF‑S. El agente no lee todos los archivos al inicio de su ciclo. Selecciona únicamente los artefactos necesarios para la fase actual, manteniendo el consumo de tokens bajo control.

**Matriz de Carga Progresiva:**

| Fase del Ciclo ASA | Artefactos Cargados | Tokens Aprox. (N3) |
| :--- | :--- | :--- |
| **Arranque** | `00_manifest.yaml`, `01_context_static.md`, `02_mission.yaml`, `ASA.yaml`, `constitution.md` | ~3-5k |
| **Razonar** | `episodic/` (top‑N), `policies.yaml`, `state.md`, `variables.yaml`, `domain_facts.md` | +2-3k |
| **Hipótesis** | `causal_graph.proto`, `delays.yaml`, `expert_corpus/` (RAG), `simulation_skill.md` | +3-5k |
| **Verificar (N3)** | `verification_rules.clp`, contratos semánticos (JSON Schema), políticas OPA | +1-2k |
| **Ejecutar** | `current_steps.md`, skills específicas | variable |
| **Evaluar** | `feedback_log.yaml`, `anomaly_notes.md` | +1k |
| **Reflexionar** | Hipótesis activa, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml` (N3) | +2-3k |

**Ejemplo de traza de tokens para un agente N3 (Orquestado):**
1.  **Arranque:** Carga 5 archivos (3,200 tokens). El agente ya sabe quién es, dónde está y qué debe hacer.
2.  **Razonar:** Recupera 3 episodios similares y 2 políticas procedurales (2,800 tokens adicionales).
3.  **Hipótesis:** Carga el Grafo Causal y ejecuta RAG sobre el corpus experto (4,500 tokens adicionales). Pico total: **10,500 tokens**.
4.  **Verificar:** El motor neuro‑simbólico valida la hipótesis (1,200 tokens adicionales).
5.  **Ejecutar:** El agente actúa con un contexto mínimo del plan de pasos y la skill requerida.
6.  **Evaluar:** Lee las métricas post‑intervención (800 tokens).
7.  **Reflexionar:** Actualiza el modelo y las memorias (2,500 tokens).

El pico máximo de consumo se mantiene en torno a los 12‑16k tokens durante la fase de hipótesis, dejando amplio margen dentro de los 200k tokens de ventana de un LLM moderno. Esto contrasta con el enfoque de cargar un `AGENTS.md` estático de 40KB, que ya consume una porción significativa de la ventana antes de que el agente haya empezado a razonar.

### 3.4 Agencia Progresiva en la Práctica

La transición entre niveles es un proceso declarativo gestionado por `noctl`:

```bash
# De un N1 ultra‑light a un N1 completo
noctl upgrade --level 1

# De un N1 completo a un N2 sistémico (con memoria y modelo del mundo)
noctl upgrade --level 2

# De un N2 a un N3 orquestado (con meta‑cognición y gobernanza)
noctl upgrade --level 3
```

Cada comando añade los artefactos correspondientes al nuevo nivel sin eliminar los existentes ni romper la configuración previa. El agente evoluciona orgánicamente.

## 4. Noosfile y Orquestación Declarativa

AOF‑S no solo define la estructura interna de cada agente. Define también cómo se gobierna el ecosistema completo. El **Noosfile** es el manifiesto declarativo que describe el estado deseado de un (sub)Noosistema, y **noctl** es la herramienta de línea de comandos que aplica ese manifiesto, siguiendo el patrón de Infraestructura como Código (IaC) que popularizaron Docker Compose y Kubernetes.

### 4.1 El Noosfile como Fuente de Verdad

El `Noosfile.yaml` es el equivalente en el Noosistema a un `docker-compose.yml` o a un `kustomization.yaml`: un archivo versionable que declara qué agentes deben existir, con qué configuración, bajo qué políticas de seguridad y con qué criterios de evaluación. No describe *cómo* lograr ese estado, sino *qué* estado es el deseado. El orquestador (un agente N3 o el futuro Operador Noosistémico) se encarga de materializarlo.

**Estructura de un Noosfile:**

```yaml
apiVersion: "noos.cncf.io/v1alpha2"
kind: "Noosystem"
metadata:
  name: "ecommerce-ops"
  namespace: "noos://ecommerce"
spec:
  mission: "Garantizar SLO del checkout (p95 < 500ms, 99.95% uptime)"
  noos_level: 2
  sector: "e-commerce"

  agents:
    - name: "orchestrator"
      level: 3
      scaling: { replicas: 1 }
      resources: { cpu: "1000m", memory: "1Gi" }
    - name: "db-provisioner"
      level: 2
      scaling: { replicas: 1 }

  security:
    agent_identity: "SPIFFE"
    mTLS: true
    policies:
      agent_policies: |
        package noos.authz
        default allow = false
        allow { input.agent.level == 3 }
        allow { input.agent.level == 2; input.action in ["read", "execute"] }

  global_policies:
    trust_threshold: 0.85
    circuit_breaker: { max_failures: 3, window: "5m", cooldown: "60s" }

  evaluation:
    benchmark_suites:
      - name: "MCP-AgentBench"
        endpoint: "https://benchmarks.example.com/mcp-agentbench/v1"
```

Cada sección del Noosfile aborda una dimensión de la gobernanza del ecosistema:

| Sección | Propósito |
|---------|-----------|
| `agents` | Declara los agentes que deben existir, su nivel, escalado y recursos. |
| `security` | Define las políticas de seguridad Zero Trust (SPIFFE, mTLS, OPA). |
| `shared_services` | Declara servicios comunes (base de datos vectorial, message broker, knowledge graph). |
| `global_policies` | Establece umbrales de confianza, circuit breakers y políticas de reinicio. |
| `evaluation` | Configura los benchmarks y métricas de evaluación del ecosistema. |
| `inference` | Define la estrategia de inferencia Local‑First del ecosistema. |

### 4.2 `noctl`: La CLI de Gobierno del Noosistema

`noctl` es la herramienta que materializa la orquestación declarativa. Inspirada en `kubectl`, `docker` y `git`, permite a los operadores humanos y a los propios agentes N3 gobernar el ciclo de vida completo del ecosistema.

**Comandos principales:**

| Comando | Función |
|---------|---------|
| `noctl init` | Inicializa un nuevo Noosistema con un nivel de agencia y configuración sectorial. |
| `noctl apply -f <noosfile>` | Aplica un manifiesto Noosfile al ecosistema, creando, actualizando o eliminando agentes. |
| `noctl upgrade --level <n>` | Evoluciona un agente o ecosistema al siguiente nivel de agencia. |
| `noctl status` | Muestra el estado actual del Noosistema (agentes, trust scores, SLOs). |
| `noctl evaluate` | Ejecuta la evaluación de un agente o del ecosistema completo. |
| `noctl security scan` | Escanea el ecosistema en busca de vulnerabilidades OWASP. |
| `noctl diagnose` | Ejecuta un diagnóstico de salud del ecosistema. |
| `noctl memory compact` | Ejecuta la compresión autónoma de contexto. |

**Ejemplo de flujo de trabajo con `noctl`:**
```bash
# Inicializar un ecosistema para un hospital
noctl init --level 2 --sector hospital

# Aplicar un Noosfile personalizado
noctl apply -f noosfile.yaml

# Verificar el estado
noctl status

# Evaluar el ecosistema
noctl evaluate ecosystem

# Escalar a nivel 3 cuando se necesite orquestación avanzada
noctl upgrade --level 3
```

### 4.3 Orquestación Adaptativa: El Operador Noosistémico

Para ecosistemas de Nivel 3 (Enjambre), AOF‑S prevé la figura del **Operador Noosistémico**: un controlador nativo de Kubernetes que observa el estado del clúster, detecta desviaciones respecto al Noosfile y aplica el estado deseado de forma autónoma, gestionando el ciclo de vida completo de los agentes. Este operador sigue el mismo patrón que los operadores de Kubernetes: observa recursos personalizados (`Noosystem`), compara el estado real con el deseado y ejecuta las acciones necesarias para converger.

El Operador Noosistémico será la implementación definitiva del principio de orquestación adaptativa, permitiendo que un ecosistema de agentes se auto‑gestione, auto‑escale y auto‑repare sin intervención humana constante.

## 5. Capa Semántica NSL

La comunicación entre agentes no es solo un problema de transporte o sintaxis. Dos agentes pueden intercambiar mensajes perfectamente formados y aun así no entenderse porque no comparten el significado de los conceptos. Esta es la brecha semántica que la **Capa Semántica Noosistémica (NSL)** viene a cerrar. La NSL no es un nuevo protocolo de red; es una extensión de valor añadido que opera sobre los protocolos existentes (A2A, MCP) para dotar a los agentes de la capacidad de alinear sus significados, negociar contratos y confiar los unos en los otros de forma programática.

### 5.1 El Problema del Aislamiento Semántico

En un ecosistema abierto, un agente clínico y un agente energético necesitan colaborar para pre‑enfriar un quirófano sin comprometer la seguridad del paciente. Ambos hablan A2A y MCP, por lo que pueden intercambiar tareas. Pero ¿cómo sabe el agente energético que "temperatura de consigna" para el agente clínico significa "18‑20°C en quirófano" y no "22‑24°C en hospitalización"? ¿Cómo sabe el agente clínico que "pre‑enfriamiento" no afectará a los equipos de soporte vital? Sin una capa semántica, estas preguntas no tienen respuesta automatizada.

### 5.2 Componentes de la NSL

#### 5.2.1 Semantic Handshake (Negociación Semántica)

Antes de ejecutar una tarea colaborativa, dos agentes comparan sus perfiles semánticos (declarados en `ASA.yaml`) para verificar que comparten un entendimiento común del dominio.

**Flujo de la negociación:**
1. El agente A (orquestador) envía una solicitud de tarea al agente B (especialista), incluyendo su `semantic_profile`: namespace, ontologías vinculantes y versión.
2. El agente B compara el perfil recibido con el suyo.
3. Si ambos comparten al menos una ontología para el dominio de la tarea, la negociación es exitosa y la tarea se ejecuta con las reglas de esa ontología.
4. Si no hay ontología común, los agentes pueden solicitar intervención humana, usar un servicio de alineación externo o rechazar la tarea con un error semántico.

**Ejemplo (PCI DSS en e‑commerce):**
```
Agente A (orchestrator):
  semantic_profile:
    namespace: "noos://ecommerce"
    ontology_binding: ["PCI_DSS_v4", "noos://ecommerce/checkout-domain"]

Agente B (db-provisioner):
  semantic_profile:
    namespace: "noos://ecommerce"
    ontology_binding: ["PCI_DSS_v4", "noos://ecommerce/database-domain"]

→ Ambos comparten PCI_DSS_v4. Handshake exitoso.
→ B aplica las reglas de PCI_DSS_v4 que A espera.
```

#### 5.2.2 Trust Score Programático

Cada agente en el Noosistema tiene un `trust_score` dinámico (0.0 a 1.0) que refleja su fiabilidad operativa. Este score es calculado por el orquestador N3 y almacenado en el `registry.yaml`.

**Factores de cálculo:**

| Factor | Peso | Descripción |
|--------|------|-------------|
| **Precisión de hipótesis** | 40% | Proporción de hipótesis validadas vs. falsadas. |
| **Fiabilidad de respuestas** | 30% | Tasa de éxito en invocaciones (sin errores ni timeouts). |
| **Cumplimiento de SLAs** | 15% | Adherencia a los tiempos de respuesta declarados. |
| **Feedback de pares** | 15% | Evaluaciones recibidas de otros agentes tras colaboraciones. |

**Umbrales de confianza:**
- **≥ 0.90:** Alta confianza. Autonomía completa.
- **0.70 – 0.89:** Confiable. Supervisión recomendada para acciones críticas.
- **0.50 – 0.69:** En observación. Solo tareas de lectura.
- **< 0.50:** No confiable. Aislado (estado `degraded` o `failed`).

El Trust Score no es una credencial de seguridad (la autenticación la proporciona SPIFFE), sino una métrica de fiabilidad operativa que permite al orquestador enrutar tareas al agente más adecuado y detectar agentes degradados antes de que afecten al ecosistema.

#### 5.2.3 Contratos Semánticos Aprendidos

La NSL permite que los agentes aprendan de negociaciones semánticas pasadas. Un agente N2 o N3 puede almacenar en su memoria semántica los patrones de éxito en negociaciones y reutilizarlos en futuras interacciones.

**Ejemplo de entrada en `learned_contracts.yaml`:**
```yaml
contracts:
  - contract_id: "PCI_DSS_v4_compliance"
    success_rate: 0.96
    preferred_ontology: "PCI_DSS_v4"
    fallback_ontology: "ISO_27001"
    negotiation_strategy: "strict_then_fallback"
    last_negotiated: "2026-06-07T10:00:00Z"
```

#### 5.2.4 Bridge NSL‑OSI

Para garantizar la interoperabilidad con el ecosistema más amplio de estándares semánticos, la NSL incluye un **bridge con OSI (Open Semantic Interchange)** , el estándar impulsado por ThoughtSpot y Snowflake. Este bridge permite que un agente del Noosistema traduzca su perfil semántico a definiciones OSI y viceversa, facilitando la colaboración con agentes externos al ecosistema.

### 5.3 La NSL como Capa de Valor Añadido

La NSL no compite con A2A o MCP; los complementa. Viaja como metadatos en las cabeceras de las solicitudes A2A y en los contextos de las herramientas MCP. No impone un nuevo protocolo ni añade rigidez innecesaria a la capa de transporte. Es, simplemente, la capa que permite que los agentes se entiendan, confíen y aprendan a colaborar mejor.

## 6. Seguridad Zero Trust Declarativa

La seguridad en AOF‑S no se implementa mediante código propietario ni configuraciones oscuras en servidores. Se **declara** en los mismos archivos YAML que definen al agente y al ecosistema. Esta declaratividad es la que permite que la seguridad sea progresiva, auditable y aplicable de forma automática por el orquestador, sin que el desarrollador del agente tenga que escribir lógica de seguridad en su código.

### 6.1 El Manifiesto de Seguridad: La Sección `security` del Noosfile

Toda la postura de seguridad de un (sub)Noosistema se define en la sección `security` del `Noosfile.yaml`. Esta sección actúa como un contrato vinculante que el orquestador N3 hace cumplir.

**Fragmento de un Noosfile con seguridad declarada:**
```yaml
security:
  agent_identity: "SPIFFE"
  mTLS: true
  policies:
    agent_policies: |
      package noos.authz
      default allow = false
      allow { input.agent.level == 3 }
      allow { input.agent.level == 2; input.action in ["read", "execute"] }
  sandbox:
    runtime: "gvisor"
    allow_network_egress: false
    readonly_rootfs: true
  compliance:
    - "OWASP Top 10 for Agentic Applications (2026)"
    - "NIST AI RMF 1.0"
```

Con solo estas líneas, el ecosistema activa:
- **Identidad verificable:** Todos los agentes deben identificarse con SPIFFE.
- **Cifrado de canal:** Toda comunicación entre agentes se realiza con mTLS.
- **Control de acceso granular:** El motor OPA bloquea cualquier acción que no esté explícitamente permitida.
- **Aislamiento de ejecución:** Cada agente se ejecuta en un sandbox gVisor con sistema de archivos de solo lectura y sin acceso a la red externa.
- **Cumplimiento normativo:** Se documenta la alineación con los principales marcos de seguridad.

Este es el poder de la declaratividad: una configuración de pocas líneas, versionable en Git, aplicable con un comando `noctl apply`, y que el orquestador hace cumplir sin necesidad de que cada agente implemente su propia lógica de seguridad.

### 6.2 Cómo AOF‑S Materializa la Seguridad en los Artefactos

| Concepto de Seguridad | Materialización en AOF‑S |
|-----------------------|--------------------------|
| **Identidad del agente** | Campo `security.identity.spiffe_id` en `ASA.yaml`. Cada agente declara su identidad única. |
| **Cifrado en tránsito** | `security.mTLS: true` en el `Noosfile`. La service mesh (Istio) aplica la política sin intervención del agente. |
| **Autorización granular** | `security.policies.agent_policies` en el `Noosfile`, expresadas en Rego (lenguaje de OPA). |
| **Aislamiento** | `security.sandbox` en el `Noosfile`. Define el runtime de sandbox y las restricciones de red y sistema de archivos. |
| **Auditoría** | `compliance.audit.enabled: true` en `ASA.yaml`. Las acciones se registran en el ledger con trazabilidad completa. |
| **Acciones críticas** | `security.human_approval_required_for` en `ASA.yaml`. Lista explícita de operaciones que requieren autorización humana. |

### 6.3 Progresividad de la Seguridad

Fiel al principio de agencia progresiva, AOF‑S no exige el mismo nivel de seguridad a un agente Reactivo que a un Orquestador.

| Nivel | Requisitos de Seguridad |
|-------|------------------------|
| **N1 (Reactivo)** | Seguridad mínima. El agente se ejecuta en un sandbox (gVisor). SPIFFE y mTLS pueden ser gestionados por un sidecar transparente, sin configuración por parte del desarrollador. No se requiere declarar políticas OPA. |
| **N2 (Sistémico)** | Seguridad automática. SPIFFE y mTLS se activan por defecto. Se aplica una política OPA por defecto (leer y ejecutar dentro de su dominio). |
| **N3 (Orquestado)** | Seguridad completa obligatoria. SPIFFE, mTLS y OPA son requeridos. Se aplican políticas granulares. Las acciones críticas requieren aprobación humana explícita. |

Esta progresividad garantiza que un agente Reactivo (modo ultra‑light, 2 archivos) no necesite configurar identidades ni escribir políticas OPA, mientras que un ecosistema empresarial complejo tenga todas las garantías de seguridad Zero Trust.

### 6.4 Defensa contra Envenenamiento de Memoria

AOF‑S protege la memoria colectiva del Noosistema contra escrituras maliciosas. Dado que los agentes N1 publican evaluaciones en la memoria colectiva y los N2/N3 las consumen para aprender, un agente comprometido podría envenenar el conocimiento de todo el ecosistema. Para prevenirlo, AOF‑S implementa una **defensa bayesiana**:

- **Aislamiento arquitectónico:** Las memorias de diferentes agentes y dominios están lógicamente aisladas. Un agente N1 no puede escribir en la memoria semántica de un N2.
- **Puntuación de confianza bayesiana:** Cada operación de escritura en la memoria colectiva es evaluada por el orquestador. Si el `trust_score` del agente emisor es bajo o la operación se desvía significativamente de los patrones históricos, se rechaza.
- **Aprendizaje adaptativo:** El sistema de defensa aprende a clasificar operaciones legítimas vs. maliciosas utilizando un clasificador bayesiano ligero, sin necesidad de llamadas adicionales al LLM.

Esta defensa, inspirada en los mecanismos de SuperLocalMemory, está documentada en los artefactos AOF‑S: la política de aislamiento en el `Noosfile`, el `trust_score` en el `registry.yaml`, y la auditoría en `observations/anomaly_notes.md`.

### 6.5 Alineación con Marcos de Referencia

AOF‑S se alinea explícitamente con los principales marcos de seguridad para aplicaciones de IA, proporcionando un lenguaje común y un marco de auditoría reconocido:

| Marco | Implementación en AOF‑S |
|-------|------------------------|
| **OWASP Top 10 for Agentic Applications (2026)** | Mitigaciones documentadas para los diez riesgos catalogados (inyección de prompts, envenenamiento de datos, Denial of Wallet, agencia excesiva, etc.). Cada riesgo tiene una contramedida específica en los artefactos AOF‑S. |
| **NIST AI RMF 1.0** | El ciclo ASA (Map, Measure, Manage, Govern) implementa los cuatro pilares del marco de gestión de riesgos. Los artefactos AOF‑S (`hypothesis.md`, `observations/`, `constitution.md`) proporcionan la trazabilidad requerida. |
| **ITU‑T Y.4814** | La recomendación sobre control de acceso Zero Trust para plataformas IoT se materializa en la combinación de SPIFFE + OPA + mTLS declarados en el `Noosfile`. |

## 7. Principio Local‑First en AOF‑S

El Principio 0 de AOF‑S establece que la ejecución local es el camino predeterminado para todo agente. Esta sección detalla cómo AOF‑S materializa ese principio en su estructura de archivos, en la configuración declarativa de la inferencia y en los mecanismos de fallback a la nube.

### 7.1 La Sección `inference` en ASA.yaml

Cada agente AOF‑S, a partir del Nivel 2, declara su configuración de inferencia en su manifiesto de identidad (`ASA.yaml`). Esta sección permite especificar qué motores locales están disponibles, qué modelos ejecuta cada uno, con qué prioridad se seleccionan y bajo qué condiciones se permite el fallback a la nube.

**Fragmento de `ASA.yaml` con configuración de inferencia:**
```yaml
inference:
  mode: "local_first"
  local_engines:
    - name: "ollama"
      models: ["llama3.2:3b", "qwen2.5-coder:7b"]
      priority: 1
    - name: "llama.cpp"
      models: ["llama3.1-8b-Q4_K_M.gguf"]
      priority: 2
  resource_limits:
    max_vram_gb: 8
    fallback_to_cloud: false
```

**Campos de la sección `inference`:**

| Campo | Descripción |
|-------|-------------|
| `mode` | Estrategia de inferencia. `local_first` significa que se prefiere la ejecución local sobre la nube. |
| `local_engines` | Lista de motores de inferencia locales disponibles, cada uno con sus modelos asociados y su prioridad. El motor con prioridad 1 se usa por defecto. |
| `resource_limits` | Restricciones de hardware: VRAM máxima que el agente puede consumir. |
| `fallback_to_cloud` | Si es `false`, el agente nunca recurrirá a la nube, garantizando el 100% de soberanía de datos. Si es `true`, se activa bajo condiciones definidas. |

### 7.2 Motores de Inferencia Soportados

AOF‑S es compatible con los tres principales motores de inferencia local del ecosistema de agentes en 2026:

| Motor | Caso de Uso Óptimo | Características Clave |
|-------|-------------------|----------------------|
| **Ollama** | Desarrollo rápido, prototipado, escritorio. | Instalación simple, biblioteca de modelos curada, API REST integrada, soporte multi‑plataforma. |
| **llama.cpp** | Producción en edge, máximo control, CPU. | Portabilidad extrema (compila en C++), control fino de cuantización (GGUF), soporte para CPU y GPU, JSON Schema nativo. |
| **vLLM** | Servidores multi‑usuario, alta concurrencia. | Hasta 19x más rápido que Ollama bajo carga concurrente, PagedAttention para gestión eficiente de VRAM, tool calling nativo. |

### 7.3 Asociación de Motores a Niveles de Agencia

La elección del motor de inferencia no es arbitraria; está guiada por el nivel de agencia y los recursos disponibles:

| Nivel | Hardware Recomendado | Motor Predeterminado | Modelos de Ejemplo |
|-------|---------------------|---------------------|-------------------|
| **N1 (Reactivo)** | CPU, Raspberry Pi 5, laptops. | llama.cpp (CPU) | Llama 3.2 3B (Q4_K_M). |
| **N2 (Sistémico)** | GPU 8‑12 GB VRAM, Apple Silicon 16‑32 GB. | Ollama | Qwen 2.5 Coder 7B, Llama 3.1 8B. |
| **N3 (Orquestado)** | GPU 18 GB+ VRAM, DGX Spark, clúster. | vLLM (bajo carga), Ollama (tareas aisladas) | Llama 3.1 70B, Qwen 2.5 32B. |

### 7.4 Política de Fallback a la Nube

Si el usuario configura `fallback_to_cloud: true`, el agente puede recurrir a la nube bajo condiciones controladas. Esta política de fallback se define en la misma sección `inference`:

```yaml
inference:
  mode: "local_first"
  local_engines:
    - name: "ollama"
      models: ["llama3.2:3b"]
      priority: 1
  fallback_to_cloud: true
  cloud_fallback:
    providers: ["openai", "anthropic"]
    triggers:
      - "local_engine_unavailable"
      - "resource_exhaustion"
      - "task_requires_frontier_model"
```

**Disparadores del fallback:**

| Disparador | Descripción |
|------------|-------------|
| `local_engine_unavailable` | El motor local (Ollama, llama.cpp) está caído por mantenimiento o error. |
| `resource_exhaustion` | La VRAM o la CPU locales están al límite. |
| `task_requires_frontier_model` | La tarea requiere un modelo de última generación (GPT‑4, Claude Opus) que no está disponible localmente. |

Incluso cuando el fallback está activado, el agente intenta primero la ruta local. Solo si esta falla por alguno de los disparadores configurados, se recurre a la nube. Este enfoque garantiza que la nube sea siempre un respaldo, nunca una dependencia.

### 7.5 Beneficios del Principio Local‑First en AOF‑S

| Beneficio | Descripción |
|-----------|-------------|
| **Soberanía de datos** | Los artefactos AOF‑S (`memory/`, `expert_corpus/`, `observations/`) residen en el sistema de archivos local. En modo `local_first` con `fallback_to_cloud: false`, el 0% de los datos abandonan el dispositivo. |
| **Cero costes por token** | En modo local, la ejecución no genera facturación alguna. Esto es crítico para agentes autónomos de larga duración o ecosistemas con muchos agentes. |
| **Operación offline** | Los agentes pueden funcionar sin conexión a internet, esencial para edge computing, entornos hospitalarios o infraestructuras críticas aisladas. |
| **Privacidad por defecto** | El cumplimiento normativo (HIPAA, PCI DSS, GDPR) se simplifica al no transmitir datos a terceros. |
| **Independencia de proveedores** | El usuario decide qué motor de inferencia usa y si quiere activar el respaldo cloud. Sin vendor lock‑in. |

## 8. Configurabilidad Sectorial

Una de las propiedades más potentes de AOF‑S es su capacidad de adaptarse a diferentes sectores sin modificar su arquitectura base. Las cinco capas del Noosistema, los tres niveles de agencia ASA y los protocolos de comunicación permanecen invariantes. Lo que cambia es la **biblioteca de agentes especializados**, las **skills de dominio** y el **corpus de conocimiento experto**. Esta separación entre el núcleo genérico y la configuración sectorial permite que AOF‑S sea un estándar verdaderamente universal.

### 8.1 Principio de Adaptabilidad

AOF‑S sigue el principio de **"arquitectura invariante, configuración variable"**. Esto significa que:

- **El núcleo AOF‑S es genérico:** Los niveles de agencia (Reactivo, Sistémico, Orquestado), el ciclo ASA de 7 fases, la estructura de archivos, la carga progresiva y la pila de protocolos (A2A, MCP, NSL) son idénticos para un hospital, un puerto, un centro de datos o una planta manufacturera.
- **La especialización se logra mediante módulos intercambiables:** Las skills (`05_skills/`), el corpus experto (`expert_corpus/`), los agentes declarados en el `Noosfile` y las preferencias de usuario (`preferences.yaml`) son los únicos elementos que varían entre sectores.
- **La adopción de un nuevo sector no requiere reingeniería:** No hay que reescribir el motor de orquestación, ni el ciclo ASA, ni la capa semántica. Basta con configurar los módulos sectoriales adecuados.

### 8.2 Mecanismo de Configuración Declarativa

AOF‑S proporciona un comando específico en su CLI para inicializar un ecosistema sectorial:

```bash
# Inicializar un Noosistema para un hospital
noctl sector init --sector hospital --level 2

# Inicializar un Noosistema para un centro de datos
noctl sector init --sector datacenter --level 3
```

Este comando realiza tres acciones:

1. **Copia la plantilla AOF‑S** correspondiente al nivel de agencia solicitado (Reactivo, Sistémico u Orquestado).
2. **Añade las skills sectoriales** en `05_skills/`. Por ejemplo, `clinical_agent_skill.md` y `energy_optimization_skill.md` para un hospital; `cooling_optimization_skill.md` para un centro de datos.
3. **Genera un `Noosfile.yaml` pre‑configurado** con los agentes especializados del sector, sus niveles, recursos y políticas de seguridad.

El resultado es un ecosistema listo para desplegar, con agentes que entienden el dominio, skills que encapsulan la lógica sectorial y un corpus experto que proporciona conocimiento de referencia (normativas, manuales, guías de operación).

### 8.3 Mapeo de Sectores a Configuraciones AOF‑S

| Sector | Skills Sectoriales (Ejemplos) | Agentes N2 Especializados | Documentos del Corpus Experto |
| :--- | :--- | :--- | :--- |
| **Hospital** | Monitorización de signos vitales, optimización energética hospitalaria, auditoría HIPAA. | Agente clínico, agente energético, agente de seguridad. | Protocolos clínicos, normativas HIPAA, guías de eficiencia energética en hospitales. |
| **Centro de Datos** | Optimización de refrigeración, balanceo predictivo de carga, escaneo de vulnerabilidades. | Agente de refrigeración, balanceador de carga, agente de seguridad. | Especificaciones de PUE, normas ISO 27001, guías de diseño de centros de datos. |
| **Puerto** | Planificación logística, optimización de grúas, seguridad portuaria. | Agente logístico, agente de grúas, agente de seguridad perimetral. | Regulaciones portuarias internacionales, manuales de operación de grúas. |
| **Manufactura** | Mantenimiento predictivo, control de calidad, optimización de línea de producción. | Agente de mantenimiento, agente de calidad, agente de producción. | Manuales de maquinaria, normativas ISO 9001, guías de lean manufacturing. |
| **Aeropuerto** | Gestión de flujos de pasajeros, asignación de puertas, eficiencia energética. | Agente de flujos, agente de puertas, agente energético. | Regulaciones de aviación civil, guías de operación aeroportuaria. |

### 8.4 Ejemplo: Configuración de un Hospital Inteligente con AOF‑S

Para un hospital de 300 camas con un consumo eléctrico medio de 4.2 MW, AOF‑S genera un ecosistema con los siguientes agentes, todos operando sobre las mismas cinco capas del Noosistema:

- **C1 (Campo) y C3 (Edge):** Agentes N1 como `vital-signs-monitor` (monitor de signos vitales) y `hvac-local-controller` (controlador local de climatización). Estos agentes son ligeros, se ejecutan en gateways IoT o servidores edge, y publican telemetría en el bus de eventos.
- **C4 (Cloud):** Agentes N2 como `clinical-agent` (monitorización clínica), `energy-agent` (optimización energética), `security-agent` (escaneo de vulnerabilidades y auditoría HIPAA) y `logistics-agent` (gestión de camas y suministros). Estos agentes ejecutan el ciclo ASA completo: formulan hipótesis, verifican contra políticas, ejecutan y reflexionan.
- **C5 (Orquestación):** Un agente N3 `hospital-orchestrator` coordina a todos los N2 y N1, mantiene el Grafo Causal global del hospital, simula escenarios contrafactuales y vela por el cumplimiento de los SLOs clínicos y energéticos.

El `Noosfile` declara estos agentes, sus recursos y las políticas de seguridad específicas del sector (HIPAA, ENS). Las skills sectoriales contienen la lógica de dominio, pero se ejecutan sobre el mismo ciclo ASA genérico. El corpus experto incluye documentos como "Guía de eficiencia energética en hospitales" y "Normativa HIPAA para datos clínicos", indexados en la base de datos vectorial local para RAG.

### 8.5 La Adaptabilidad como Ventaja Evolutiva

La configurabilidad sectorial de AOF‑S proporciona varias ventajas:

- **Replicabilidad:** Un hospital que ha validado el ecosistema puede compartir su configuración con otro hospital, que solo necesita ajustar los parámetros de su infraestructura (número de camas, potencia fotovoltaica, etc.).
- **Escalabilidad:** Un ecosistema que comienza con dos agentes N1 y un N2 puede evolucionar a un enjambre de 20 agentes sobre las mismas capas, simplemente añadiendo nuevos agentes al `Noosfile`.
- **Compartición de conocimiento entre sectores:** Las reglas semánticas aprendidas por un `energy-agent` en un hospital pueden ser transferidas a un `energy-agent` en un centro de datos, adaptando solo los umbrales y las restricciones. AOF‑S facilita esta transferencia gracias a su formato estandarizado de reglas (`learned_rules.yaml`).

## 9. Relación con ASA y el Noosistema

AOF‑S no es una entidad independiente. Es el tercer pilar del triángulo arquitectónico que, junto con el Modelo ASA y el Noosistema, proporciona un marco completo para agentes de IA. Esta sección explica cómo se relacionan estos tres pilares, por qué cada uno es necesario y cómo AOF‑S actúa como el puente que convierte la teoría en un estándar implementable y un ecosistema gobernable.

### 9.1 El Triángulo Arquitectónico

| Pilar | ¿Qué es? | ¿Qué problema resuelve? |
|-------|----------|------------------------|
| **Noosistema** | El ecosistema cognitivo digital estratificado. | Define el escenario: dónde operan los agentes, cómo colaboran, cómo se gobiernan y cómo aprenden colectivamente. |
| **ASA** | La arquitectura del Agente Sistémico Adaptativo. | Define el agente: cómo razona (ciclo de 7 fases), cómo recuerda (memoria cuádruple), cómo aprende (reflexión continua) y cómo se autorregula (gobernanza algorítmica). |
| **AOF‑S** | El estándar abierto de archivos versionables. | Define el empaquetado, la orquestación y la gobernanza: cómo se materializan los artefactos cognitivos, cómo se gobierna el ecosistema y cómo se garantiza la interoperabilidad. |

**Analogía:** Si el Noosistema fuera una ciudad inteligente, ASA sería el diseño de los vehículos autónomos que circulan por ella, y AOF‑S sería el estándar que define cómo se fabrican, se comunican y se regulan esos vehículos.

### 9.2 AOF‑S como Materialización de ASA

ASA es una arquitectura abstracta. Define *qué* debe hacer un agente, pero no *cómo* debe implementarse. AOF‑S es la respuesta a ese "cómo". Cada concepto de ASA tiene una correspondencia directa en un artefacto AOF‑S:

| Concepto ASA | Materialización en AOF‑S |
|--------------|--------------------------|
| **Principio de Intervención Reflexiva Causal** | Ciclo de 7 fases documentado en `spec/asa.yaml`. El agente lee y escribe `hypothesis.md`, `observations/` y `policies.yaml`. |
| **Tres niveles de agencia** | Plantillas `templates/reactive/`, `templates/systemic/`, `templates/orchestrated/`. |
| **Ciclo de 7 fases** | Matriz de carga progresiva en `spec/aof-s.yaml`. Cada fase carga un subconjunto de artefactos. |
| **Memoria cuádruple** | Directorios `06_memory/episodic/`, `06_memory/semantic/`, `06_memory/procedural/` y archivo `meta_strategies.yaml`. |
| **Grafo Causal Probabilístico** | `causal_graph.proto` con nodos, aristas y distribuciones de probabilidad. |
| **Verificación neuro‑simbólica** | Motor de reglas CLIPS/Drools configurado en `ASA.yaml`. |
| **Seguridad Zero Trust** | Sección `security` en `Noosfile.yaml`. Identidades SPIFFE, mTLS y políticas OPA. |
| **Principio Local‑First** | Sección `inference` en `ASA.yaml`. |

Sin AOF‑S, ASA sería una especulación teórica. Con AOF‑S, es un estándar que cualquier LLM puede adoptar, cualquier desarrollador puede personalizar y cualquier organización puede auditar.

### 9.3 AOF‑S como Orquestador del Noosistema

AOF‑S no solo materializa a los agentes individuales; también proporciona las herramientas para gobernar el ecosistema completo:

- **Noosfile.yaml:** Es el manifiesto declarativo que describe el estado deseado del (sub)Noosistema. Inspirado en Kubernetes, permite declarar qué agentes deben existir, con qué nivel, cuántas réplicas y bajo qué políticas de seguridad.
- **noctl:** Es la CLI que aplica el `Noosfile` (similar a `kubectl apply`), permitiendo inicializar, escalar, evaluar y diagnosticar el ecosistema.
- **registry.yaml:** Es el tejido conectivo que mantiene el mapa vivo de agentes, sus capacidades, trust scores y topología de conocimiento.
- **La NSL (Capa Semántica Noosistémica):** Permite que los agentes se entiendan, negocien significados y confíen unos en otros, cerrando la brecha semántica que A2A y MCP no resuelven por sí solos.

Estos cuatro componentes —Noosfile, noctl, registry y NSL— convierten al Noosistema de un concepto abstracto en un ecosistema gobernable, auditable y escalable.

### 9.4 El Flujo Completo

1. **El Noosistema** define el ecosistema donde los agentes colaboran: sus capas de despliegue (C1 a C5), su memoria colectiva y sus políticas de gobernanza.

2. **Cada agente ASA** opera dentro de ese ecosistema siguiendo el ciclo de 7 fases. Arranca cargando sus artefactos AOF‑S, razona con la estrategia más adecuada, formula hipótesis causales, las verifica (si es N3), ejecuta monitorizado, evalúa los resultados y reflexiona para actualizar su modelo del mundo y sus memorias.

3. **Los artefactos AOF‑S** son el lenguaje común que los agentes leen y escriben. Son versionables, auditables y portables. Cualquier LLM que entienda Markdown + YAML puede operar como un agente ASA.

4. **El runtime NooKepler** (en desarrollo) actuará como el harness que ejecuta el ciclo ASA, gestiona la carga progresiva de contexto y proporciona el entorno de ejecución seguro.

El triángulo está completo. ASA proporciona la inteligencia del agente. El Noosistema proporciona el ecosistema donde esa inteligencia se despliega. Y AOF‑S proporciona el estándar que hace posible que ambos existan en la práctica.

---

## Conclusión del Documento Conceptual de AOF‑S

AOF‑S no es otro formato de configuración. Es el estándar que cierra el triángulo del Noosistema, materializando la arquitectura ASA en archivos versionables y proporcionando las herramientas para gobernar ecosistemas de agentes de forma declarativa, segura y escalable.

A lo largo de este documento, hemos visto cómo AOF‑S:

1. **Resuelve la fragmentación** del ecosistema de agentes con un estándar abierto, Spec‑Driven y Local‑First que cualquier LLM puede adoptar.

2. **Implementa los ocho principios de diseño** (0‑7) que garantizan la soberanía de datos, la eficiencia de tokens, la trazabilidad de las decisiones y la progresividad de la complejidad.

3. **Define una estructura de archivos vivos** que el agente lee y escribe a lo largo de su ciclo de vida, permitiendo el aprendizaje continuo y la auditoría completa.

4. **Proporciona orquestación declarativa** mediante el `Noosfile` y la CLI `noctl`, siguiendo el patrón de Infraestructura como Código que ha demostrado su eficacia en Kubernetes.

5. **Incorpora una capa semántica (NSL)** que resuelve el problema del aislamiento semántico, permitiendo que los agentes se entiendan, negocien y confíen.

6. **Integra seguridad Zero Trust** de forma declarativa, progresiva y alineada con los principales marcos de referencia (OWASP, NIST).

7. **Garantiza el Principio Local‑First** mediante una sección de configuración de inferencia que permite a cada agente declarar sus motores locales y sus políticas de fallback a la nube.

8. **Se adapta a cualquier sector** (hospitales, puertos, centros de datos, manufactura) sin modificar su núcleo, cambiando solo skills, corpus experto y configuración.

AOF‑S es, en definitiva, el sistema operativo del agente. Con ASA como mente y el Noosistema como mundo, AOF‑S proporciona el lenguaje, las herramientas y las reglas para que los agentes de IA aprendan, razonen y colaboren.

---
**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek (DeepThink) como asistente de diseño sistémico.  
**Parte del estándar:** AOF‑S v1.0  
**Ubicación:** `docs/aofs-conceptual.md` en el repositorio público.