# AOF‑S: AI Orchestration Files for Systemic Agents — Documento Conceptual y Normativo

**Versión:** 1.6  
**Fecha:** 4 de septiembre de 2026  
**Parte del estándar:** AOF‑S v1.6  
**Referencias normativas:**  
- `aof-s.yaml` v1.6 — especificación canónica del estándar  
- `aof-s.templates.yaml` v1.6 — plantillas de generación Spec‑Driven  
- `PRINCIPIOS-RECTORES.md` v3.0 — principios del ecosistema  

**Estado:** Canónico  
**Idioma:** Español  
**Audiencia:** Arquitectos, desarrolladores de runtimes, autores de skills y paquetes AOF, agentes N3 y adoptantes del estándar.

---

## Tabla de contenidos

1. Propósito y alcance  
2. Definiciones y terminología  
3. Principios de diseño aplicados  
4. Taxonomía de rangos cognitivos  
5. Estructura canónica de directorios `.aof/`  
6. Niveles de agencia y artefactos requeridos  
7. Descripción normativa de artefactos  
8. La skill como unidad de capacidad  
9. Especificación del agente (`01_agent_spec/agent_spec.yaml`)  
10. Manifiesto del agente (`00_manifest.yaml`)  
11. Paquetes AOF e imports  
12. Gestión de secretos  
13. Carga progresiva de contexto  
14. Puerta de Evidencia  
15. Perfiles de despliegue  
16. Protocolos e interoperabilidad  
17. Registro de agentes  
18. Plantillas Spec‑Driven  
19. Validación y conformidad  
20. Evolución del estándar  
21. Anexos  

---

## 1. Propósito y alcance

### 1.1 Propósito

AOF‑S define **qué artefactos** componen un agente ASA, **dónde** se ubican, **qué formato** tienen y **qué reglas** deben cumplir. Es la base de empaquetado, validación, generación y gobernanza de agentes en el ecosistema Noosistema.

Este documento es la referencia normativa para:

- Implementadores de runtimes compatibles con AOF‑S.
- Autores de skills y paquetes AOF.
- Agentes N3 que generan o validan agentes.
- Desarrolladores que crean herramientas de inspección o migración.

### 1.2 Alcance

AOF‑S cubre:

- Estructura de directorios `.aof/`.
- Taxonomía de rangos cognitivos.
- Formato de cada artefacto.
- Reglas de nombres y rutas.
- Sintaxis de skills y herramientas.
- Mecanismos de importación de paquetes.
- Gestión de referencias a secretos.
- Carga progresiva de contexto.
- Contratos para generación Spec‑Driven.

### 1.3 Fuera de alcance

AOF‑S **no** define:

- El comportamiento interno del agente (ASA).
- La topología del ecosistema ni sus perfiles (Noosistema).
- El runtime o su implementación.
- La CLI `noctl` (definida en `noosystem.yaml`).
- La lista de sectores soportados (definida en `noosystem.yaml`).
- El directorio central del ecosistema (definido en `noosystem.yaml`).

Ante cualquier discrepancia entre este documento y `aof-s.yaml`, prevalece `aof-s.yaml`.

---

## 2. Definiciones y terminología

| Término | Definición |
|---------|------------|
| **AOF‑S** | AI Orchestration Files for Systemic Agents. Estándar de empaquetado y configuración. |
| **Artefacto** | Archivo o directorio dentro de `.aof/` que cumple una función cognitiva. |
| **Rango cognitivo** | Prefijo numérico `00`‑`12` que agrupa artefactos por pregunta cognitiva. |
| **Agente** | Instancia ASA materializada por un directorio `.aof/`. |
| **Skill** | Unidad reutilizable de capacidad, empaquetada en `06_skills/<skill>/`. |
| **AOF Package** | Directorio reutilizable que puede importarse a un agente. |
| **Noosfile** | Manifiesto de ecosistema. Se referencia desde AOF‑S pero se define en `noosystem.yaml`. |
| **Spec‑Driven Development** | Generación de agentes a partir de planes JSON y plantillas declarativas. |
| **Executor** | Campo que determina cómo se despacha una herramienta: `native`, `mcp`, `system_command`, `function`. |
| **VCV** | Versioned Capability Vector. Identificador versionado de capacidad. |
| **Carga progresiva** | Mecanismo por el cual solo se cargan los artefactos necesarios para la fase y nivel. |
| **Puerta de Evidencia** | Mecanismo ASA que omite hipótesis y reflexión si no hay errores. |
| **Success criteria** | Criterio declarativo opcional para evaluar el éxito de una misión o tarea. |
| **Placeholder** | Valor simbólico entre ángulos (`<...>`) que indica estructura, no valor literal. |

---

## 3. Principios de diseño aplicados

Los principios transversales del ecosistema se definen en `PRINCIPIOS-RECTORES.md` v3.0. AOF‑S los materializa de la siguiente manera:

| Principio | Materialización en AOF‑S |
|-----------|--------------------------|
| **Local‑First** | Los artefactos residen en `.aof/` local; la nube es opcional. |
| **Separación Cognitiva** | Los rangos `00`‑`12` agrupan artefactos por pregunta cognitiva. |
| **Carga Progresiva** | Tabla de artefactos por fase; compilación condicional en runtime. |
| **Intervención Reflexiva Causal** | Rango propio `05_hypotheses/` para predicciones. |
| **Memoria con Propósito** | Rango `08_memory/` con subdirectorios por tipo. |
| **Zero Trust** | Secretos externos, validación estricta, perfiles progresivos. |
| **Evaluabilidad Integrada** | Métricas y validación declaradas en artefactos. |
| **Agencia Progresiva** | Niveles N1, N2, N3; artefactos compartidos en las mismas rutas. |

---

## 4. Taxonomía de rangos cognitivos

### 4.1 Regla general

Cada artefacto AOF‑S pertenece a **un único rango cognitivo**. El prefijo numérico no indica orden de ejecución, sino agrupación conceptual.

### 4.2 Tabla canónica de rangos

| Rango | Dominio | Pregunta cognitiva | Directorio/archivo raíz |
|-------|---------|--------------------|--------------------------|
| `00` | Identidad | ¿Quién soy? | `00_manifest.yaml` |
| `01` | Especificación del agente | ¿Qué capacidades tengo? | `01_agent_spec/` |
| `02` | Contexto estático | ¿En qué entorno fijo opero? | `02_context_static.md` |
| `03` | Misión | ¿Qué debo lograr? | `03_mission.yaml` |
| `04` | Tareas y planificación | ¿Qué voy a hacer? | `04_tasks/` |
| `05` | Hipótesis y predicciones | ¿Qué espero que ocurra cuando actúe? | `05_hypotheses/` |
| `06` | Skills | ¿Qué sé hacer? | `06_skills/` |
| `07` | Modelo del mundo | ¿Cómo es el mundo que percibo? | `07_world_model/` |
| `08` | Memoria | ¿Qué he aprendido? | `08_memory/` |
| `09` | Validación | ¿Cómo verifico que cumplo las reglas? | `09_validation_spec.md` |
| `10` | Observaciones | ¿Qué está ocurriendo? | `10_observations/` |
| `11` | Valores y gobernanza | ¿Bajo qué principios y con quién gobierno? | `11_values/` |
| `12` | Identidad noosistémica | ¿Cómo me comunico con otros agentes? | `12_ASA.yaml` |

### 4.3 Reglas de los rangos

- Los rangos son estables. No se reutilizan números para dominios distintos.
- Un artefacto no puede pertenecer a dos rangos.
- Los directorios de rango pueden contener subdirectorios, pero el propósito cognitivo no cambia.
- Los niveles N1, N2 y N3 usan los mismos rangos para los artefactos que comparten.

---

## 5. Estructura canónica de directorios `.aof/`

La raíz de un agente AOF‑S es un directorio `.aof/`.

### 5.1 Vista completa N3

```
.aof/
├── 00_manifest.yaml
├── 01_agent_spec/
│   └── agent_spec.yaml
├── 02_context_static.md
├── 03_mission.yaml
├── 04_tasks/
│   ├── current_plan.md
│   ├── plan_context.yaml        # N2/N3
│   ├── executed/
│   └── delegated/
│       └── current_steps.md
├── 05_hypotheses/
│   ├── active.md                 # N2
│   ├── active/                   # N3
│   ├── validated/                # N3
│   └── falsified/                # N3
├── 06_skills/
│   └── <skill-name>/
│       ├── SKILL.md
│       ├── scripts/
│       ├── references/
│       ├── assets/
│       ├── templates/
│       └── endpoints.yaml
├── 07_world_model/
│   ├── variables.yaml
│   ├── state.md
│   ├── causal_graph.proto        # N3
│   ├── delays.yaml               # N3
│   └── expert_corpus/            # N3
│       ├── vector_store/
│       ├── source_documents/
│       └── sectors/
├── 08_memory/
│   ├── episodic/
│   ├── semantic/
│   │   ├── learned_rules.yaml
│   │   └── domain_facts.md       # N3
│   ├── procedural/
│   │   └── policies.yaml
│   └── meta/
│       └── meta_strategies.yaml  # N3
├── 09_validation_spec.md
├── 10_observations/
│   ├── feedback.yaml
│   └── anomalies.md
├── 11_values/
│   ├── constitution.md
│   ├── preferences.yaml
│   ├── registry.yaml             # N3
│   ├── ledger.yaml
│   └── trust_score.yaml
└── 12_ASA.yaml                   # N3
```

### 5.2 Notas estructurales

- Los artefactos marcados como N2 o N3 no existen en niveles inferiores.
- Los archivos `.gitkeep` son permitidos en directorios vacíos para control de versiones.
- No se permiten artefactos fuera de los rangos definidos.
- Los directorios opcionales dentro de skills no alteran la validez del agente.

---

## 6. Niveles de agencia y artefactos requeridos

### 6.1 N1 — Reactivo

**Requisitos mínimos:**

| Artefacto | Formato | Obligatorio |
|-----------|---------|-------------|
| `00_manifest.yaml` | YAML | Sí |
| `01_agent_spec/agent_spec.yaml` | YAML | Sí |

**Artefactos completos N1:**

- `00_manifest.yaml`
- `01_agent_spec/agent_spec.yaml`
- `02_context_static.md`
- `03_mission.yaml`
- `04_tasks/current_plan.md`
- `06_skills/`
- `09_validation_spec.md`
- `10_observations/feedback.yaml`

**Modo ultra‑ligero N1:**

Solo se requieren:

- `00_manifest.yaml`
- `01_agent_spec/agent_spec.yaml`
- `02_context_static.md`

En modo ultra‑ligero no hay `06_skills/`, por lo que no pueden declararse herramientas MCP.

### 6.2 N2 — Sistémico

Añade a N1:

- `04_tasks/executed/`
- `04_tasks/plan_context.yaml`
- `05_hypotheses/active.md`
- `07_world_model/variables.yaml`
- `07_world_model/state.md`
- `08_memory/episodic/`
- `08_memory/procedural/policies.yaml`
- `10_observations/anomalies.md`
- `11_values/constitution.md`
- `11_values/preferences.yaml`
- `11_values/ledger.yaml`
- `11_values/trust_score.yaml`

### 6.3 N3 — Orquestado

Añade a N2:

- `04_tasks/delegated/current_steps.md`
- `05_hypotheses/active/`
- `05_hypotheses/validated/`
- `05_hypotheses/falsified/`
- `07_world_model/causal_graph.proto`
- `07_world_model/delays.yaml`
- `07_world_model/expert_corpus/`
- `08_memory/semantic/domain_facts.md`
- `08_memory/meta/meta_strategies.yaml`
- `11_values/registry.yaml`
- `12_ASA.yaml`

---

## 7. Descripción normativa de artefactos

### 7.1 Formato de artefactos

| Clasificación | Descripción | Artefactos |
|---------------|-------------|------------|
| `yaml_pure` | YAML estructurado, parseable directamente. | `00_manifest.yaml`, `01_agent_spec/agent_spec.yaml`, `03_mission.yaml`, `04_tasks/plan_context.yaml`, `07_world_model/variables.yaml`, `08_memory/semantic/learned_rules.yaml`, `08_memory/procedural/policies.yaml`, `10_observations/feedback.yaml`, `11_values/preferences.yaml`, `11_values/registry.yaml`, `11_values/ledger.yaml`, `11_values/trust_score.yaml`, `12_ASA.yaml` |
| `markdown_pure` | Texto libre para humanos o narrativa del agente. | `02_context_static.md`, `04_tasks/current_plan.md`, hipótesis, `07_world_model/state.md`, `09_validation_spec.md`, `10_observations/anomalies.md`, `11_values/constitution.md` |
| `markdown_with_frontmatter` | Frontmatter YAML + cuerpo Markdown. | `06_skills/*/SKILL.md` |

**Regla:** El runtime no debe autodetectar el formato. La clasificación es normativa.

### 7.2 `00_manifest.yaml`

**Pregunta:** ¿Quién soy?  
**Formato:** YAML puro  
**Niveles:** N1, N2, N3  

Campos definidos:

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|-------------|-------------|
| `framework` | string | Sí | Versión del estándar AOF‑S. |
| `level` | integer | Sí | Nivel del agente: 1, 2 o 3. |
| `name` | string | Sí | Nombre del agente. |
| `project` | string | No | Proyecto o instancia superior. |
| `created` | string | No | Fecha de creación. |
| `agent_instance` | string | No | Identificador único de instancia. |
| `profile` | string | Sí | Perfil de despliegue: `standalone`, `edge`, `enterprise`. |
| `active_skills` | list | No | Lista de skills activas. Si se omite, se consideran todas las de `06_skills/`. |
| `imports` | list | No | Lista de paquetes AOF a importar. |

Ejemplo mínimo:

```yaml
framework: AOF-S v1.6
level: 1
name: "vital-signs-monitor"
profile: "standalone"
```

### 7.3 `01_agent_spec/agent_spec.yaml`

**Pregunta:** ¿Qué capacidades tengo?  
**Formato:** YAML puro  
**Niveles:** N1, N2, N3  

Es el contrato central del agente. Se describe en detalle en la sección 9.

### 7.4 `02_context_static.md`

**Pregunta:** ¿En qué entorno fijo opero?  
**Formato:** Markdown puro  
**Niveles:** N1, N2, N3  

Reglas:

- Es **inmutable** durante la operación del agente.
- Contiene stack tecnológico, dependencias fijas, límites del entorno y reglas permanentes.
- No debe contener datos dinámicos ni observaciones.

### 7.5 `03_mission.yaml`

**Pregunta:** ¿Qué debo lograr?  
**Formato:** YAML puro  
**Niveles:** N1, N2, N3  

Estructura recomendada:

```yaml
mission: "Descripción de la misión"
constraints:
  - "Restricción 1"
  - "Restricción 2"
notes: "Notas adicionales"
success_criteria:
  type: "tool_success"
  tool: "csv_to_markdown"
```

`success_criteria` es opcional. Si está presente y no se cumple, el fallo de misión se cuenta como error en la Puerta de Evidencia.

### 7.6 `04_tasks/`

**Pregunta:** ¿Qué voy a hacer?  
**Formato:** Markdown para planes; YAML para contexto táctico.

| Artefacto | Formato | Nivel | Descripción |
|-----------|---------|-------|-------------|
| `current_plan.md` | Markdown | N1‑N3 | Descripción de la tarea actual en lenguaje natural. |
| `plan_context.yaml` | YAML | N2‑N3 | Restricciones tácticas y criterios de éxito específicos del plan. |
| `executed/` | Directorio | N2‑N3 | Archivo de tareas ejecutadas con timestamp. |
| `delegated/current_steps.md` | Markdown | N3 | Tarea delegada entrante. |

Reglas:

- `current_plan.md` no debe contener pasos detallados ni nombres de archivo concretos; solo el objetivo.
- `plan_context.yaml` no duplica la misión; solo añade restricciones tácticas.
- El N3 delega un objetivo, no herramientas específicas.
- Al procesar una tarea delegada, el agente la archiva en `executed/`.

### 7.7 `05_hypotheses/`

**Pregunta:** ¿Qué espero que ocurra cuando actúe?  
**Formato:** Markdown puro  
**Niveles:** N2, N3  

| Artefacto | Nivel | Descripción |
|-----------|-------|-------------|
| `active.md` | N2 | Hipótesis activa. |
| `active/` | N3 | Directorio de hipótesis activas. |
| `validated/` | N3 | Hipótesis validadas. |
| `falsified/` | N3 | Hipótesis falsificadas. |

Cada hipótesis debe contener:

- Predicción.
- Confianza.
- Evidencia.
- Efectos previstos de 1º, 2º y 3º orden.

### 7.8 `06_skills/`

**Pregunta:** ¿Qué sé hacer?  
**Formato:** Directorio de skills.  
**Niveles:** N1, N2, N3  

Se describe en detalle en la sección 8.

### 7.9 `07_world_model/`

**Pregunta:** ¿Cómo es el mundo que percibo?  
**Formato:** Mixto.  
**Niveles:** N2, N3  

| Artefacto | Formato | Nivel | Descripción |
|-----------|---------|-------|-------------|
| `variables.yaml` | YAML | N2‑N3 | Variables observables. |
| `state.md` | Markdown | N2‑N3 | Estado inferido. |
| `causal_graph.proto` | Protobuf | N3 | Grafo causal probabilístico. |
| `delays.yaml` | YAML | N3 | Retardos entre variables. |
| `expert_corpus/` | Directorio | N3 | Corpus sectorial para RAG. |

### 7.10 `08_memory/`

**Pregunta:** ¿Qué he aprendido?  
**Niveles:** N2, N3  

| Artefacto | Formato | Nivel | Descripción |
|-----------|---------|-------|-------------|
| `episodic/` | Directorio | N2‑N3 | Memoria episódica. |
| `semantic/learned_rules.yaml` | YAML | N2‑N3 | Reglas semánticas. |
| `semantic/domain_facts.md` | Markdown | N3 | Hechos del dominio. |
| `procedural/policies.yaml` | YAML | N2‑N3 | Políticas procedurales. |
| `meta/meta_strategies.yaml` | YAML | N3 | Rendimiento de estrategias. |

### 7.11 `09_validation_spec.md`

**Pregunta:** ¿Cómo verifico que cumplo las reglas?  
**Formato:** Markdown puro  
**Niveles:** N1, N2, N3  

Debe incluir:

- Nivel del agente.
- Referencia a la versión de `aof-s.yaml`.
- Reglas de validación específicas.

### 7.12 `10_observations/`

**Pregunta:** ¿Qué está ocurriendo?  
**Formato:** YAML y Markdown.  

| Artefacto | Formato | Nivel |
|-----------|---------|-------|
| `feedback.yaml` | YAML | N1‑N3 |
| `anomalies.md` | Markdown | N2‑N3 |

`feedback.yaml` es de escritura automática por el runtime.

### 7.13 `11_values/`

**Pregunta:** ¿Bajo qué principios y con quién gobierno?  
**Formato:** Mixto.  

| Artefacto | Formato | Nivel |
|-----------|---------|-------|
| `constitution.md` | Markdown | N2‑N3 |
| `preferences.yaml` | YAML | N2‑N3 |
| `registry.yaml` | YAML | N3 |
| `ledger.yaml` | YAML | N2‑N3 |
| `trust_score.yaml` | YAML | N2‑N3 |

### 7.14 `12_ASA.yaml`

**Pregunta:** ¿Cómo me comunico con otros agentes?  
**Formato:** YAML puro  
**Nivel:** N3  

Define identidad SPIFFE, canales A2A y federación.

---

## 8. La skill como unidad de capacidad

### 8.1 Ubicación

Cada skill reside en `06_skills/<skill-name>/`.

El nombre del directorio debe:

- Ser único dentro del agente.
- Estar en minúsculas.
- Usar guiones como separadores.
- No contener espacios ni caracteres especiales.

### 8.2 `SKILL.md`

Es el único archivo obligatorio de la skill.

#### Frontmatter YAML

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|-------------|-------------|
| `name` | string | Sí | Nombre de la skill. |
| `description` | string | Sí | Descripción breve. |
| `version` | string | No | Versión semántica. |
| `tools` | list | No | Lista de herramientas. Extensión AOF‑S. |
| `llm` | object | No | Configuración de LLM. Extensión AOF‑S. |
| `allowed-tools` | list | No | Restricción de herramientas para esta skill. |

#### Cuerpo Markdown

Contiene instrucciones para el LLM o para un operador humano.

Regla de compatibilidad:

- Si una skill **no** define `tools`, el runtime debe inyectar el cuerpo Markdown como instrucciones.
- Si una skill define `tools`, el cuerpo Markdown complementa la ejecución.

### 8.3 Herramientas y `executor`

Cada herramienta debe definir `executor` o asumir el valor por defecto `native`.

| Executor | Descripción |
|----------|-------------|
| `native` | Herramienta integrada en el runtime. |
| `mcp` | Herramienta de servidor MCP. |
| `system_command` | Comando del sistema. |
| `function` | Herramienta delegada al LLM con JSON Schema. |

#### Herramienta nativa

```yaml
tools:
  - name: get_weather
    description: "Obtiene el clima"
    executor: "native"
    native:
      action: "http_request"
    defaults:
      method: "GET"
      url: "https://api.example.com/weather"
```

Las acciones nativas válidas están declaradas en `asa.yaml` (sección `native_capabilities`), no en AOF‑S. AOF‑S solo define la sintaxis de declaración en las skills.

#### Herramienta MCP

```yaml
tools:
  - name: echo
    description: "Echo MCP tool"
    executor: "mcp"
    mcp_server:
      name: "server-everything"
```

#### Herramienta function

```yaml
tools:
  - name: analyze_text
    description: "Analiza texto"
    executor: "function"
    function:
      parameters:
        type: object
        properties:
          text:
            type: string
        required: ["text"]
```

### 8.4 Modos LLM

| Modo | Comportamiento |
|------|----------------|
| `never` | Sin consulta al LLM. |
| `always` | Consulta antes de cada ejecución. |
| `on_error` | Consulta solo si hubo errores. |
| `on_change` | Consulta si cambian misión, contexto o endpoints. |
| `cached` | Usa memoria semántica si hay confianza; si no, consulta y almacena. |

### 8.5 `endpoints.yaml`

Archivo opcional para la herramienta nativa `collect_endpoints`.

```yaml
endpoints:
  service_health:
    path: "/health"
    json_path: "$.status"
  service_metrics:
    path: "/metrics"
    json_path: "$.total"
```

### 8.6 Subdirectorios opcionales

| Directorio | Descripción |
|------------|-------------|
| `scripts/` | Código ejecutable. |
| `references/` | Documentación de referencia. |
| `assets/` | Plantillas y recursos. |
| `templates/` | Plantillas específicas de la skill. |

---

## 9. Especificación del agente (`01_agent_spec/agent_spec.yaml`)

### 9.1 Propósito

Es el contrato central que declara capacidades, restricciones, inferencia, hipótesis, enrutamiento, planificación, confianza y comunicación del agente.

### 9.2 Campos principales

| Campo | Tipo | Nivel | Descripción |
|-------|------|-------|-------------|
| `role` | string | N1‑N3 | Rol legible. |
| `vcv` | list | N1‑N3 | Capacidades versionadas. |
| `allowed_tools` | list | N1‑N3 | Herramientas permitidas a nivel de agente. |
| `constraints` | object | N1‑N3 | Límites operativos. |
| `run_mode` | string | N1‑N3 | `once` o `continuous`. |
| `interval_seconds` | integer/null | N1‑N3 | Intervalo entre ciclos. |
| `inference` | object | N1‑N3 | Motor de inferencia. |
| `mode` | string | N3 | Modo máximo: `complete`, `degraded`, `delegating`. |
| `hypothesis` | object | N2‑N3 | Configuración de hipótesis. |
| `llm_routing` | object | N1‑N3 | Reglas de enrutamiento LLM. |
| `trust_score_config` | object | N2‑N3 | Pesos y parámetros de confianza. |
| `plan` | object | N1‑N3 | Plan estático opcional. |
| `heartbeat_interval_secs` | integer | N3 | Intervalo de heartbeats. |
| `heartbeat_timeout_secs` | integer | N3 | Timeout de heartbeats. |
| `context_summary` | string | N1‑N3 | Resumen opcional de contexto. |
| `max_previous_lines` | integer | N1‑N3 | Líneas máximas de feedback previo. |
| `max_rules_in_prompt` | integer | N1‑N3 | Reglas máximas en prompts. |
| `evaluation` | object | N1‑N3 | Evaluación opcional de misión mediante LLM. |
| `planning_strategy` | string | N2‑N3 | Estrategia de planificación: `static`, `dynamic`, `hybrid`. Por defecto: `hybrid`. |

### 9.3 Ejemplo mínimo N1

```yaml
role: "health-monitor"
vcv:
  - "health_check:v1"
allowed_tools:
  - "http_request"
inference:
  mode: "local_first"
  endpoint: "http://localhost:11434/api/generate"
  model: "llama3.2:latest"
run_mode: "once"
```

### 9.4 Success criteria

Dentro de `03_mission.yaml` y de `plan.steps`, se puede declarar un criterio de éxito.

```yaml
success_criteria:
  type: "output_exists"
  path: "./reports/informe_final.md"
```

o

```yaml
success_criteria:
  type: "tool_success"
  tool: "csv_to_markdown"
```

Si no se define `success_criteria`, el fallo de misión o tarea no se infiere automáticamente.

### 9.5 Evaluación de misión con LLM

Opcionalmente, en `01_agent_spec/agent_spec.yaml`:

```yaml
evaluation:
  llm_mission_check: true
  llm_mission_prompt: |
    Given the following mission and observed results, determine if the mission was accomplished.
    Respond only YES or NO.
```

Si esta evaluación devuelve `NO`, se registra fallo de misión.

### 9.6 Estrategia de planificación

El campo `planning_strategy` determina cómo el agente aborda la generación de planes:

- `static`: usa únicamente el plan definido en `plan`.
- `dynamic`: usa `current_plan.md` y `plan_context.yaml` para generar un plan con el `PlanGenerator`.
- `hybrid`: combina ambos; si no hay plan estático, recurre al dinámico.

Si no se especifica, el runtime usará `hybrid` por defecto.

---

## 10. Manifiesto del agente (`00_manifest.yaml`)

### 10.1 Reglas

- `framework` debe ser `AOF-S v1.6`.
- `level` debe coincidir con el nivel real del agente.
- `profile` debe ser uno de `standalone`, `edge`, `enterprise`.

### 10.2 Imports

```yaml
imports:
  - path: "../packages/health_check"
    include: ["06_skills", "07_world_model"]
    conflict_policy: "merge"
```

- `path`: relativa al directorio del agente o absoluta.
- `include`: rangos a importar.
- `conflict_policy`: `merge`, `override`, `skip`.

---

## 11. Paquetes AOF e imports

Un paquete AOF es un directorio que contiene uno o más rangos AOF‑S.

Puede contener:

- `06_skills/`
- `07_world_model/`
- `11_values/`
- Otros rangos válidos.

Reglas de importación:

- Solo se importan los rangos listados en `include`.
- Si no se especifica `include`, se importa todo el paquete.
- Los conflictos se resuelven según `conflict_policy`.
- El paquete no puede alterar `00_manifest.yaml` ni `01_agent_spec/`.

---

## 12. Gestión de secretos

Los secretos nunca se almacenan en texto plano en `.aof/`.

### 12.1 Sintaxis

| Referencia | Descripción |
|------------|-------------|
| `${env:VARIABLE_NAME}` | Variable de entorno. |
| `${secret:path/to/secret}` | Secreto gestionado por el perfil. |

### 12.2 Restricción opcional

En `01_agent_spec/agent_spec.yaml` puede definirse:

```yaml
security:
  allowed_env_vars:
    - "API_TOKEN"
    - "MONITORING_TOKEN"
```

Si se define, el runtime solo resolverá `${env:...}` para esas variables.

---

## 13. Carga progresiva de contexto

### 13.1 Regla general

El agente solo carga los artefactos de la fase actual y de su nivel.

### 13.2 Tabla normativa

| Fase | Artefactos cargados |
|------|---------------------|
| startup | `00_manifest.yaml`, `01_agent_spec/agent_spec.yaml`, `02_context_static.md`, `03_mission.yaml`, `07_world_model/state.md`, `11_values/constitution.md` |
| reason | episódicos top‑N, políticas procedurales, variables, hechos del dominio, `04_tasks/current_plan.md`, `04_tasks/plan_context.yaml` |
| hypothesize | grafo causal, delays, corpus experto, skill de simulación |
| verify | reglas de validación, contratos semánticos, políticas de seguridad |
| execute | `04_tasks/current_plan.md`, `04_tasks/plan_context.yaml`, skills activas |
| evaluate | `10_observations/feedback.yaml`, `10_observations/anomalies.md` |
| reflect | hipótesis, reglas semánticas, políticas, meta‑estrategias |

---

## 14. Puerta de Evidencia

Definida en `asa.yaml`. No es un artefacto AOF‑S, pero afecta a cuándo se escriben hipótesis y reflexiones.

Si `total_errors == 0`, el runtime omite `hypothesize` y `reflect`. Por tanto, los autores de artefactos no deben asumir que esas fases siempre se ejecutan.

`total_errors` incluye errores operativos, fallos de tarea y fallo de misión.

---

## 15. Perfiles de despliegue

Los perfiles son definidos por `noosystem.yaml`. AOF‑S solo usa `profile` en `00_manifest.yaml` para determinar la resolución de secretos y las dependencias permitidas.

| Perfil | Resolución de secretos |
|--------|------------------------|
| `standalone` | Variables de entorno / `.env` |
| `edge` | Archivo cifrado Age/SOPS |
| `enterprise` | Vault / AWS Secrets Manager / SPIFFE |

---

## 16. Protocolos e interoperabilidad

AOF‑S no define protocolos propietarios. Se apoya en:

- **A2A** para comunicación y descubrimiento.
- **MCP** para herramientas externas.
- **VCV** para descubrimiento semántico versionado.

La integración se declara en skills y en `12_ASA.yaml`.

---

## 17. Registro de agentes

El `registry.yaml` reside en `11_values/` y es mantenido por el N3.

Campos:

- `name`
- `level`
- `status`
- `identity`
- `trust_score`
- `vcv`
- `capabilities`
- `heartbeat_interval`
- `last_heartbeat`
- `agent_card_path`
- `endpoint`

---

## 18. Plantillas Spec‑Driven

### 18.1 Separación

Las plantillas se definen en `aof-s.templates.yaml`, no en `aof-s.yaml`.

### 18.2 Reglas

- Solo el N3 las carga.
- Usan placeholders `{{ ... }}`.
- Los `context_defaults` se heredan por nivel con `extends`.
- Los valores del plan JSON prevalecen sobre `context_defaults`.

### 18.3 Placeholders de runtime

| Placeholder | Origen |
|-------------|--------|
| `{{ creation_date }}` | Runtime |
| `{{ instance_id }}` | Runtime |
| `{{ skill_body }}` | Skill |
| `{{ inference_info }}` | context_defaults |

---

## 19. Validación y conformidad

Un agente es conforme a AOF‑S v1.6 si:

- Cumple la estructura de su nivel.
- Todos los artefactos obligatorios existen y tienen el formato correcto.
- Los rangos y rutas son correctos.
- Los secretos no están en texto plano.
- Las skills cumplen la sintaxis de `SKILL.md`.
- El `framework` en `00_manifest.yaml` es `AOF-S v1.6`.
- Los ejemplos en las especificaciones usan placeholders, no valores literales.
- `plan_context.yaml` existe en N2 y N3 si la estrategia lo requiere.

La validación puede realizarse con `validate_aof` en runtime N3 o con futuras versiones de `noctl validate`.

---

## 20. Evolución del estándar

El estándar evoluciona mediante ADRs. Se aplica versionado semántico.

Cambios compatibles: adición de campos opcionales.

Cambios incompatibles: reubicación de rangos, renumeración, eliminación de artefactos.

---

## 21. Anexos

### 21.1 Herramientas nativas canónicas

Véase `asa.yaml` v1.6 para la lista completa.

### 21.2 Reglas de nomenclatura

- Directorios y archivos de rango: minúsculas, snake_case.
- Skills: minúsculas con guiones.
- Timestamps: `%Y%m%d_%H%M%SZ`.

### 21.3 ADR relacionada

- ADR‑0016 — Uso de placeholders en ejemplos de especificaciones.

---

**Autores:** Jorge Y. Hernández García (ETKinnova), DeepSeek como Arquitecto Integral del Ecosistema.