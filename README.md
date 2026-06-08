# AOF‑S: AI Orchestration Files for Systemic Agents v1.0

**El estándar abierto que materializa agentes de IA con razonamiento causal, memoria persistente y orquestación declarativa.**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Standard](https://img.shields.io/badge/AOF--S-v1.0-green.svg)](spec/aof-s.yaml)
[![Status](https://img.shields.io/badge/Status-Public%20Preview-orange.svg)]()

---

## El Triángulo del Noosistema

AOF‑S no existe en el vacío. Forma parte de un ecosistema de tres pilares complementarios:

| Pilar | ¿Qué es? | Este repositorio |
|-------|----------|------------------|
| **Noosistema** | El ecosistema cognitivo digital donde los agentes colaboran. | Define el contexto de aplicación. |
| **ASA** | La arquitectura del agente: cómo razona, recuerda y aprende. | Define el modelo de agencia que AOF‑S materializa. |
| **AOF‑S** | El estándar de archivos que implementa ASA en el Noosistema. | **Este repositorio.** |

---

## ¿Por qué AOF‑S?

| Problema | Solución AOF‑S |
|----------|---------------|
| Los agentes olvidan entre sesiones | **Memoria persistente** (episódica, semántica, procedural, meta‑cognitiva) |
| No saben explicar por qué actuaron | **Hipótesis obligatoria** antes de cada acción |
| Cada framework usa su propio formato | **Estándar abierto** de archivos Markdown + YAML |
| La nube es una dependencia | **Local‑First**: ejecuta en tu hardware, sin costes por token |
| No aprenden de sus errores | **Ciclo reflexivo**: hipótesis → verificación → evaluación → reflexión |

---

## Los Tres Niveles de Agencia (ASA)

AOF‑S implementa la arquitectura ASA en tres niveles progresivos:

| Nivel | Nombre | Memoria | ¿Aprende? | ¿Orquesta? | Arranca en |
|-------|--------|---------|-----------|------------|------------|
| **1** | **Reactivo** | Sin memoria persistente | No | No | 2 archivos, 5 minutos |
| **2** | **Sistémico** | Episódica + Semántica + Procedural | Sí (localmente) | No | 13 archivos |
| **3** | **Orquestado** | Cuádruple (+ Meta‑cognitiva) | Sí (local y sistémico) | Sí, otros agentes | 20+ archivos |

---

## 🚀 Empieza en 5 minutos

```bash
# 1. Inicializa un agente reactivo (solo 2 archivos)
noctl init --level 1 --ultra-light

# 2. Personaliza tu contexto
#    Edita 01_context_static.md con tu stack, comandos y reglas

# 3. Ejecuta tu agente
#    Cualquier LLM que lea el directorio .aof/ puede operar

# 4. Cuando necesites más, escala
noctl upgrade --level 2
```

---

## Estructura del Repositorio

```
aof-s/
├── README.md                # Este archivo
├── LICENSE                  # Apache 2.0
├── spec/                    # Especificación ejecutable (verdad única)
│   ├── asa.yaml             # Modelo de agencia (tres niveles)
│   ├── aof-s.yaml           # Estándar de archivos
│   ├── noosfile.yaml        # Manifiesto de orquestación
│   ├── nsl.yaml             # Capa Semántica Noosistémica
│   └── causal_graph.proto   # Grafo Causal Probabilístico
├── docs/                    # Documentación para humanos
│   ├── why.md               # Por qué existe AOF‑S
│   ├── how.md               # Cómo usarlo
│   └── guides/              # Guías detalladas
└── templates/               # Plantillas listas para usar
    ├── ultra-light/         # Nivel 1 reducido (2 archivos)
    ├── reactive/            # Nivel 1 completo
    ├── systemic/            # Nivel 2
    └── orchestrated/        # Nivel 3
```

---

## Principios de Diseño

0. **Local‑First** — Ejecuta en tu hardware. La nube es opcional.
1. **Separación cognitiva** — Cada archivo responde a una pregunta precisa.
2. **Carga progresiva** — El agente solo lee lo necesario en cada fase.
3. **Intervención reflexiva causal** — Toda acción es una hipótesis que se verifica.
4. **Memoria con propósito** — La memoria olvida, fortalece y consolida.
5. **Seguridad Zero Trust** — SPIFFE, mTLS, OPA desde el diseño.
6. **Agencia progresiva** — Empieza simple, escala cuando lo necesites.

---

## Pila de Estándares

AOF‑S se integra con los estándares abiertos del ecosistema de agentes:

| Capa | Estándar | Rol |
|------|----------|-----|
| Herramientas | **MCP** (Anthropic) | El "USB‑C" de la IA |
| Comunicación | **A2A** (Google/LF) | Colaboración entre agentes |
| **Empaquetado** | **AOF‑S** | Cómo se define, gobierna y orquesta un agente |

---

## ¿Qué hace único a AOF‑S?

- **Grafo Causal Probabilístico** — El agente modela P(Y|do(X)) y actualiza sus creencias.
- **Verificación neuro‑simbólica** — Un motor de reglas lógicas valida las hipótesis antes de actuar.
- **Meta‑cognición** — El agente aprende qué estrategia de razonamiento funciona mejor en cada dominio.
- **Memoria colectiva** — Los agentes reactivos alimentan a los agentes sistémicos.
- **Contratos semánticos aprendidos** — La NSL extiende A2A/MCP con negociación semántica y confianza programática.

---

## Documentación Completa

| Documento | Descripción |
|-----------|-------------|
| [`docs/why.md`](docs/why.md) | La filosofía detrás de AOF‑S |
| [`docs/how.md`](docs/how.md) | Guía práctica de adopción |
| [`spec/aof-s.yaml`](spec/aof-s.yaml) | Especificación formal del estándar |
| [`spec/asa.yaml`](spec/asa.yaml) | Modelo de agencia ASA |
| [`docs/concept/1_aofs-conceptual_es.md`](docs/concept/1_aofs-conceptual_es.md) | Diseño conceptual AOF‑S|

---

## Contribuir

AOF‑S es un estándar abierto en evolución. Damos la bienvenida a:

- **Propuestas de mejora (RFC)** mediante issues
- **Pull Requests** para plantillas, documentación o ejemplos
- **Casos de uso** que validen el estándar

---

## Autoría

**Jorge Y. Hernández García** — ETKinnova, Cuba   
📧 etkinnova@gmail.com

Co‑diseñado con la asistencia de **DeepSeek** (modo DeepThink) como entorno de razonamiento extendido y co‑creación iterativa.

---

## Licencia

Apache 2.0 — ver [`LICENSE`](LICENSE)

---

**AOF‑S v1.0: del prompt estático al agente que razona, recuerda y aprende.**