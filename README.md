# AOF‑S: AI Orchestration Files for Systemic Agents v1.5

**El estándar abierto que materializa agentes de IA con razonamiento causal, memoria persistente y orquestación declarativa.**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Standard](https://img.shields.io/badge/AOF--S-v1.5-green.svg)](spec/aof-s.yaml)
[![Status](https://img.shields.io/badge/Status-Public%20Preview-orange.svg)]()
---
## Definición Canónica Multidimensional

**AOF‑S (AI Orchestration Files for Systemic Agents) es un estándar de configuración estructural que materializa un marco arquitectónico (Harness Engineering) y sirve como el artefacto físico fundamental para la aplicación del Spec‑Driven Development (SDD) en ecosistemas de agentes.**

## Propósito y Alcance

**AOF‑S (AI Orchestration Files for Systemic Agents)** es un estándar abierto que define el formato de empaquetado, la estructura de archivos y las convenciones de configuración para agentes de IA dentro del ecosistema Noosistema. Especifica **qué** artefactos componen un agente y **cómo** se organizan; no prescribe el comportamiento interno del agente (ámbito de la arquitectura ASA) ni la topología del ecosistema (ámbito del Noosistema).

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

## Qué NO es AOF‑S

| Concepto | ¿Es AOF‑S? | Justificación |
|----------|------------|---------------|
| **Runtime / Motor de ejecución** | No | No ejecuta código ni contiene un loop de inferencia; solo define la configuración que un runtime (como ASA Agent) interpreta. |
| **Framework de orquestación** | No | No proporciona herramientas de programación para construir agentes; es puramente declarativo (YAML/Markdown). |
| **Técnica de prompting** | No | No es un conjunto de instrucciones para el prompt; es una estructura de sistema de archivos que el agente consulta. |
| **Lenguaje de programación** | No | No es Turing completo; es un lenguaje de marcado para la cognición y las restricciones del agente. |
| **Gestor de secretos** | No | Los secretos nunca se almacenan en los artefactos. Se usan referencias (`${env:...}` o `${secret:...}`) que el runtime resuelve externamente. |

---

## Principios de Diseño

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
## ¿Qué hace único a AOF‑S?

- **Grafo Causal Probabilístico** — El agente modela P(Y|do(X)) y actualiza sus creencias.
- **Verificación neuro‑simbólica** — Un motor de reglas lógicas valida las hipótesis antes de actuar.
- **Meta‑cognición** — El agente aprende qué estrategia de razonamiento funciona mejor en cada dominio.
- **Memoria colectiva** — Los agentes reactivos alimentan a los agentes sistémicos.
- **Contratos semánticos aprendidos** — La NSL extiende A2A/MCP con negociación semántica y confianza programática.

---

## Estructura del Repositorio

```
aof-s/
├── README.md                # Este archivo
├── LICENSE                  # Apache 2.0
├── spec/                    # Especificación ejecutable (verdad única)
│   ├── aof-s.yaml           # Estándar de archivos
├── docs/                    # Documentación para humanos
│   ├──aofs-conceptual_es.md # Documento Conceptual
└── packages/               # Plantillas listas para usar
    ├── n1_test_tools/       # Nivel 1
    ├── n2-log-analyzer/     # Nivel 2
    └── n3_orquestator/      # Nivel 3
```
---

## Documentación Completa

| Documento | Descripción |
|-----------|-------------|
| [`spec/aof-s.yaml`](spec/aof-s.yaml) | Especificación formal del estándar |
| [`docs/aofs-conceptual_es.md`](docs/concept/aofs-conceptual_es.md) | Diseño conceptual AOF‑S|

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

**AOF‑S v1.5: del prompt estático al agente que razona, recuerda y aprende.**
