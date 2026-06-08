# Cómo usar AOF‑S: Guía práctica de adopción

**Propósito:** Este documento es una guía paso a paso para empezar a usar AOF‑S en un proyecto real. Explica cómo inicializar un agente, configurar sus artefactos, escalar entre niveles y entender el flujo de trabajo diario.

---

## 1. Requisitos previos

- **`noctl`** instalado (o acceso a la CLI). Consulta [`spec/noctlspec.md`](../spec/noctlspec.md) para instrucciones de instalación.
- **Un LLM accesible**: local (Ollama, llama.cpp) o cloud (opcional). AOF‑S no depende de un proveedor concreto.
- **Un directorio de proyecto** donde residirá la carpeta `.aof/`.

---

## 2. Inicio rápido: tu primer agente en 5 minutos

El modo **ultra‑light** del Nivel 1 genera solo dos archivos. Es la forma más rápida de tener un agente funcional.

```bash
# 1. Inicializa un agente reactivo en modo ultra‑light
noctl init --level 1 --ultra-light

# Esto crea la carpeta .aof/ con:
#   .aof/00_manifest.yaml
#   .aof/01_context_static.md
```

### 2.1 Personaliza tu contexto

Edita `.aof/01_context_static.md` con la información de tu proyecto:

```markdown
# Stack
Node.js 22, PostgreSQL 16, pnpm

# Comandos
- build: `pnpm build`
- test: `pnpm test -- --coverage`
- lint: `pnpm lint`

# Reglas
1. No modificar `schema.sql` sin PR aprobado.
2. Usar tipos generados desde OpenAPI.
```

### 2.2 Ejecuta tu agente

Cualquier LLM que lea el directorio `.aof/` puede operar como un agente AOF‑S. El archivo `00_manifest.yaml` le indica qué estándar está usando y en qué nivel se encuentra.

```yaml
# 00_manifest.yaml (ejemplo)
framework: AOF‑S v1.0
level: 1
project: "mi-api"
created: "2026-06-07"
agent_instance: "agent-001"
```

Tu agente ya está funcionando. Ejecuta tareas, haz preguntas, pide cambios en el código. El agente usará `01_context_static.md` para entender tu stack y tus reglas.

---

## 3. Entendiendo la estructura del Nivel 1

Cuando necesites más que el modo ultra‑light, puedes generar la estructura completa del Nivel 1:

```bash
noctl upgrade --level 1
```

Esto añadirá los archivos restantes:

| Archivo | Propósito |
|---------|-----------|
| `00_manifest.yaml` | Metadatos del agente (framework, nivel, proyecto). |
| `01_context_static.md` | Stack, comandos, reglas inmutables. |
| `02_mission.yaml` | Qué necesidad debe resolver el agente. |
| `03_agent_spec.yaml` | Rol, herramientas permitidas, restricciones, evaluación. |
| `04_tasks/current_plan.md` | Plan de acción actual. |
| `05_skills/` | Habilidades modulares (testing, deployment, etc.). |
| `06_validation_spec.md` | Criterios de aceptación. |
| `07_observations.log` | Registro de observaciones y métricas. |

---

## 4. Escalando al Nivel 2 (Sistémico)

Cuando tu agente necesite **recordar** entre sesiones y **aprender** de sus intervenciones:

```bash
noctl upgrade --level 2
```

Se añadirán los artefactos del Nivel 2:

| Nuevo artefacto | Propósito |
|-----------------|-----------|
| `04_tasks/hypothesis.md` | Hipótesis de intervención con efectos de 1º y 2º orden. |
| `04_tasks/steps.md` | Pasos concretos derivados de la hipótesis. |
| `05_world_model/variables.yaml` | Variables observables del sistema. |
| `05_world_model/state.md` | Estado actual inferido del sistema. |
| `07_memory/episodic/` | Registro cronológico de intervenciones. |
| `07_memory/procedural/policies.yaml` | Políticas de acción aprendidas. |
| `09_observations/feedback.yaml` | Métricas estructuradas. |
| `09_observations/anomalies.md` | Anomalías detectadas. |
| `10_values/constitution.md` | Principios inviolables del agente. |
| `10_values/preferences.yaml` | Preferencias de usuario. |

A partir de este nivel, el agente **cierra el ciclo de aprendizaje localmente**: formula hipótesis antes de actuar, registra los resultados, compara y actualiza sus políticas procedurales.

---

## 5. Configurando el Nivel 3 (Orquestado)

Cuando necesites **orquestar múltiples agentes** y **razonamiento avanzado**:

```bash
noctl upgrade --level 3
```

Este nivel añade:

| Nuevo artefacto | Propósito |
|-----------------|-----------|
| `ASA.yaml` | Identidad completa del agente (seguridad, inferencia, perfil semántico). |
| `04_knowledge/world_model/causal_graph.proto` | Grafo Causal Probabilístico. |
| `04_knowledge/world_model/delays.yaml` | Retardos del sistema con distribuciones de probabilidad. |
| `04_knowledge/expert_corpus/` | Corpus de conocimiento indexado (RAG nativo). |
| `04_knowledge/registry.yaml` | Registro de agentes del ecosistema. |
| `04_knowledge/hypotheses/` | Historial de hipótesis (activas, validadas, falsadas). |
| `06_memory/semantic/` | Reglas y hechos extraídos de la experiencia. |
| `06_memory/procedural/policies.yaml` | Políticas de acción (actualizadas). |

### 5.1 El Noosfile

Para orquestar múltiples agentes, necesitas un `Noosfile.yaml` en la raíz de tu proyecto:

```yaml
apiVersion: "noos.cncf.io/v1alpha2"
kind: "Noosystem"
metadata:
  name: "mi-ecosistema"
spec:
  mission: "Garantizar SLO del flujo de checkout"
  noos_level: 2
  agents:
    - name: "orchestrator"
      level: 3
      scaling: { replicas: 1 }
    - name: "db-provisioner"
      level: 2
      scaling: { replicas: 1 }
  security:
    agent_identity: "SPIFFE"
    mTLS: true
  global_policies:
    trust_threshold: 0.85
```

Aplica el manifiesto:

```bash
noctl apply -f noosfile.yaml
```

---

## 6. Flujo de trabajo diario

### 6.1 El ciclo ASA en acción

El agente AOF‑S sigue un ciclo de 7 fases (con *Verificar* solo en N3):

1. **Arranque**: lee `manifest.yaml`, `context_static.md`, `mission.yaml`, `constitution.md`.
2. **Razonar**: contextualiza la tarea. En N2/N3, recupera memoria relevante.
3. **Hipótesis**: (N2/N3) formula una predicción con efectos esperados.
4. **Verificar**: (N3) valida la hipótesis con un motor de reglas lógicas.
5. **Ejecutar**: ejecuta los pasos y monitoriza.
6. **Evaluar**: registra métricas y detecta anomalías.
7. **Reflexionar**: (N2/N3) compara resultado con hipótesis, actualiza el modelo y la memoria.

### 6.2 El agente escribe sus propios artefactos

AOF‑S no es un formato estático. El agente **lee y escribe** los archivos como parte de su ciclo:

- Antes de actuar: escribe `hypothesis.md` con sus predicciones.
- Después de actuar: escribe `observations/feedback_log.yaml` con los resultados.
- Periódicamente: actualiza `learned_rules.yaml` con nuevas reglas semánticas.
- En cada reflexión: actualiza `policies.yaml` con nuevas políticas procedurales.

---

## 7. Personalización

### 7.1 Añadir skills

Las skills son archivos Markdown en `05_skills/` que encapsulan capacidades modulares:

```markdown
# testing_skill.md
Ejecutar `pnpm test` y verificar cobertura > 80%.
```

El agente las cargará cuando la tarea las requiera.

### 7.2 Cambiar la misión

Edita `02_mission.yaml`:

```yaml
mission: "Reducir el tiempo de build en un 20%"
constraints:
  - "No romper tests e2e"
success_metrics:
  - "Tiempo de build < 120s"
```

### 7.3 Configurar la inferencia local (N2/N3)

En `ASA.yaml` o `03_agent_spec.yaml`, declara tus motores locales:

```yaml
inference:
  mode: "local_first"
  local_engines:
    - name: "ollama"
      models: ["llama3.2:3b", "qwen2.5-coder:7b"]
      priority: 1
  resource_limits:
    max_vram_gb: 8
    fallback_to_cloud: false
```

---

## 8. Próximos pasos

- **Explora la especificación completa**: [`spec/aof-s.yaml`](../spec/aof-s.yaml).
- **Entiende la filosofía**: [`docs/why.md`](why.md).
- **Profundiza en la arquitectura ASA**: [`spec/asa.yaml`](../spec/asa.yaml).
- **Aprende sobre la capa semántica**: [`spec/nsl.yaml`](../spec/nsl.yaml).

---

**AOF‑S no es otro archivo de configuración. Es el sistema operativo de tu agente.** Empieza con 2 archivos y 5 minutos. Escala cuando lo necesites.
