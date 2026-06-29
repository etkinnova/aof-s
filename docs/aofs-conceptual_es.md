# AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual

**Versión:** 3.0  
**Fecha:** 28 de junio de 2026  
**Parte del estándar:** AOF‑S v1.2  
**Referencia normativa:** `spec/aof-s.yaml` (prevalece en caso de discrepancia)  
**Estado:** Canónico

---

## Tabla de Contenidos

1. [Propósito y Alcance](#1-propósito-y-alcance)
2. [Definición Canónica](#2-definición-canónica)
3. [Principios de Diseño](#3-principios-de-diseño)
4. [Niveles de Agencia y Estructura de Archivos](#4-niveles-de-agencia-y-estructura-de-archivos)
5. [La Skill como Unidad de Capacidad](#5-la-skill-como-unidad-de-capacidad)
6. [El Manifiesto y la Especificación del Agente](#6-el-manifiesto-y-la-especificación-del-agente)
7. [Carga Progresiva y Puerta de Evidencia](#7-carga-progresiva-y-puerta-de-evidencia)
8. [Perfiles de Despliegue](#8-perfiles-de-despliegue)
9. [Protocolos e Interoperabilidad](#9-protocolos-e-interoperabilidad)
10. [Seguridad y Gobernanza Declarativa](#10-seguridad-y-gobernanza-declarativa)
11. [Inferencia Local‑First](#11-inferencia-local-first)
12. [Evaluación Integrada](#12-evaluación-integrada)
13. [Configurabilidad Sectorial](#13-configurabilidad-sectorial)
14. [Relación con ASA y el Noosistema](#14-relación-con-asa-y-el-noosistema)
15. [Evolución del Estándar](#15-evolución-del-estándar)

---

## 1. Propósito y Alcance

**AOF‑S (AI Orchestration Files for Systemic Agents)** es un estándar abierto que define el formato de empaquetado, la estructura de archivos y las convenciones de configuración para agentes de IA. Especifica **qué** artefactos componen un agente y **cómo** se organizan; no prescribe **qué** comportamiento debe exhibir el agente (eso corresponde a la arquitectura ASA) ni **dónde** opera (ámbito del Noosistema).

Este documento explica los fundamentos del estándar para lectores humanos. La especificación técnica completa reside en `spec/aof-s.yaml` (versión 1.2), que actúa como fuente normativa y es consumida directamente por los agentes N3 para generar configuraciones.

---

## 2. Definición Canónica

Un agente AOF‑S es un directorio `.aof/` que contiene archivos Markdown y YAML. Estos archivos constituyen el **contrato ejecutable** del agente: definen su identidad, su misión, sus capacidades, su memoria, su modelo del mundo y las políticas bajo las que opera. El agente lee y escribe estos artefactos a lo largo de su ciclo de vida, manteniendo un registro versionable y auditable de todas sus decisiones.

La especificación separa explícitamente el **formato** (AOF‑S) del **runtime** que lo interpreta (ASA Agent u otros). Esto permite que cualquier plataforma adopte el estándar sin depender de una implementación concreta.

---

## 3. Principios de Diseño

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

## 4. Niveles de Agencia y Estructura de Archivos

AOF‑S define tres niveles de agencia progresiva. La especificación YAML lista los artefactos exactos para cada uno.

### N1 — Reactivo (8 archivos, modo ultra‑ligero: 3 archivos)

```
.aof/
├── 00_manifest.yaml
├── 01_context_static.md
├── 02_agent_spec.yaml
├── 03_mission.yaml
├── 04_tasks/current_plan.md
├── 05_skills/testing_skill.md
├── 08_validation_spec.md
└── 09_observations.log
```

El modo ultra‑ligero (ESP32, RPi) omite todos los archivos excepto `00_manifest.yaml`, `01_context_static.md` y `02_agent_spec.yaml`. No requiere LLM.

### N2 — Sistémico (16 archivos)

Añade modelo del mundo (`06_world_model/`), memoria tricameral (`07_memory/`), hipótesis (`04_tasks/hypothesis.md`) y constitución (`10_values/constitution.md`).

### N3 — Orquestado (30+ archivos)

Añade grafo causal (`causal_graph.proto`), registro de agentes (`registry.yaml`), corpus experto, memoria meta‑cognitiva y modos de operación.

### Modos del N3

| Modo | Fases | Contexto |
|------|-------|----------|
| **Completo** | 7 | Simulación causal, verificación neuro‑simbólica, orquestación global. |
| **Degradado** | 5 | Operación en edge sin simulación ni verificación. |
| **Delegando** | 4 | Enruta tareas a N2/N1 especializados. |

---

## 5. La Skill como Unidad de Capacidad

Las skills residen en `05_skills/`. AOF‑S v1.2 adopta el formato **agentskills.io** para garantizar interoperabilidad con el ecosistema comunitario. Una skill AOF‑S puede ser usada por otras plataformas, y las skills comunitarias pueden ser usadas por agentes AOF‑S sin modificaciones.

### 5.1 Estructura de una skill

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

llm:
  mode: "on_error"
---
# Skill: health_check
Realiza una petición HTTP a cada endpoint y registra el resultado.
```

### 5.2 Tipos de herramientas

AOF‑S v1.2 reconoce tres tipos de herramientas en las skills. El runtime las interpreta en este orden de prioridad:

| Tipo | Campo YAML | Descripción |
|------|------------|-------------|
| **MCP** | `mcp_server` | Herramienta proporcionada por un servidor Model Context Protocol. La skill declara el nombre del servidor; el runtime se conecta a él. |
| **Nativa** | `native` | Herramienta integrada en el runtime (ej. `generate_agent`, `http_request`). Se identifica mediante el campo `action`. |
| **Función** | `function` | Herramienta definida con JSON Schema, compatible con el tool calling de OpenAI y Anthropic. Portable a cualquier plataforma. |

**Regla de prioridad:** si una herramienta declara `mcp_server`, se usa MCP. Si declara `native`, se usa la acción nativa. Si solo declara `function`, el runtime puede exponerla al LLM mediante function calling o buscar un adaptador MCP genérico.

### 5.3 Ejecución de scripts locales

Para ejecutar scripts del sistema (bash, Python) sin romper la portabilidad de la skill, se recomienda usar el servidor MCP interno **`local-command-server`** que el runtime ASA Agent proporciona. La skill declara una herramienta MCP que apunta a este servidor, encapsulando el comando como parámetro:

```yaml
tools:
  - name: check_disk
    mcp_server:
      name: local-command-server
    function:
      parameters:
        script:
          type: string
    defaults:
      script: "df -h /"
```

De este modo, la skill sigue siendo compatible con agentskills.io y MCP, y la ejecución local queda confinada al runtime, que puede aplicar políticas de seguridad.

### 5.4 Modos de LLM

| Modo | Comportamiento | Cuándo usarlo |
|------|----------------|---------------|
| `never` | Ejecución determinista sin LLM. | Health checks, tareas sin ambigüedad. |
| `always` | Consulta al LLM antes de cada ejecución. | Tareas que requieren análisis contextual. |
| `on_error` | Solo consulta si el ciclo anterior tuvo errores. | Monitorización con diagnóstico bajo demanda. |
| `on_change` | Consulta si cambian la misión, el contexto o los endpoints. | Agentes adaptativos de bajo consumo. |

---

## 6. El Manifiesto y la Especificación del Agente

### 6.1 `00_manifest.yaml`

El manifiesto declara la identidad del agente. AOF‑S v1.2 añade dos campos opcionales:

```yaml
agent:
  name: "db-provisioner"
  level: 2
  version: "1.0.0"
  description: "Optimiza y mantiene bases de datos"
  metadata:
    project: "ecommerce-ops"
    instance: "db-001"
    created: "2026-06-27"
  active_skills:
    - diagnostic_skill.md
  inference:
    mode: "local_first"
    engines:
      - ollama
    fallback_to_cloud: false
```

- **`metadata`:** pares clave‑valor libres para identificar el proyecto, la instancia o cualquier otra información operativa.
- **`active_skills`:** lista de archivos de skill (dentro de `05_skills/`) que este agente tiene activos. Si no se especifica, se consideran activas todas las skills del directorio.

### 6.2 `02_agent_spec.yaml`

Define el rol, las capacidades (VCVs) y las restricciones operativas del agente. AOF‑S v1.2 añade dos campos opcionales:

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
```

- **`allowed_tools`:** lista blanca de herramientas que el agente puede usar (principio de mínimo privilegio).
- **`constraints`:** límites operativos que el runtime debe respetar.

---

## 7. Carga Progresiva y Puerta de Evidencia

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

Mecanismo transversal implementado en el runtime: si tras la evaluación `total_errors == 0`, el agente omite las fases de hipótesis y reflexión, reduciendo drásticamente el consumo de tokens en ciclos normales. Aplica a N2 y N3.

---

## 8. Perfiles de Despliegue

AOF‑S reconoce que no todos los entornos tienen la misma infraestructura. Define tres perfiles:

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

---

## 9. Protocolos e Interoperabilidad

AOF‑S no define una capa semántica propietaria. Se apoya en estándares abiertos:

| Capa | Protocolo | Función |
|------|-----------|---------|
| **Transporte** | SLIM (AGNTCY) | Mensajería segura de baja latencia. |
| **Sintaxis** | A2A + MCP | A2A para descubrimiento y delegación entre agentes; MCP para herramientas externas. |
| **Semántica** | Convenciones NSL sobre A2A/MCP | Semantic Handshake, Trust Score y VCVs como metadatos en Agent Cards A2A. |

### Versioned Capability Vectors (VCVs)

Cada agente expone sus capacidades como VCVs (`capability:v1`) en `02_agent_spec.yaml`. El `registry.yaml` del N3 permite búsqueda semántica de agentes por capacidades, eliminando la necesidad de un protocolo semántico separado.

---

## 10. Seguridad y Gobernanza Declarativa

La seguridad se declara en el `Noosfile.yaml`. Los artefactos AOF‑S no contienen lógica de seguridad; solo exponen la configuración.

**Pila de seguridad:**
- **Identidad:** SPIFFE (`spiffe://noos.dominio/agent/nombre`)
- **Cifrado:** mTLS gestionado por service mesh (Istio en enterprise)
- **Autorización:** OPA (Rego)

**Progresividad:**
- **N1:** Sandbox (gVisor). Sin requisitos SPIFFE/OPA.
- **N2:** mTLS automático. OPA opcional con políticas por defecto.
- **N3:** SPIFFE, mTLS y OPA obligatorios.

---

## 11. Inferencia Local‑First

Cada agente declara su configuración de inferencia en `02_agent_spec.yaml`:

- **Motores locales:** Ollama, llama.cpp, vLLM con prioridades.
- **Límites de recursos:** VRAM máxima.
- **Fallback a la nube:** Configurable mediante disparadores (motor no disponible, saturación de recursos, tarea requiere modelo no disponible localmente).

En modo `local_first` con `fallback_to_cloud: false`, el 0% de los datos abandonan el dispositivo.

---

## 12. Evaluación Integrada

Métricas declaradas en `02_agent_spec.yaml`:
- `hypothesis_precision`: proporción de hipótesis validadas.
- `falsification_rate`: proporción de hipótesis falsadas.
- `p95_latency_ms`: latencia en percentil 95.
- `avg_tokens_per_intervention`: tokens promedio por ciclo.
- `task_success_rate`: tasa de éxito en tareas.

Soporte para **MCP‑AgentBench** como benchmark externo. Comandos: `noctl evaluate agent <name>` y `noctl evaluate ecosystem`.

---

## 13. Configurabilidad Sectorial

La arquitectura base de AOF‑S es invariante entre sectores. La adaptación a un dominio (hospital, datacenter, puerto, manufactura, campus, aeropuerto) se logra mediante:

- **Skills sectoriales** en `05_skills/`.
- **Corpus experto** en `06_knowledge/expert_corpus/`.
- **Agentes declarados** en el `Noosfile`.

La inicialización se realiza con `noctl init --sector <sector>`.

---

## 14. Relación con ASA y el Noosistema

AOF‑S es el tercer pilar del triángulo arquitectónico:

| Pilar | Rol |
|-------|-----|
| **ASA** | Define la arquitectura del agente (ciclo, memoria, niveles). |
| **Noosistema** | Define el ecosistema colaborativo (capas, protocolos, gobernanza). |
| **AOF‑S** | Define el estándar de archivos que materializa ASA y gobierna el Noosistema. |

La especificación `aof-s.yaml` declara `implements: "ASA v2.0"` y `ecosystem: "Noosystem"`, estableciendo la trazabilidad entre los tres pilares.

---

## 15. Evolución del Estándar

El estándar se versiona semánticamente (MAJOR.MINOR). Las propuestas de cambio se gestionan mediante **ADR (Architecture Decision Records)** en el repositorio público. La gobernanza es abierta: la comunidad puede proponer extensiones a través de issues y PRs, manteniendo la compatibilidad con los principios fundacionales.

---

**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek como colaborador de diseño sistémico.  
**Documentos complementarios:** `spec/aof-s.yaml` v1.2 (especificación normativa), documentos conceptuales de ASA y del Noosistema.  
