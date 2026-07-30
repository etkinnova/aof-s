# AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual

**Versión:** 1.5  
**Fecha:** 26 de julio de 2026  
**Parte del estándar:** AOF‑S v1.5  
**Referencia normativa:** `spec/aof-s.yaml` (prevalece en caso de discrepancia)  
**Estado:** Canónico

---

## Tabla de Contenidos

- [AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual](#aofs-ai-orchestration-files-for-systemic-agents--documento-conceptual)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [1. Propósito y Alcance](#1-propósito-y-alcance)
  - [2. Definición Canónica Multidimensional](#2-definición-canónica-multidimensional)
    - [2.1 Dimensión: Estándar de Configuración (El "Qué" / Formato)](#21-dimensión-estándar-de-configuración-el-qué--formato)
    - [2.2 Dimensión: Marco Arquitectónico o "Harness" (El "Dónde" / Estructura)](#22-dimensión-marco-arquitectónico-o-harness-el-dónde--estructura)
    - [2.3 Dimensión: Artefacto Fundamental de Spec‑Driven Development (El "Por qué" / Metodología)](#23-dimensión-artefacto-fundamental-de-specdriven-development-el-por-qué--metodología)
    - [2.4 Dimensión: Descriptor de Ecosistema y Portabilidad (El "Quién" / Gobernanza)](#24-dimensión-descriptor-de-ecosistema-y-portabilidad-el-quién--gobernanza)
  - [3. Qué NO es AOF‑S](#3-qué-no-es-aofs)
  - [4. Principios de Diseño](#4-principios-de-diseño)
  - [5. Tabla de Rangos y Estructura de Directorios](#5-tabla-de-rangos-y-estructura-de-directorios)
    - [5.1. Tabla de Rangos](#51-tabla-de-rangos)
    - [5.2. Estructura de Directorios `.aof/` (AOF‑S v1.5)](#52-estructura-de-directorios-aof-aofs-v15)
    - [5.3. Glosario de Artefactos](#53-glosario-de-artefactos)
  - [6. La Skill como Unidad de Capacidad](#6-la-skill-como-unidad-de-capacidad)
    - [6.1 Estructura del SKILL.md](#61-estructura-del-skillmd)
    - [6.2 Compatibilidad con Agent Skills](#62-compatibilidad-con-agent-skills)
    - [6.3 Tipos de herramientas](#63-tipos-de-herramientas)
    - [6.4 Modos de LLM](#64-modos-de-llm)
  - [7. El Manifiesto y la Especificación del Agente](#7-el-manifiesto-y-la-especificación-del-agente)
    - [7.1 `00_manifest.yaml`](#71-00_manifestyaml)
    - [7.2 `02_agent_spec.yaml`](#72-02_agent_specyaml)
  - [8. Paquetes AOF: Reutilización y Repositorios](#8-paquetes-aof-reutilización-y-repositorios)
  - [9. Gestión Segura de Secretos](#9-gestión-segura-de-secretos)
  - [10. Carga Progresiva y Puerta de Evidencia](#10-carga-progresiva-y-puerta-de-evidencia)
  - [11. Perfiles de Despliegue](#11-perfiles-de-despliegue)
  - [12. Comunicación y Protocolos](#12-comunicación-y-protocolos)
  - [13. Registro de Agentes y Descubrimiento](#13-registro-de-agentes-y-descubrimiento)
  - [14. Seguridad y Gobernanza Declarativa](#14-seguridad-y-gobernanza-declarativa)
  - [15. Inferencia Local‑First](#15-inferencia-localfirst)
  - [16. Evaluación Integrada](#16-evaluación-integrada)
  - [17. Generación Automática de Ecosistemas](#17-generación-automática-de-ecosistemas)
  - [18. Configurabilidad Sectorial](#18-configurabilidad-sectorial)
  - [19. Relación con ASA y el Noosistema](#19-relación-con-asa-y-el-noosistema)
  - [20. Evolución del Estándar](#20-evolución-del-estándar)

---

## 1. Propósito y Alcance

**AOF‑S (AI Orchestration Files for Systemic Agents)** es un estándar abierto que define el formato de empaquetado, la estructura de archivos y las convenciones de configuración para agentes de IA dentro del ecosistema Noosistema. Especifica **qué** artefactos componen un agente y **cómo** se organizan; no prescribe el comportamiento interno del agente (ámbito de la arquitectura ASA) ni la topología del ecosistema (ámbito del Noosistema).

Este documento explica los fundamentos del estándar para lectores humanos. La especificación técnica completa reside en `spec/aof-s.yaml` (versión 1.5), que actúa como fuente normativa y es consumida directamente por los agentes N3 para generar configuraciones.

---

## 2. Definición Canónica Multidimensional

**AOF‑S (AI Orchestration Files for Systemic Agents) es un estándar de configuración estructural que materializa un marco arquitectónico (Harness Engineering) y sirve como el artefacto físico fundamental para la aplicación del Spec‑Driven Development (SDD) en ecosistemas de agentes.**

Esta definición se desglosa en cuatro dimensiones constitutivas:

### 2.1 Dimensión: Estándar de Configuración (El "Qué" / Formato)

En su capa más básica, AOF‑S es un **estándar abierto de serialización y esquema** (análogo a OpenAPI para REST o a un Dockerfile para contenedores).

- **Función**: Define una taxonomía estricta de archivos YAML/Markdown (manifiesto, especificación, hipótesis, memoria) y una estructura de directorios (`.aof/`).
- **Propósito**: Estandarizar *cómo se escribe* la identidad, el conocimiento y las reglas de un agente, garantizando que sea legible tanto por humanos como por cualquier LLM o runtime, independientemente del proveedor.
- **Valor diferencial**: Introduce el principio de **Agencia Progresiva** (Niveles 1, 2 y 3) dentro del propio estándar, permitiendo escalar la complejidad de la configuración según la madurez del agente. Los artefactos con el mismo propósito cognitivo residen en la misma ruta en todos los niveles, facilitando la evolución sin migración.

### 2.2 Dimensión: Marco Arquitectónico o "Harness" (El "Dónde" / Estructura)

AOF‑S no es un simple archivo de configuración; es la **materialización física del Harness Engineering** (el "sistema operativo" del agente).

- **Función**: Actúa como el contenedor estructural que organiza los **Guides (feedforward)** y los **Sensors (feedback)** del agente.
- **Evidencia**:
  - Los artefactos `10_values/` y `03_mission.yaml` son los *Guides* estratégicos.
  - Los artefactos `04_hypotheses/` y `09_observations/` son los *Sensors* que permiten la reflexión y el aprendizaje.
  - Los directorios `07_memory/` (episódica, semántica) son el estado persistente del harness.
- **Valor diferencial**: Almacena el **ciclo reflexivo** (hipótesis → verificación → reflexión) directamente en el sistema de archivos, convirtiendo el harness en un ente que "aprende" y persiste su experiencia.

### 2.3 Dimensión: Artefacto Fundamental de Spec‑Driven Development (El "Por qué" / Metodología)

AOF‑S es la **encarnación física del SDD** aplicado a agentes. No es solo una técnica de SDD; es el **sistema de archivos donde viven las especificaciones**.

- **Función**: Mientras que el SDD es la *metodología* (escribe la spec antes de actuar), AOF‑S es el *repositorio de specs* que el agente lee y actualiza durante su ejecución.
- **Evidencia**:
  - El directorio `04_tasks/` es la especificación descompuesta del trabajo a realizar.
  - El agente no "adivina" qué hacer basándose en un prompt; **lee la spec** (los archivos YAML) y ejecuta contra ella.
  - La especificación (`values` e `hypotheses`) **evoluciona** con el tiempo gracias a la reflexión del agente, cerrando el bucle SDD de manera dinámica.
- **Valor diferencial**: AOF‑S genera y actualiza **estado y conocimiento del agente**, transformando el SDD de un proceso de generación de código a un proceso de evolución cognitiva.

### 2.4 Dimensión: Descriptor de Ecosistema y Portabilidad (El "Quién" / Gobernanza)

Un directorio `.aof/` contiene toda la información necesaria para que un agente sea desplegado, gobernado y orquestado en cualquier entorno (standalone, edge o cloud) sin perder su identidad o memoria.

- **Función**: Actúa como el **"pasaporte" de un agente dentro de un ecosistema**.
- **Propósito**: Materializa el **contrato de intenciones** del agente con el ecosistema. Al ser `Local‑First`, garantiza que el agente pueda operar offline y sincronizarse cuando vuelva a estar en línea.

---

## 3. Qué NO es AOF‑S

| Concepto | ¿Es AOF‑S? | Justificación |
|----------|------------|---------------|
| **Runtime / Motor de ejecución** | No | No ejecuta código ni contiene un loop de inferencia; solo define la configuración que un runtime (como ASA Agent) interpreta. |
| **Framework de orquestación** | No | No proporciona herramientas de programación para construir agentes; es puramente declarativo (YAML/Markdown). |
| **Técnica de prompting** | No | No es un conjunto de instrucciones para el prompt; es una estructura de sistema de archivos que el agente consulta. |
| **Lenguaje de programación** | No | No es Turing completo; es un lenguaje de marcado para la cognición y las restricciones del agente. |
| **Gestor de secretos** | No | Los secretos nunca se almacenan en los artefactos. Se usan referencias (`${env:...}` o `${secret:...}`) que el runtime resuelve externamente. |

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
| 7 | **Progressive Agency** | La complejidad se activa por nivel. El agente evoluciona desde 3 archivos (N1 ultra‑ligero) hasta 30+ (N3) mediante `noctl upgrade`. Los artefactos con el mismo propósito residen en la misma ruta en todos los niveles. |

---

## 5. Tabla de Rangos y Estructura de Directorios

Los artefactos AOF‑S se organizan en **rangos numéricos fijos** que actúan como una taxonomía estable de dominios cognitivos. Cada rango agrupa artefactos que responden a una misma pregunta fundamental sobre el agente. Esta numeración no refleja un orden secuencial de importancia, sino una **agrupación funcional** que se mantiene invariable en todos los niveles de agencia.

### 5.1. Tabla de Rangos

| Rango | Dominio | Pregunta cognitiva |
|:---|:---|:---|
| `00` | Identidad | ¿Quién soy? |
| `01` | Contexto estático | ¿Dónde estoy? |
| `02` | Especificación del agente | ¿Qué capacidades tengo? |
| `03` | Misión | ¿Qué debo lograr? |
| `04` | Tareas y planificación | ¿Qué voy a hacer? |
| `05` | Skills (habilidades) | ¿Qué sé hacer? |
| `06` | Modelo del mundo | ¿Cómo es el mundo que percibo? |
| `07` | Memoria | ¿Qué he aprendido? |
| `08` | Validación | ¿Cómo verifico que cumplo las reglas? |
| `09` | Observaciones | ¿Qué está ocurriendo? |
| `10` | Valores y gobernanza | ¿Bajo qué principios y con quién gobierno mis acciones? |
| `11` | Identidad noosistémica (solo N3) | ¿Cómo me comunico con otros agentes? |

### 5.2. Estructura de Directorios `.aof/` (AOF‑S v1.5)

A continuación se muestra la estructura completa del directorio `.aof/` para un agente N3. Los artefactos exclusivos de N2 o N3 están marcados como `[N2+]` o `[N3]`.

```
.aof/                                    # Raíz del agente AOF‑S v1.5
│
├── 00_manifest.yaml                    # [YAML] Identidad, nivel, perfil, skills activas, imports
├── 01_context_static.md                # [MD]   Contexto inmutable del proyecto
├── 02_agent_spec.yaml                  # [YAML] Rol, VCVs, herramientas permitidas,
│                                         #       restricciones, inferencia, modo de operación
├── 03_mission.yaml                     # [YAML] Misión, restricciones y notas
│
├── 04_tasks/                           # Tareas y planificación
│   ├── current_plan.md                 # [MD]   Plan actual de trabajo
│   ├── executed/                       #        Tareas delegadas ejecutadas (trazabilidad) [N2+]
│   ├── contracts/                      # [MD]   Contratos entre agentes [N2+]
│   └── delegated/                      #        Tareas delegadas entrantes [N3]
│       └── current_steps.md            # [MD]   Tarea delegada actual
│
├── 04_hypotheses/                      # Hipótesis [N2+]
│   ├── active.md                       # [MD]   Hipótesis activa (N2)
│   ├── active/                         #        Hipótesis activas (N3, directorio)
│   ├── validated/                      #        Hipótesis validadas [N3]
│   └── falsified/                      #        Hipótesis falsificadas [N3]
│
├── 05_skills/                          # Habilidades (agentskills.io compatible)
│   └── <skill-name>/                   # Cada skill es un subdirectorio
│       ├── SKILL.md                    # [MD+YAML] Metadatos, herramientas e instrucciones
│       ├── scripts/                    # Scripts ejecutables (opcional)
│       ├── references/                 # Documentación de referencia (opcional)
│       └── assets/                     # Plantillas y recursos (opcional)
│
├── 06_world_model/                     # Modelo del mundo [N2+]
│   ├── variables.yaml                  # [YAML] Variables observables
│   ├── state.md                        # [MD]   Estado inferido del sistema
│   ├── causal_graph.proto              #        Grafo causal probabilístico [N3, placeholder]
│   ├── delays.yaml                     # [YAML] Retardos entre variables [N3]
│   └── expert_corpus/                  # Corpus de conocimiento experto [N3]
│       ├── vector_store/               # Base de datos vectorial para RAG
│       ├── source_documents/           # Documentos fuente
│       └── sectors/                    # Conocimiento sectorial (hospital, datacenter, etc.)
│
├── 07_memory/                          # Memoria del agente [N2+]
│   ├── episodic/                       # Episodios (se generan automáticamente)
│   ├── semantic/
│   │   ├── learned_rules.yaml          # [YAML] Reglas semánticas aprendidas
│   │   └── domain_facts.md             # [MD]   Hechos del dominio [N3]
│   ├── procedural/
│   │   └── policies.yaml               # [YAML] Políticas procedurales (Sistema 1)
│   └── meta/
│       └── meta_strategies.yaml         # [YAML] Rendimiento de estrategias [N3, placeholder]
│
├── 08_validation_spec.md               # [MD]   Reglas de validación
│
├── 09_observations/                    # Registro de observaciones
│   ├── feedback.yaml                   # [YAML] Feedback estructurado (N1, N2, N3)
│   └── anomalies.md                    # [MD]   Anomalías detectadas [N2+]
│
├── 10_values/                          # Valores y gobernanza
│   ├── constitution.md                 # [MD]   Principios inviolables [N2+]
│   ├── preferences.yaml                # [YAML] Preferencias del usuario [N2+]
│   └── registry.yaml                   # [YAML] Registro de agentes del ecosistema [N3]
│
└── 11_ASA.yaml                         # [YAML] Identidad noosistémica, comunicación y federación [N3]
```

### 5.3. Glosario de Artefactos

| Artefacto | Formato | Nivel | Función resumida |
|-----------|--------|-------|------------------|
| `00_manifest.yaml` | YAML | N1‑N3 | Identidad del agente: nombre, nivel, perfil de despliegue, skills activas e imports. |
| `01_context_static.md` | Markdown | N1‑N3 | Contexto inmutable del proyecto: stack tecnológico, dependencias y reglas fijas. |
| `02_agent_spec.yaml` | YAML | N1‑N3 | Especificación del agente: rol, VCVs, herramientas permitidas, restricciones, inferencia y modo (N3). |
| `03_mission.yaml` | YAML | N1‑N3 | Misión del agente en lenguaje natural, restricciones operativas y notas. |
| `04_tasks/current_plan.md` | Markdown | N1‑N3 | Plan actual de trabajo del agente. |
| `04_tasks/executed/` | — | N2‑N3 | Archivo de tareas delegadas procesadas con timestamp (trazabilidad). |
| `04_tasks/contracts/` | Markdown | N2‑N3 | Contratos de colaboración entre agentes (micro‑contratos). |
| `04_tasks/delegated/current_steps.md` | Markdown | N3 | Tarea delegada entrante desde el N3 a un agente hijo. |
| `04_hypotheses/active.md` | Markdown | N2 | Hipótesis activa del agente sistémico (un solo archivo). |
| `04_hypotheses/active/` | Markdown | N3 | Directorio de hipótesis activas del orquestador. |
| `04_hypotheses/validated/` | Markdown | N3 | Hipótesis que han sido validadas tras la reflexión. |
| `04_hypotheses/falsified/` | Markdown | N3 | Hipótesis que han sido refutadas tras la reflexión. |
| `05_skills/<skill>/SKILL.md` | Markdown + YAML | N1‑N3 | Skill con metadatos YAML (herramientas, modo LLM) e instrucciones en Markdown. |
| `06_world_model/variables.yaml` | YAML | N2‑N3 | Variables observables del modelo del mundo. |
| `06_world_model/state.md` | Markdown | N2‑N3 | Estado inferido actual del sistema. |
| `06_world_model/causal_graph.proto` | Protobuf | N3 | Grafo causal probabilístico (placeholder). |
| `06_world_model/delays.yaml` | YAML | N3 | Retardos entre variables del modelo del mundo. |
| `06_world_model/expert_corpus/` | — | N3 | Corpus de conocimiento experto: base vectorial, documentos fuente y sectores. |
| `07_memory/episodic/` | — | N2‑N3 | Memoria episódica: registro cronológico de intervenciones. |
| `07_memory/semantic/learned_rules.yaml` | YAML | N2‑N3 | Memoria semántica: reglas y patrones causales extraídos de la experiencia. |
| `07_memory/semantic/domain_facts.md` | Markdown | N3 | Hechos del dominio aprendidos por el agente. |
| `07_memory/procedural/policies.yaml` | YAML | N2‑N3 | Memoria procedural: políticas de acción automatizadas (Sistema 1). |
| `07_memory/meta/meta_strategies.yaml` | YAML | N3 | Memoria meta‑cognitiva: rendimiento histórico de estrategias de razonamiento. |
| `08_validation_spec.md` | Markdown | N1‑N3 | Reglas de validación AOF‑S que el agente debe cumplir. |
| `09_observations/feedback.yaml` | YAML | N1‑N3 | Registro de observaciones y métricas de cada ciclo. |
| `09_observations/anomalies.md` | Markdown | N2‑N3 | Anomalías detectadas durante la operación. |
| `10_values/constitution.md` | Markdown | N2‑N3 | Principios inviolables que rigen el comportamiento del agente. |
| `10_values/preferences.yaml` | YAML | N2‑N3 | Preferencias del usuario (configuración flexible). |
| `10_values/registry.yaml` | YAML | N3 | Registro de agentes del ecosistema: identidad, capacidades, estado y confianza. |
| `11_ASA.yaml` | YAML | N3 | Identidad noosistémica, configuración de comunicación A2A y federación. |

---

## 6. La Skill como Unidad de Capacidad

Desde la v1.3, cada skill es un **subdirectorio** dentro de `05_skills/`, cumpliendo con el estándar **agentskills.io**. Esto permite que skills creadas para AOF‑S se usen en otros ecosistemas y viceversa.

```
05_skills/
└── health-check/
    ├── SKILL.md           # Metadatos YAML + instrucciones
    ├── scripts/           # Código ejecutable (opcional)
    ├── references/        # Documentación (opcional)
    └── assets/            # Plantillas (opcional)
```

### 6.1 Estructura del SKILL.md

```markdown
---
name: health-check
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
# Skill: health-check
Realiza una petición HTTP a cada endpoint y registra el resultado.
```

### 6.2 Compatibilidad con Agent Skills

Los campos `tools` y `llm` son extensiones opcionales de AOF‑S. Si una skill no los incluye (por ejemplo, una skill estándar de agentskills.io), el runtime debe usar el cuerpo Markdown como fuente de instrucciones para el LLM, garantizando compatibilidad total con el ecosistema comunitario.

### 6.3 Tipos de herramientas

| Tipo | Campo YAML | Descripción |
|------|------------|-------------|
| **MCP** | `mcp_server` | Herramienta proporcionada por un servidor Model Context Protocol. La skill declara el nombre del servidor; el runtime se conecta a él. |
| **Nativa** | `native` | Herramienta integrada en el runtime (`deploy_agent`, `validate_aof`, `generate_workspace`). Se identifica mediante el campo `action`. |
| **Función** | `function` | Herramienta definida con JSON Schema, compatible con el tool calling de OpenAI y Anthropic. Portable a cualquier plataforma. |

### 6.4 Modos de LLM

| Modo | Comportamiento | Cuándo usarlo |
|------|----------------|---------------|
| `never` | Ejecución determinista sin LLM. | Health checks, tareas sin ambigüedad. |
| `always` | Consulta al LLM antes de cada ejecución. | Tareas que requieren análisis contextual. |
| `on_error` | Solo consulta si el ciclo anterior tuvo errores. | Monitorización con diagnóstico bajo demanda. |
| `on_change` | Consulta si cambian la misión, el contexto o los endpoints. | Agentes adaptativos de bajo consumo. |
| `cached` | Busca primero en memoria semántica una respuesta de alta confianza; si no, consulta y almacena. | FAQs, diagnósticos recurrentes. |

---

## 7. El Manifiesto y la Especificación del Agente

### 7.1 `00_manifest.yaml`

El manifiesto declara la identidad, el perfil de despliegue y las skills activas del agente. También permite importar Paquetes AOF externos.

### 7.2 `02_agent_spec.yaml`

Define el rol, las capacidades (VCVs), el modo de operación (para N3) y las restricciones. El campo `mode` especifica el modo máximo autorizado; el runtime puede degradarse automáticamente a modos más ligeros según la tarea, pero nunca superarlo.

---

## 8. Paquetes AOF: Reutilización y Repositorios

Un **Paquete AOF** es un directorio que agrupa artefactos reutilizables (skills, knowledge, values). Se referencia desde el manifiesto mediante el campo `imports`, con políticas de conflicto configurables (`merge`, `override`, `skip`).

---

## 9. Gestión Segura de Secretos

Los secretos **nunca** se almacenan en texto plano dentro de los artefactos. Se utilizan referencias `${env:VAR}` o `${secret:path}` que el runtime resuelve según el perfil de despliegue: `.env` en standalone, archivos cifrados (Age/SOPS) en edge, y gestores externos (Vault, AWS Secrets Manager) o SPIFFE/SPIRE en enterprise.

---

## 10. Carga Progresiva y Puerta de Evidencia

El agente solo carga los artefactos necesarios para la fase actual del ciclo ASA. La **Puerta de Evidencia** (definida en `asa.yaml`) omite las fases de hipótesis y reflexión cuando no se han detectado errores, ahorrando tokens en N2 y N3.

La tabla Muestra, para cada fase del ciclo ASA y cada nivel de agente, qué artefactos se cargan (📖), se crean o modifican (✍️), se archivan (🗄️) o se generan (🆕), reflejando las unificaciones de rutas de la v1.5.

| Fase | N1 | N2 | N3 | Artefactos involucrados |
|------|:--:|:--:|:--:|--------------------------|
| **1. Arranque** | 📖 | 📖 | 📖 | `00_manifest.yaml`, `02_agent_spec.yaml`, `03_mission.yaml`, `01_context_static.md` |
| | — | 📖 | 📖 | `06_world_model/variables.yaml`, `06_world_model/state.md`, `07_memory/procedural/policies.yaml`, `07_memory/semantic/learned_rules.yaml`, `10_values/constitution.md` |
| | — | — | 📖 | `10_values/registry.yaml`, `11_ASA.yaml` |
| | — | — | ✍️ | `04_tasks/delegated/current_steps.md` (se carga y se renombra a `executed/` tras procesarla) |
| **2. Razonar** | 📖 | 📖 | 📖 | `07_memory/episodic/` (top‑N), `07_memory/procedural/policies.yaml`, `06_world_model/variables.yaml`, `07_memory/semantic/domain_facts.md` |
| | — | — | 📖 | `07_memory/meta/meta_strategies.yaml` (si existe; placeholder en v1.5) |
| **3. Hipótesis** | — | ✍️ | ✍️ | `04_hypotheses/active.md` (N2) o `04_hypotheses/active/<timestamp>.md` (N3) |
| | — | — | 📖 | `06_world_model/causal_graph.proto`, `06_world_model/delays.yaml`, `06_world_model/expert_corpus/` (RAG) |
| **4. Verificar** | — | — | 📖 | `08_validation_spec.md`, contratos semánticos (`02_agent_spec.yaml`), políticas OPA (`Noosfile.yaml` o `security_overrides`) |
| **5. Ejecutar** | 📖 | 📖 | 📖 | `04_tasks/current_plan.md`, skills activas (`05_skills/*/SKILL.md`) |
| | — | — | ✍️ | `06_world_model/state.md` (si la ejecución modifica el estado), `04_tasks/delegated/current_steps.md` (se archiva en `executed/`) |
| **6. Evaluar** | ✍️ | ✍️ | ✍️ | `09_observations/feedback.yaml` (se añaden métricas) |
| | — | ✍️ | ✍️ | `09_observations/anomalies.md` (si se detectan anomalías) |
| **7. Reflexionar** | — | ✍️ | ✍️ | `07_memory/episodic/` (nuevo episodio), `07_memory/semantic/learned_rules.yaml` (reglas actualizadas), `07_memory/procedural/policies.yaml` (políticas refinadas) |
| | — | — | ✍️ | `07_memory/meta/meta_strategies.yaml` (rendimiento actualizado) |
| | — | — | 🗄️ | `04_hypotheses/active/<timestamp>.md` → `04_hypotheses/validated/` o `falsified/` |
| | — | — | ✍️ | `10_values/registry.yaml` (trust scores, heartbeats) |
| **Puerta de Evidencia** | — | ✍️ | ✍️ | Si `total_errors == 0`, se omiten las fases 3 y 7 (no se escriben hipótesis ni se actualiza memoria) |

**Leyenda:**
- 📖 Carga/lectura (el artefacto debe existir previamente).
- ✍️ Creación o modificación (el artefacto se escribe o actualiza en esta fase).
- 🗄️ Archivado (el artefacto se mueve de ubicación al completar la fase).
- 🆕 Generación (el artefacto se crea por primera vez; aplica a la generación Spec‑Driven por el N3).
- — No aplica a este nivel.

---

## 11. Perfiles de Despliegue

| Perfil | Agentes | Componentes | Gestión de Secretos |
|--------|---------|-------------|----------------------|
| **Standalone** | 1 | `.aof/` + binario | Variables de entorno / `.env` |
| **Edge** | 10‑50 | `.aof/`, NATS, LanceDB opcional | Archivo cifrado (Age/SOPS) + clave maestra local |
| **Enterprise** | 100s | `.aof/`, Kubernetes, Istio, SPIRE, OPA, Kafka | Vault, AWS Secrets Manager o SPIFFE/SPIRE |

---

## 12. Comunicación y Protocolos

AOF‑S se apoya en estándares abiertos:

| Capa | Protocolo | Función |
|------|-----------|---------|
| **Transporte** | SLIM (AGNTCY) | Mensajería segura de baja latencia. |
| **Sintaxis** | A2A + MCP | A2A para descubrimiento y delegación entre agentes; MCP para herramientas externas. |
| **Semántica** | VCVs + convenciones NSL sobre A2A/MCP | Descubrimiento semántico sin capa propietaria. |

---

## 13. Registro de Agentes y Descubrimiento

El `registry.yaml` (ubicado en `10_values/` desde la v1.5, en coherencia con su función de gobernanza) contiene la identidad, el estado, las capacidades (VCVs) y los endpoints de cada agente. El N3 actúa como Discovery Agent natural del ecosistema.

---

## 14. Seguridad y Gobernanza Declarativa

La seguridad se declara en el `Noosfile.yaml` (políticas globales) o en `02_agent_spec.yaml` (overrides). La pila se compone de identidades SPIFFE, cifrado mTLS y autorización OPA. Los requisitos son progresivos: mínimos en N1, automáticos en N2 y obligatorios en N3.

---

## 15. Inferencia Local‑First

Cada agente declara su motor de inferencia en `02_agent_spec.yaml`: motores locales (Ollama, llama.cpp, vLLM) con prioridades y fallback opcional a la nube. En modo `local_first` con `fallback_to_cloud: false`, el 0% de los datos abandona el dispositivo.

---

## 16. Evaluación Integrada

Métricas estándar declaradas: `hypothesis_precision`, `falsification_rate`, `p95_latency_ms`, `avg_tokens_per_intervention`, `task_success_rate`. Soporte para benchmarks externos como MCP‑AgentBench. Comandos: `noctl evaluate agent <name>` y `noctl evaluate ecosystem`.

---

## 17. Generación Automática de Ecosistemas

La sección **Templates** de `aof-s.yaml` contiene plantillas con placeholders para cada artefacto y nivel. Los `context_defaults` se heredan de niveles padre mediante `extends`. El N3 puede generar automáticamente los directorios `.aof/` de múltiples agentes a partir de un plan JSON (`EcosystemPlan`) devuelto por el LLM, validando cada uno con `validate_aof`.

**Placeholders especiales del runtime:**
| Placeholder | Origen | Ejemplo |
|-------------|--------|---------|
| `creation_date` | Generado por el runtime (`chrono::Utc::now()`) | `2026-07-26` |
| `instance_id` | Generado por el runtime (timestamp) | `agent-1234567890` |
| `skill_body` | Cuerpo Markdown de la skill | `# Instrucciones\n...` |

---

## 18. Configurabilidad Sectorial

El núcleo de AOF‑S es invariante entre sectores. La adaptación a un dominio (hospital, datacenter, puerto, manufactura, campus, aeropuerto) se logra intercambiando skills, corpus experto y agentes en el `Noosfile`.

---

## 19. Relación con ASA y el Noosistema

AOF‑S es el pilar de empaquetado del triángulo arquitectónico. El runtime ASA Agent consume tres YAML para operar:

| YAML | Rol | Define |
|------|-----|--------|
| `aof-s.yaml` | Formato | Estructura de archivos, skills, plantillas, validación. |
| `asa.yaml` | Arquitectura del agente | Ciclo de 7 fases, memoria, meta‑cognición, gobernanza. |
| `noosystem.yaml` | Ecosistema | Capas, perfiles, protocolos, orquestación. |

---

## 20. Evolución del Estándar

El estándar se versiona semánticamente. Las propuestas de cambio se gestionan mediante ADR en el repositorio público. La metadata `compatible_versions` indica las versiones anteriores compatibles. El comando `noctl migrate` facilitará la transición de agentes existentes a nuevas versiones del estándar.

---

**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek como colaborador de diseño sistémico.  
**Documentos complementarios:** `spec/aof-s.yaml` v1.5, `spec/asa.yaml` v1.5, `spec/noosystem.yaml` v1.5, documentos conceptuales de ASA y del Noosistema.