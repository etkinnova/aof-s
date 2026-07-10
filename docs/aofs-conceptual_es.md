---

# AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual

**Versión:** 3.3  
**Fecha:** 6 de julio de 2026  
**Parte del estándar:** AOF‑S v1.4  
**Referencia normativa:** `spec/aof-s.yaml` (prevalece en caso de discrepancia)  
**Estado:** Canónico

---

## Tabla de Contenidos

- [Tabla de Contenidos](#tabla-de-contenidos)
- [1. Definición Canónica Multidimensional](#1-definición-canónica-multidimensional)
  - [1.1 Dimensión: Estándar de Configuración (El "Qué" / Formato)](#11-dimensión-estándar-de-configuración-el-qué--formato)
  - [1.2 Dimensión: Marco Arquitectónico o "Harness" (El "Dónde" / Estructura)](#12-dimensión-marco-arquitectónico-o-harness-el-dónde--estructura)
  - [1.3 Dimensión: Artefacto Fundamental de Spec‑Driven Development (El "Por qué" / Metodología)](#13-dimensión-artefacto-fundamental-de-specdriven-development-el-por-qué--metodología)
  - [1.4 Dimensión: Descriptor de Ecosistema y Portabilidad (El "Quién" / Gobernanza)](#14-dimensión-descriptor-de-ecosistema-y-portabilidad-el-quién--gobernanza)
- [2. Qué NO es AOF‑S](#2-qué-no-es-aofs)
- [3. Propósito y Alcance](#3-propósito-y-alcance)
- [4. Principios de Diseño](#4-principios-de-diseño)
- [5. Niveles de Agencia y Estructura de Archivos](#5-niveles-de-agencia-y-estructura-de-archivos)
  - [N1 — Reactivo (8 artefactos, ultra‑ligero: 3)](#n1--reactivo-8-artefactos-ultraligero-3)
  - [N2 — Sistémico (16 artefactos)](#n2--sistémico-16-artefactos)
  - [N3 — Orquestado (30+ artefactos)](#n3--orquestado-30-artefactos)
  - [5.1 Formato de los artefactos según su propósito cognitivo](#51-formato-de-los-artefactos-según-su-propósito-cognitivo)
- [6. La Skill como Unidad de Capacidad](#6-la-skill-como-unidad-de-capacidad)
  - [6.1 Estructura del SKILL.md](#61-estructura-del-skillmd)
  - [6.2 Tipos de herramientas](#62-tipos-de-herramientas)
  - [6.3 Modos de LLM](#63-modos-de-llm)
  - [6.4 Modo `cached` y aprendizaje progresivo](#64-modo-cached-y-aprendizaje-progresivo)
- [7. Manifiesto y Especificación del Agente](#7-manifiesto-y-especificación-del-agente)
  - [7.1 `00_manifest.yaml`](#71-00_manifestyaml)
  - [7.2 `02_agent_spec.yaml`](#72-02_agent_specyaml)
- [8. Paquetes AOF: Reutilización y Repositorios](#8-paquetes-aof-reutilización-y-repositorios)
  - [8.1 Estructura de un paquete](#81-estructura-de-un-paquete)
  - [8.2 Políticas de conflicto](#82-políticas-de-conflicto)
- [9. Gestión Segura de Secretos](#9-gestión-segura-de-secretos)
  - [9.1 Sintaxis de referencia](#91-sintaxis-de-referencia)
  - [9.2 Resolución por perfil](#92-resolución-por-perfil)
  - [9.3 Auditoría](#93-auditoría)
- [10. Carga Progresiva y Puerta de Evidencia](#10-carga-progresiva-y-puerta-de-evidencia)
  - [Matriz de carga por fase (N3)](#matriz-de-carga-por-fase-n3)
  - [Puerta de Evidencia](#puerta-de-evidencia)
- [11. Perfiles de Despliegue](#11-perfiles-de-despliegue)
- [12. Comunicación y Protocolos](#12-comunicación-y-protocolos)
  - [Versioned Capability Vectors (VCVs)](#versioned-capability-vectors-vcvs)
- [13. Registro de Agentes y Descubrimiento](#13-registro-de-agentes-y-descubrimiento)
- [14. Seguridad y Gobernanza Declarativa](#14-seguridad-y-gobernanza-declarativa)
- [15. Inferencia Local‑First](#15-inferencia-localfirst)
- [16. Evaluación Integrada](#16-evaluación-integrada)
- [17. Generación Automática de Ecosistemas](#17-generación-automática-de-ecosistemas)
- [18. Configurabilidad Sectorial](#18-configurabilidad-sectorial)
- [19. Relación con ASA, Noosistema y el Runtime](#19-relación-con-asa-noosistema-y-el-runtime)
- [20. Evolución del Estándar](#20-evolución-del-estándar)
- [21. Directorio Central del Ecosistema](#21-directorio-central-del-ecosistema)
  - [21.1 Estructura](#211-estructura)
  - [21.2 Propósito de cada subdirectorio](#212-propósito-de-cada-subdirectorio)
  - [21.3 Variable de entorno `NOOSYSTEM_PATH`](#213-variable-de-entorno-noosystem_path)
- [Si la variable `NOOSYSTEM_PATH` está definida, el runtime usará esa ruta como raíz del directorio central. Si no, usará `~/.noosystem/`. Si la variable está definida pero el directorio no existe, el runtime debe crearlo automáticamente con la estructura de subdirectorios esperada.](#si-la-variable-noosystem_path-está-definida-el-runtime-usará-esa-ruta-como-raíz-del-directorio-central-si-no-usará-noosystem-si-la-variable-está-definida-pero-el-directorio-no-existe-el-runtime-debe-crearlo-automáticamente-con-la-estructura-de-subdirectorios-esperada)


---

## 1. Definición Canónica Multidimensional

**AOF‑S (AI Orchestration Files for Systemic Agents) es un estándar de configuración estructural que materializa un marco arquitectónico (Harness Engineering) y sirve como el artefacto físico fundamental para la aplicación del Spec‑Driven Development (SDD) en ecosistemas de agentes.**

Esta definición se desglosa en cuatro dimensiones constitutivas:

### 1.1 Dimensión: Estándar de Configuración (El "Qué" / Formato)

En su capa más básica, AOF‑S es un **estándar abierto de serialización y esquema** (análogo a OpenAPI para REST o a un Dockerfile para contenedores).

- **Función**: Define una taxonomía estricta de archivos YAML/Markdown (manifiesto, especificación, hipótesis, memoria) y una estructura de directorios (`.aof/`).
- **Propósito**: Estandarizar *cómo se escribe* la identidad, el conocimiento y las reglas de un agente, garantizando que sea legible tanto por humanos como por cualquier LLM o runtime, independientemente del proveedor.
- **Valor diferencial**: Introduce el principio de **Agencia Progresiva** (Niveles 1, 2 y 3) dentro del propio estándar, permitiendo escalar la complejidad de la configuración según la madurez del agente.

### 1.2 Dimensión: Marco Arquitectónico o "Harness" (El "Dónde" / Estructura)

AOF‑S no es un simple archivo de configuración; es la **materialización física del Harness Engineering** (el "sistema operativo" del agente).

- **Función**: Actúa como el contenedor estructural que organiza los **Guides (feedforward)** y los **Sensors (feedback)** del agente.
- **Evidencia**:
  - Los artefactos `10_values/` y `03_mission.yaml` son los *Guides* estratégicos.
  - Los artefactos `04_hypotheses/` y `09_observations/` son los *Sensors* que permiten la reflexión y el aprendizaje.
  - Los directorios `07_memory/` (episódica, semántica) son el estado persistente del harness.
- **Valor diferencial**: Almacena el **ciclo reflexivo** (hipótesis → verificación → reflexión) directamente en el sistema de archivos, convirtiendo el harness en un ente que "aprende" y persiste su experiencia, algo que los harnesses tradicionales no hacen.

### 1.3 Dimensión: Artefacto Fundamental de Spec‑Driven Development (El "Por qué" / Metodología)

AOF‑S es la **encarnación física del SDD** aplicado a agentes. No es solo una técnica de SDD; es el **sistema de archivos donde viven las especificaciones**.

- **Función**: Mientras que el SDD es la *metodología* (escribe la spec antes de actuar), AOF‑S es el *repositorio de specs* que el agente lee y actualiza durante su ejecución.
- **Evidencia**:
  - El directorio `04_tasks/` es la especificación descompuesta del trabajo a realizar.
  - El agente no "adivina" qué hacer basándose en un prompt; **lee la spec** (los archivos YAML) y ejecuta contra ella.
  - La especificación (`values` e `hypotheses`) **evoluciona** con el tiempo gracias a la reflexión del agente, cerrando el bucle SDD de manera dinámica.
- **Valor diferencial**: A diferencia de herramientas SDD que generan código, AOF‑S genera y actualiza **estado y conocimiento del agente**, transformando el SDD de un proceso de generación de código a un proceso de evolución cognitiva.

### 1.4 Dimensión: Descriptor de Ecosistema y Portabilidad (El "Quién" / Gobernanza)

Un directorio `.aof/` contiene toda la información necesaria para que un agente sea desplegado, gobernado y orquestado en cualquier entorno (standalone, edge o cloud) sin perder su identidad o memoria.

- **Función**: Actúa como el **"pasaporte" de un agente dentro de un ecosistema**.
- **Propósito**: Materializa el **contrato de intenciones** del agente con el ecosistema. Al ser `Local‑First`, garantiza que el agente pueda operar offline y sincronizarse cuando vuelva a estar en línea.

---

## 2. Qué NO es AOF‑S

| Concepto | ¿Es AOF‑S? | Justificación |
|----------|------------|---------------|
| **Runtime / Motor de ejecución** | No | No ejecuta código ni contiene un loop de inferencia; solo define la configuración que un runtime (como ASA Agent) interpreta. |
| **Framework de orquestación** | No | No proporciona herramientas de programación para construir agentes; es puramente declarativo (YAML/Markdown). |
| **Técnica de prompting** | No | No es un conjunto de instrucciones para el prompt; es una estructura de sistema de archivos que el agente consulta. |
| **Lenguaje de programación** | No | No es Turing completo; es un lenguaje de marcado para la cognición y las restricciones del agente. |
| **Gestor de secretos** | No | Los secretos nunca se almacenan en los artefactos. Se usan referencias (`${env:...}` o `${secret:...}`) que el runtime resuelve externamente. |

---

## 3. Propósito y Alcance

AOF‑S define el formato de empaquetado, la estructura de archivos y las convenciones de configuración para agentes de IA dentro del ecosistema Noosistema. Especifica **qué** artefactos componen un agente y **cómo** se organizan; no prescribe el comportamiento interno del agente (ámbito de la arquitectura ASA) ni la topología del ecosistema (ámbito del Noosistema).

La versión 1.4 incorpora: plantillas para generación automática de ecosistemas, compatibilidad total con el formato de skills comunitario (agentskills.io), Paquetes AOF para reutilización modular, gestión declarativa de secretos y validación nativa mediante el N3.

---

## 4. Principios de Diseño

| ID | Principio | Descripción |
|----|-----------|-------------|
| 0 | **Local‑First by Design** | Los artefactos residen en el sistema de archivos local. La nube es un respaldo opcional explícitamente configurado. |
| 1 | **Cognitive Separation** | Cada archivo responde a una pregunta concreta: identidad, misión, contexto, memoria, observaciones, valores. |
| 2 | **Progressive Context Loading** | El agente solo carga los artefactos necesarios para la fase actual, manteniendo el consumo entre 2k y 12k tokens. |
| 3 | **Reflexive Causal Intervention** | Toda acción requiere una hipótesis registrada con efectos previstos; la realidad se contrasta y el modelo se actualiza. |
| 4 | **Purposeful Cognitive Memory** | Memoria organizada en tipos (episódica, semántica, procedural, meta‑cognitiva) con olvido y fortalecimiento. |
| 5 | **Zero Trust Security by Construction** | SPIFFE, mTLS y OPA integrados desde el diseño; requisitos progresivos por nivel. |
| 6 | **Integrated Evaluability** | Cada agente declara métricas y benchmarks; la evaluación es un comando nativo del ciclo de vida. |
| 7 | **Progressive Agency** | La complejidad se activa por nivel. El agente evoluciona desde 3 archivos (N1 ultra‑ligero) hasta 30+ (N3) mediante `noctl upgrade`. |

---

## 5. Niveles de Agencia y Estructura de Archivos

AOF‑S define tres niveles de agencia progresiva. Cada nivel especifica los artefactos requeridos (`artifacts`) y los mínimos imprescindibles (`requires`), usados por el validador nativo `validate_aof`. La numeración sigue el esquema de rangos fijos por dominio funcional:

| Rango | Dominio |
|:---|:---|
| `00` | Identidad |
| `01` | Contexto estático |
| `02` | Especificación del agente |
| `03` | Misión |
| `04` | Tareas y planificación |
| `05` | Skills (habilidades) |
| `06` | Modelo del mundo |
| `07` | Memoria |
| `08` | Validación |
| `09` | Observaciones |
| `10` | Valores y gobernanza |
| `11` | Identidad noosistémica (solo N3) |

### N1 — Reactivo (8 artefactos, ultra‑ligero: 3)

```
.aof/
├── 00_manifest.yaml
├── 01_context_static.md
├── 02_agent_spec.yaml
├── 03_mission.yaml
├── 04_tasks/current_plan.md
├── 05_skills/
│   └── testing_skill/
│       ├── SKILL.md
│       └── scripts/...
├── 08_validation_spec.md
└── 09_observations.log
```

El modo ultra‑ligero (ESP32, RPi) omite todos los archivos excepto `00_manifest.yaml`, `01_context_static.md` y `02_agent_spec.yaml`. No requiere LLM.

### N2 — Sistémico (16 artefactos)

Añade modelo del mundo (`06_world_model/`), memoria tricameral (`07_memory/`), hipótesis (`04_tasks/hypothesis.md`) y constitución (`10_values/constitution.md`).

### N3 — Orquestado (30+ artefactos)

Añade grafo causal, registro de agentes, corpus experto y modos de operación.

**Modos del N3:**
| Modo | Fases | Contexto |
|------|-------|----------|
| **Completo** | 7 | Simulación causal, verificación neuro‑simbólica, orquestación global. |
| **Degradado** | 5 | Operación en edge sin simulación ni verificación. |
| **Delegando** | 4 | Enruta tareas a N2/N1 especializados. |

### 5.1 Formato de los artefactos según su propósito cognitivo

Cada artefacto AOF‑S tiene un formato específico determinado por su función cognitiva. Esta clasificación es normativa y el runtime debe respetarla sin aplicar heurísticas de detección automática.

| Formato | Descripción | Artefactos |
|---------|-------------|------------|
| **YAML puro** (`.yaml`) | Datos estructurados que el runtime parsea directamente. No contienen texto libre ni instrucciones para humanos. | `00_manifest.yaml`, `02_agent_spec.yaml`, `03_mission.yaml`, `variables.yaml`, `registry.yaml`, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml`, `feedback*.yaml`, `preferences.yaml` |
| **Markdown puro** (`.md`) | Texto libre para humanos o narrativa para el agente. El runtime los lee como texto; no extrae datos estructurados. | `01_context_static.md`, `current_plan.md`, `hypothesis.md`, `steps.md`, `state.md`, `current_steps.md`, `domain_facts.md`, `validation_spec.md`, `anomalies*.md`, `constitution.md` |
| **Markdown con frontmatter YAML** (`.md`) | Bloque YAML delimitado por `---` seguido de instrucciones en Markdown. Es el **único** formato que utiliza frontmatter. | `05_skills/*/SKILL.md` |

**Notas importantes:**
- `current_steps.md` es Markdown puro. El N3 delega un **objetivo** en lenguaje natural, no una lista de herramientas. El agente hijo decide cómo ejecutarlo según sus propias skills.
- `hypothesis.md` es Markdown puro. El LLM genera un texto semiestructurado que el runtime analiza con patrones simples durante la reflexión.
- `preferences.yaml` es YAML, pero admite contenido flexible con claves y valores definidos por el usuario.
- El runtime **no debe intentar detectar automáticamente** el formato de un artefacto (por ejemplo, comprobando si empieza por `---`). Cada artefacto se parsea según el formato declarado en esta clasificación.
---

## 6. La Skill como Unidad de Capacidad

Desde la v1.3, cada skill es un **subdirectorio** dentro de `05_skills/`, cumpliendo con el estándar **agentskills.io**. Esto permite que skills creadas para AOF‑S se usen en otros ecosistemas y viceversa.

```
05_skills/
└── health_check/
    ├── SKILL.md           # Metadatos YAML + instrucciones
    ├── scripts/           # Código ejecutable (opcional)
    ├── references/        # Documentación (opcional)
    └── assets/            # Plantillas (opcional)
```

### 6.1 Estructura del SKILL.md

```markdown
---
name: health_check
description: "Verifica estado de endpoints HTTP"
version: "1.0.0"

tools:
  - name: http_health
    description: "Realiza una petición HTTP a un endpoint"
    mcp_server:
      name: monitoring-tools
    function:
      parameters:
        type: object
        properties:
          url:
            type: string
        required: ["url"]
    defaults:
      method: "GET"
      timeout_ms: 5000
    auth:
      api_key: ${env:MONITORING_API_KEY}

llm:
  mode: "on_error"
---
# Skill: health_check
Realiza una petición HTTP a cada endpoint y registra el resultado.
```

### 6.2 Tipos de herramientas

| Tipo | Campo YAML | Descripción |
|------|------------|-------------|
| **MCP** | `mcp_server` | Herramienta proporcionada por un servidor Model Context Protocol. La skill declara el nombre del servidor; el runtime se conecta a él. |
| **Nativa** | `native` | Herramienta integrada en el runtime (`deploy_agent`, `validate_aof`, `generate_workspace`). Se identifica mediante el campo `action`. |
| **Función** | `function` | Herramienta definida con JSON Schema, compatible con el tool calling de OpenAI y Anthropic. Portable a cualquier plataforma. |

### 6.3 Modos de LLM

| Modo | Comportamiento | Cuándo usarlo |
|------|----------------|---------------|
| `never` | Ejecución determinista sin LLM. | Health checks, tareas sin ambigüedad. |
| `always` | Consulta al LLM antes de cada ejecución. | Tareas que requieren análisis contextual. |
| `on_error` | Solo consulta si el ciclo anterior tuvo errores. | Monitorización con diagnóstico bajo demanda. |
| `on_change` | Consulta si cambian la misión, el contexto o los endpoints. | Agentes adaptativos de bajo consumo. |
| `cached` | Busca primero en memoria semántica una respuesta de alta confianza. Si existe, la aplica sin consultar al LLM. Si no, consulta y almacena. | FAQs, diagnósticos recurrentes, hipótesis ya validadas. |

---
### 6.4 Modo `cached` y aprendizaje progresivo

El modo `cached` cierra el ciclo de aprendizaje autónomo. Cuando una situación se repite y la respuesta ha sido validada con éxito reiteradamente, el agente deja de consumir tokens para esa tarea. La regla semántica incluye un campo `match_pattern` que permite identificar la situación, y `cached_response` con la acción a ejecutar. Si la confianza de la regla supera 0.9, puede promoverse automáticamente a política procedural (Sistema 1), reduciendo el consumo a cero incluso en la fase de razonamiento.

## 7. Manifiesto y Especificación del Agente

### 7.1 `00_manifest.yaml`

El manifiesto declara la identidad del agente y su perfil de despliegue:

```yaml
agent:
  name: "db-provisioner"
  level: 2
  version: "1.0.0"
  description: "Optimiza y mantiene bases de datos"
  metadata:
    project: "ecommerce-ops"
    instance: "db-001"
    created: "2026-07-06"
  profile: "standalone"
  active_skills:
    - diagnostic_skill
  imports:
    - path: "../packages/health_check"
      include: ["05_skills"]
      conflict_policy: "merge"
  inference:
    mode: "local_first"
    engines:
      - ollama
    fallback_to_cloud: false
```

- **`profile`**: `standalone`, `edge` o `enterprise`. Determina el backend de secretos y las dependencias de infraestructura.
- **`imports`**: Lista de Paquetes AOF externos a incorporar (ver sección 8).
- **`active_skills`**: Lista de skills activas. Si no se especifica, se consideran activas todas las del directorio.

### 7.2 `02_agent_spec.yaml`

Define el rol, las capacidades (VCVs) y las restricciones operativas del agente:

```yaml
role: "database specialist"
vcv:
  - "database_indexing:v2"
  - "migration_planning:v1"
allowed_tools:
  - analyze_indexes
  - execute_migration
constraints:
  max_parallel_actions: 3
  timeout_per_request_ms: 60000
inference:
  mode: "local_first"
  engines:
    - name: "ollama"
      models: ["llama3.2:latest"]
security_overrides:
  policies: |
    package noos.authz
    allow { input.action in ["read", "execute"] }
communication:
  protocol: "A2A"
  channels:
    - type: "request_response"
      endpoint: "/a2a/execute"
      timeout_sec: 30
      retries: 3
```

- **`allowed_tools`**: Lista blanca de herramientas (mínimo privilegio).
- **`constraints`**: Límites operativos.
- **`security_overrides`**: Políticas OPA específicas del agente (si difieren de las globales del Noosfile).
- **`communication`**: Configuración A2A por agente (tiempos de espera, reintentos).

---

## 8. Paquetes AOF: Reutilización y Repositorios

Un **Paquete AOF** es un directorio autocontenido que agrupa un conjunto de artefactos AOF‑S diseñados para ser reutilizados por múltiples agentes. Se almacenan fuera del `.aof/` del agente y se referencian mediante el campo `imports` en el manifiesto.

### 8.1 Estructura de un paquete

```
packages/
├── health_check/
│   ├── 05_skills/health_check/SKILL.md
│   └── 08_validation_spec.md
├── db_optimizer/
│   ├── 05_skills/database_indexing/SKILL.md
│   ├── 06_world_model/variables.yaml
│   └── 10_values/constitution.md
└── security_auditor/
    ├── 05_skills/security_scan/SKILL.md
    └── 02_agent_spec.yaml (parcial, solo VCVs)
```

Cada paquete contiene solo los rangos que aporta. No necesita ser un agente completo.

### 8.2 Políticas de conflicto

- `merge`: Combina las skills del paquete con las existentes.
- `override`: Las skills del paquete reemplazan a las existentes con el mismo nombre.
- `skip`: Si el artefacto ya existe, se ignora.

---

## 9. Gestión Segura de Secretos

Los secretos (API keys, tokens, credenciales) **nunca se almacenan en texto plano** dentro de los artefactos `.aof/`. En su lugar, se utilizan referencias declarativas que el runtime resuelve en tiempo de ejecución según el perfil de despliegue.

### 9.1 Sintaxis de referencia

En skills o en `02_agent_spec.yaml`:
```yaml
auth:
  api_key: ${env:OPENAI_API_KEY}
  # o bien
  api_key: ${secret:openai/api_key}
```

### 9.2 Resolución por perfil

| Perfil | Backend de secretos | Mecanismo |
|--------|---------------------|-----------|
| **Standalone** | Variables de entorno o archivo `.env` (no versionado) | El runtime carga `dotenv` si existe un `.env` junto al binario. |
| **Edge** | Archivo cifrado `.aof/secrets.enc` (Age/SOPS) + clave local | El runtime descifra al arrancar usando `AOF_MASTER_KEY` (variable de entorno) o un TPM. |
| **Enterprise** | Gestor externo (HashiCorp Vault, AWS Secrets Manager, Azure Key Vault) o SPIFFE/SPIRE | El agente se autentica mediante su identidad SPIFFE y solicita los secretos al servidor configurado. |

### 9.3 Auditoría

Cada resolución de secreto se registra en `09_observations/feedback.yaml` (timestamp, nombre del secreto, backend utilizado, resultado) **sin registrar el valor**.

---

## 10. Carga Progresiva y Puerta de Evidencia

### Matriz de carga por fase (N3)

| Fase | Artefactos cargados | Tokens |
|------|---------------------|--------|
| startup | manifest, context_static, agent_spec, mission, state, constitution | 2‑5k |
| reason | episódica (top‑N), policies, variables | +2‑3k |
| hypothesize | causal_graph, delays, expert_corpus (RAG), simulation_skill | +3‑5k |
| verify (N3) | verification_rules, contratos semánticos, políticas | +1‑2k |
| execute | current_steps, skills específicas | variable |
| evaluate | feedback_log, anomaly_notes | +1k |
| reflect | hipótesis activa, learned_rules, policies, meta‑cognitiva (N3) | +2‑3k |

### Puerta de Evidencia

Mecanismo transversal: si tras la evaluación `total_errors == 0`, el agente omite las fases de hipótesis y reflexión, reduciendo drásticamente el consumo de tokens en ciclos normales. Aplica a N2 y N3.

---

## 11. Perfiles de Despliegue

| Perfil | Componentes obligatorios | Agentes | Latencia objetivo |
|--------|--------------------------|---------|-------------------|
| **Standalone** | `.aof/` + binario N1/N2 | 1 | <100ms |
| **Edge** | `.aof/`, NATS, LanceDB/SQLite (opcional) | 10‑50 | <100ms |
| **Enterprise** | `.aof/`, Kubernetes, Istio, SPIRE, OPA, Kafka | 100s | <500ms |

| Propiedad | Standalone | Edge | Enterprise |
|-----------|------------|------|------------|
| Privacidad | Máxima | Alta | Media |
| Resiliencia offline | Total | Alta | Baja |
| Gobernanza | Básica | Media | Avanzada |
| Gestión de secretos | `.env` local | Archivo cifrado (Age/SOPS) | Vault, AWS Secrets Manager, etc. |

---

## 12. Comunicación y Protocolos

AOF‑S no define una capa semántica propietaria. Se apoya en estándares abiertos:

| Capa | Protocolo | Función |
|------|-----------|---------|
| **Transporte** | SLIM (AGNTCY) | Mensajería segura de baja latencia. |
| **Sintaxis** | A2A + MCP | A2A para descubrimiento y delegación entre agentes; MCP para herramientas externas. |
| **Semántica** | Convenciones NSL sobre A2A/MCP | Semantic Handshake, Trust Score y VCVs como metadatos en Agent Cards A2A. |

### Versioned Capability Vectors (VCVs)

Cada agente expone sus capacidades como VCVs (`capability:v1`) en `02_agent_spec.yaml`. El `registry.yaml` del N3 permite búsqueda semántica de agentes por capacidades, eliminando la necesidad de un protocolo semántico separado.

---

## 13. Registro de Agentes y Descubrimiento

El `registry.yaml`, mantenido por el N3, es el directorio vivo del ecosistema. La v1.4 incluye los campos `heartbeat_interval` y `endpoint`. Los VCVs permiten el descubrimiento semántico. El N3 actúa como *Discovery Agent* natural del ecosistema.

---

## 14. Seguridad y Gobernanza Declarativa

La seguridad se declara en el `Noosfile.yaml` (políticas globales) o en `02_agent_spec.yaml` (`security_overrides` para políticas por agente).

**Pila de seguridad:**
- **Identidad:** SPIFFE (`spiffe://noos.dominio/agent/nombre`)
- **Cifrado:** mTLS gestionado por service mesh (Istio en enterprise)
- **Autorización:** OPA (Rego)

**Progresividad:**
- **N1:** Sandbox (gVisor). Sin requisitos SPIFFE/OPA.
- **N2:** mTLS automático. OPA opcional con políticas por defecto.
- **N3:** SPIFFE, mTLS y OPA obligatorios.

---

## 15. Inferencia Local‑First

Cada agente declara su configuración de inferencia en `02_agent_spec.yaml`:

- **Motores locales:** Ollama, llama.cpp, vLLM con prioridades.
- **Límites de recursos:** VRAM máxima.
- **Fallback a la nube:** Configurable mediante disparadores (motor no disponible, saturación de recursos, tarea requiere modelo no disponible localmente).

En modo `local_first` con `fallback_to_cloud: false`, el 0% de los datos abandonan el dispositivo.

---

## 16. Evaluación Integrada

Métricas declaradas en `02_agent_spec.yaml`:
- `hypothesis_precision`: proporción de hipótesis validadas.
- `falsification_rate`: proporción de hipótesis falsadas.
- `p95_latency_ms`: latencia en percentil 95.
- `avg_tokens_per_intervention`: tokens promedio por ciclo.
- `task_success_rate`: tasa de éxito en tareas.

Soporte para **MCP‑AgentBench** como benchmark externo. Comandos: `noctl evaluate agent <name>` y `noctl evaluate ecosystem`.

---

## 17. Generación Automática de Ecosistemas

La sección **Templates** de `aof-s.yaml` contiene plantillas con placeholders para cada artefacto y nivel. Un agente N3 puede:

1. Recibir un prompt en lenguaje natural del usuario.
2. Razonar con el LLM para obtener un **Plan de Ecosistema** estructurado (JSON).
3. Aplicar las plantillas y generar automáticamente los directorios `.aof/` de los agentes necesarios.
4. Validar la configuración con la acción nativa `validate_aof`.
5. Registrar los agentes en `registry.yaml` y desplegarlos (copiando skills desde `skills_repo/` o desde Paquetes AOF).

Este flujo permite crear ecosistemas multi‑agente completos a partir de una descripción de alto nivel, materializando el principio de Spec‑Driven Development.

---

## 18. Configurabilidad Sectorial

La arquitectura base de AOF‑S es invariante entre sectores. La adaptación a un dominio (hospital, datacenter, puerto, manufactura, campus, aeropuerto) se logra mediante:

- **Skills sectoriales** en `05_skills/`.
- **Corpus experto** en `06_knowledge/expert_corpus/`.
- **Agentes declarados** en el `Noosfile`.

La inicialización se realiza con `noctl init --sector <sector>`.

---

## 19. Relación con ASA, Noosistema y el Runtime

AOF‑S es el pilar de empaquetado del triángulo arquitectónico. La tabla siguiente muestra cómo el runtime ASA Agent consume los tres YAML normativos:

| YAML | Rol | Qué define | Cuándo se consume |
|------|-----|------------|-------------------|
| **`aof-s.yaml`** | Formato | Estructura de archivos, skills, plantillas, secretos, validación | Arranque del N3, generación de agentes, validación |
| **`asa.yaml`** | Arquitectura del agente | Ciclo de 7 fases, memoria cuádruple, meta‑cognición, gobernanza algorítmica | Cada ciclo ASA del agente |
| **`noosystem.yaml`** | Ecosistema | Capas de despliegue, perfiles, protocolos, registro, orquestación adaptativa | Arranque del N3, operaciones de ecosistema |

El runtime es el motor que une los tres YAML. Ninguno de ellos asume responsabilidades que pertenecen a otro.

---

## 20. Evolución del Estándar

El estándar se versiona semánticamente (MAJOR.MINOR). Las propuestas de cambio se gestionan mediante **ADR (Architecture Decision Records)** en el repositorio público. La metadata `compatible_versions` indica qué versiones anteriores son compatibles con la actual.

La gobernanza es abierta: la comunidad puede proponer extensiones a través de issues y PRs, manteniendo la compatibilidad con los principios fundacionales.

---
## 21. Directorio Central del Ecosistema

El Noosistema define un directorio central compartido por todos los agentes de una misma máquina. Su ubicación predeterminada es `~/.noosystem/`, configurable mediante la variable de entorno `NOOSYSTEM_PATH`. Este directorio centraliza los recursos comunes del ecosistema, evitando la duplicación en cada agente y garantizando la coherencia de versiones.

### 21.1 Estructura

```text
~/.noosystem/
├── specs/                     ← YAML de especificación (solo lectura)
│   ├── aof-s.yaml
│   ├── asa.yaml
│   └── noosystem.yaml
├── secrets/                   ← Claves cifradas (acceso restringido)
│   └── secrets.enc
├── packages/                  ← Paquetes AOF reutilizables
│   ├── health_check/
│   └── db_optimizer/
└── repos/                     ← Repositorios de skills comunitarias
    └── agentskills-index.yaml
```

### 21.2 Propósito de cada subdirectorio

- **`specs/`**: Contiene las versiones canónicas de las especificaciones YAML. Todos los agentes leen estas especificaciones al arrancar. Actualizar el estándar solo requiere modificar estos archivos, sin recompilar binarios ni tocar los directorios `.aof/` individuales.
- **`secrets/`**: Almacena las claves cifradas del ecosistema (Age/SOPS). El runtime las descifra al iniciar usando la clave maestra definida en `AOF_MASTER_KEY`. Los secretos nunca se almacenan en los artefactos `.aof/`.
- **`packages/`**: Repositorio local de Paquetes AOF. Los agentes pueden importar skills, knowledge o values desde estos paquetes mediante el campo `imports` en su manifiesto.
- **`repos/`**: Índices de repositorios comunitarios de skills, permitiendo al N3 descubrir e instalar nuevas capacidades bajo demanda.

### 21.3 Variable de entorno `NOOSYSTEM_PATH`

Si la variable `NOOSYSTEM_PATH` está definida, el runtime usará esa ruta como raíz del directorio central. Si no, usará `~/.noosystem/`. Si la variable está definida pero el directorio no existe, el runtime debe crearlo automáticamente con la estructura de subdirectorios esperada.
---

**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek como colaborador de diseño sistémico.  
**Documentos complementarios:** `spec/aof-s.yaml` v1.4 (especificación normativa), `spec/asa.yaml` v2.0, `spec/noosystem.yaml` v2.0, documentos conceptuales de ASA y del Noosistema.