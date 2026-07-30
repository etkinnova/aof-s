# 🤖 ASA Agent — Runtime para Agentes Sistémicos Adaptativos

[![Rust](https://img.shields.io/badge/rust-1.82+-blue.svg)](https://www.rust-lang.org/)
[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](LICENSE)
[![Docs](https://img.shields.io/badge/docs-latest-brightgreen.svg)](https://docs.rs/asa_core)

**ASA Agent** es el runtime de referencia para la arquitectura **ASA (Agentes Sistémicos Adaptativos)**, que implementa el estándar abierto **AOF‑S (AI Orchestration Files for Systemic Agents)** v1.4 dentro del ecosistema **Noosistema**. Está escrito en Rust y produce tres binarios independientes (`asa-n1`, `asa-n2`, `asa-n3`) que cubren los tres niveles de agencia progresiva.

---

## 📖 Tabla de Contenidos

- [🤖 ASA Agent — Runtime para Agentes Sistémicos Adaptativos](#-asa-agent--runtime-para-agentes-sistémicos-adaptativos)
  - [📖 Tabla de Contenidos](#-tabla-de-contenidos)
  - [🧠 ¿Qué es ASA Agent?](#-qué-es-asa-agent)
    - [Niveles de agencia](#niveles-de-agencia)
  - [🚀 Estado actual del proyecto](#-estado-actual-del-proyecto)
  - [🏗️ Arquitectura y principios](#️-arquitectura-y-principios)
  - [📂 Estructura del repositorio](#-estructura-del-repositorio)
  - [⚙️ Requisitos previos](#️-requisitos-previos)
  - [🔧 Compilación](#-compilación)
  - [▶️ Ejecución y ejemplos](#️-ejecución-y-ejemplos)
    - [1. Ejecutar el N3 (orquestador) con una solicitud de ejemplo](#1-ejecutar-el-n3-orquestador-con-una-solicitud-de-ejemplo)
    - [2. Ejecutar el agente generado (N1)](#2-ejecutar-el-agente-generado-n1)
    - [3. Ejecutar el N3 sin solicitud (modo determinista)](#3-ejecutar-el-n3-sin-solicitud-modo-determinista)
    - [4. Ejecutar el N2 (sistémico)](#4-ejecutar-el-n2-sistémico)
  - [🌊 Flujo Spec‑Driven (N3 → generación de agentes)](#-flujo-specdriven-n3--generación-de-agentes)
  - [🌱 Variables de entorno](#-variables-de-entorno)
  - [📚 Documentación adicional](#-documentación-adicional)
  - [🗺️ Hoja de ruta y próximos pasos](#️-hoja-de-ruta-y-próximos-pasos)
  - [🤝 Contribuir](#-contribuir)
  - [📄 Licencia](#-licencia)

---

## 🧠 ¿Qué es ASA Agent?

ASA Agent es un motor genérico para agentes de IA que operan en ciclos reflexivos de **razonamiento, ejecución, evaluación y aprendizaje**. Está diseñado para ser:

- **Configurable:** Todo el comportamiento se define mediante artefactos AOF‑S (YAML/Markdown), sin necesidad de modificar el código.
- **Modular:** Arquitectura de puertos y adaptadores que permite sustituir componentes (LLM, sistema de archivos, herramientas) sin afectar al núcleo.
- **Extensible:** Soporte para herramientas nativas, MCP y funciones, con un registro dinámico que permite añadir nuevas capacidades sin recompilar.
- **Resiliente:** Degradación graciosa ante fallos del LLM, Puerta de Evidencia para ahorrar tokens, y modos de operación adaptativos.

### Niveles de agencia

| Nivel | Nombre | Capacidades |
|-------|--------|-------------|
| **N1** | Reactivo | Ciclo de 4 fases (sin memoria, sin hipótesis). Ideal para edge computing (ESP32, Raspberry Pi). |
| **N2** | Sistémico | Ciclo de 6 fases con memoria tricameral, modelo del mundo y aprendizaje local. |
| **N3** | Orquestado | Ciclo completo de 7 fases con meta‑cognición (placeholder), verificación simbólica (placeholder) y orquestación multi‑agente. |

---

## 🚀 Estado actual del proyecto

El proyecto se encuentra en una fase de **madurez funcional** con las siguientes capacidades implementadas y validadas:

| Funcionalidad | Estado |
|---------------|--------|
| Ciclo ASA completo (7 fases) | ✅ |
| Memoria episódica, semántica, procedural | ✅ |
| Modelo del mundo (variables, estado inferido) | ✅ |
| Generación Spec‑Driven de agentes (N3 → N1/N2) | ✅ |
| Registro de agentes (`registry.yaml`) | ✅ |
| Delegación de tareas (N3 → N1/N2) | ✅ |
| Herramientas nativas (`deploy_agent`, `read_file`, `write_to_file`, `delegate_task`) | ✅ |
| Plantillas desde `aof-s.yaml` con herencia `context_defaults` | ✅ |
| Degradación graciosa y modos del N3 (`complete`, `degraded`, `delegating`) | ✅ |
| Generación de skills funcionales y múltiples (estándar Agent Skills) | ✅ |
| Artefactos N1 completos (todos los requeridos por AOF‑S v1.4) | ✅ |
| Validación del agente generado (arranque y ciclo sin errores) | ✅ |
| Meta‑cognición (placeholder) | ⚠️ |
| Grafo Causal Probabilístico (placeholder) | ⚠️ |
| Verificación simbólica (placeholder) | ⚠️ |
| Gestión de secretos (placeholder) | ⚠️ |
| Soporte MCP/A2A (placeholder) | ⚠️ |

---

## 🏗️ Arquitectura y principios

El código sigue los principios de **Arquitectura Hexagonal (Puertos y Adaptadores)**:

- **Dominio (`domain/`):** Lógica de negocio pura, sin dependencias externas. Contiene el `Agent`, el ciclo ASA, la memoria y el modelo del mundo.
- **Aplicación (`application/`):** Casos de uso que orquestan el dominio (`run_n1_cycle`, `run_n2_cycle`, `run_n3_cycle`).
- **Puertos (`ports/`):** Traits que definen las dependencias del dominio (`LlmPort`, `FileSystemPort`, `ExecutorPort`).
- **Adaptadores (`adapters/`):** Implementaciones concretas de los puertos (Ollama, sistema de archivos, ejecutores).
- **Infraestructura (`infra/`):** Carga de artefactos AOF‑S (`aof_loader`, `spec_loader`, `path_renderer`).

**Principios clave:**

- **Local‑First:** El agente opera localmente por defecto; la nube es un respaldo opcional.
- **Separación cognitiva:** Cada archivo y módulo responde a una pregunta cognitiva concreta.
- **Carga progresiva:** Solo se compila y carga el código necesario para el nivel del agente (features `n1`, `n2`, `n3`).
- **Generación Spec‑Driven:** Los agentes se generan a partir de un plan JSON estructurado, sin hardcodeo de valores en el runtime.
- **Sin hardcodeo del estándar:** Todos los valores por defecto, listas de artefactos y plantillas se leen de `aof-s.yaml`.

---

## 📂 Estructura del repositorio

```
asa_agent/
├── Cargo.toml                      # Workspace raíz
├── core/                           # Librería compartida (crate asa_core)
│   ├── Cargo.toml
│   └── src/
│       ├── lib.rs
│       ├── error.rs                # Errores tipados (AgentError)
│       ├── paths.rs                # Resolución de rutas (AOF_PATH, NOOSYSTEM_PATH)
│       ├── config/                 # Estructuras de artefactos AOF‑S
│       ├── domain/                 # Lógica de negocio (Agent, ciclo, memoria, orquestación)
│       ├── application/            # Casos de uso (run_n1_cycle, run_n2_cycle, run_n3_cycle)
│       ├── ports/                  # Traits (puertos) para dependencias
│       ├── adapters/               # Implementaciones concretas (local_fs, ollama_client, executors)
│       ├── execution/              # Registro de herramientas nativas y parsers
│       ├── infra/                  # Infraestructura AOF‑S (aof_loader, spec_loader)
│       └── utils/                  # Utilidades transversales (render_path, render_template)
├── bin/                            # Binarios ejecutables
│   ├── n1/                         # asa-n1 (Reactivo)
│   ├── n2/                         # asa-n2 (Sistémico)
│   └── n3/                         # asa-n3 (Orquestado)
├── tests/                          # Pruebas de integración
├── examples/                       # Ejemplos de uso
├── .aof/                           # Artefactos de ejemplo del N3
└── specs/                          # Especificaciones YAML (aof-s.yaml, asa.yaml, noosystem.yaml)
```

---

## ⚙️ Requisitos previos

- **Rust** (edición 2021) con Cargo.
- **Ollama** (opcional, pero recomendado para el ejemplo) con el modelo `llama3.2:latest` o `llama3.1:8b`.
- **Git** para clonar el repositorio.

---

## 🔧 Compilación

```bash
# Clonar el repositorio
git clone https://github.com/etkinnova/asa_agent.git
cd asa_agent

# Compilar todos los binarios
cargo build --workspace

# Compilar solo el N3
cargo build -p asa-n3
```

---

## ▶️ Ejecución y ejemplos

### 1. Ejecutar el N3 (orquestador) con una solicitud de ejemplo

```bash
# Windows (PowerShell)
$env:ASA_USER_REQUEST="Crea un agente N1 para monitorear logs de errores cada 5 minutos"
$env:RUST_LOG="debug"
cargo run -p asa-n3

# Linux/macOS
export ASA_USER_REQUEST="Crea un agente N1 para monitorear logs de errores cada 5 minutos"
export RUST_LOG="debug"
cargo run -p asa-n3
```

El N3 generará un agente en `./nuevo-agente/error-monitor/` con todos los artefactos AOF‑S v1.4 (manifiesto, especificación, misión, contexto, tareas, skill, validación y logs).

### 2. Ejecutar el agente generado (N1)

```bash
cd ./nuevo-agente/error-monitor
$env:RUST_LOG="debug"   # (PowerShell)
cargo run -p asa-n1
```

El agente hijo arrancará y ejecutará su ciclo Reactivo (4 fases) sin errores.

### 3. Ejecutar el N3 sin solicitud (modo determinista)

```bash
cargo run -p asa-n3
```

### 4. Ejecutar el N2 (sistémico)

```bash
cargo run -p asa-n2
```

---

## 🌊 Flujo Spec‑Driven (N3 → generación de agentes)

El N3 genera agentes N1/N2 a partir de una solicitud en lenguaje natural. El flujo completo es:

1. **El usuario define `ASA_USER_REQUEST`** (variable de entorno).
2. **Fase de Razonar:** El N3 usa la skill `deploy-agent-skill`, que contiene un `output_schema` para forzar al LLM a devolver un plan JSON estructurado (`EcosystemPlan`).
3. **Parseo del plan:** El JSON se extrae de la respuesta del LLM y se almacena en `agent.ecosystem_plan`.
4. **Degradación automática:** Si hay plan, el N3 se degrada automáticamente a modo `degraded` (omite hipótesis y verificación) para ahorrar tokens.
5. **Fase de Ejecutar:** La herramienta nativa `deploy_agent`:
   - Lee el plan JSON.
   - Llama a `generate_from_plan`.
6. **`generate_from_plan`:**
   - Carga `aof-s.yaml` (desde el directorio central o fallback local).
   - Fusiona los `context_defaults` de todos los niveles según la herencia (`extends`).
   - Construye el contexto combinando el plan y los defaults.
   - Renderiza las plantillas con Handlebars y escribe todos los artefactos en `./nuevo-agente/{nombre}/.aof/`.
   - Genera una skill por cada skill declarada en el plan (sanitizando nombres según Agent Skills).
   - Registra el nuevo agente en `registry.yaml`.
7. **El agente hijo** (N1 o N2) puede ejecutarse independientemente.

---

## 🌱 Variables de entorno

| Variable | Propósito |
|----------|-----------|
| `ASA_USER_REQUEST` | Solicitud en lenguaje natural para el N3 (ej. "Crea un agente N1 para monitorizar logs"). |
| `AOF_PATH` | Ruta personalizada del directorio `.aof/` (por defecto: `./.aof/`). |
| `NOOSYSTEM_PATH` | Ruta del directorio central del Noosistema (por defecto: `~/.noosystem/`). |
| `RUST_LOG` | Nivel de log (ej. `debug`, `info`, `trace`). |

---

## 📚 Documentación adicional

| Documento | Descripción |
|-----------|-------------|
| [Documento Canónico del Proyecto](docs/ASA_AGENT_CANONICAL.md) | Referencia completa para comprender y extender el proyecto. |
| [Principios de Diseño](docs/PRINCIPLES.md) | Principios arquitectónicos y de código. |
| [Guía de `AOF_PATH`](docs/AOF_PATH_GUIDE.md) | Uso de la variable de entorno `AOF_PATH`. |
| [Guía de Estructura del Código](docs/CODESTRUCTURE_GUIDE.md) | Explicación detallada de la organización del código. |
| [Especificación AOF‑S](specs/aof-s.yaml) | Estándar normativo (fuente de verdad para plantillas y defaults). |

---

## 🗺️ Hoja de ruta y próximos pasos

| Área | Tarea | Prioridad |
|------|-------|-----------|
| **N3** | Meta‑cognición real (selección dinámica de estrategias de razonamiento). | Alta |
| **Runtime** | Gestión de secretos (resolución de `${env:VAR}` y `${secret:path}`). | Alta |
| **N3** | Grafo Causal Probabilístico y simulación MCTS. | Media |
| **N3** | Fase Verificar (motor simbólico OPA/contratos). | Media |
| **Runtime** | Soporte para MCP y A2A (extender `ExecutorPort`). | Media |
| **Ecosistema** | Heartbeats y trust score dinámico. | Media |
| **Documentación** | Actualizar guías y ejemplos con el nuevo flujo. | Media |

---

## 🤝 Contribuir

Las contribuciones son bienvenidas. Antes de enviar un Pull Request:

1. Asegurar que el código pasa `cargo fmt` y `cargo clippy`.
2. Ejecutar `cargo test --workspace` para verificar que no se rompen pruebas existentes.
3. Actualizar la documentación si se añaden nuevas funcionalidades.
4. Revisar los principios de diseño en `PRINCIPLES.md`.

Para dudas o discusiones, abrir un Issue en el repositorio.

---

## 📄 Licencia

Este proyecto está licenciado bajo **MIT** o **Apache-2.0**, a elección del usuario.

---

**ASA Agent es parte del ecosistema Noosistema.**  
Desarrollado con ❤️ por ETKinnova y la comunidad.