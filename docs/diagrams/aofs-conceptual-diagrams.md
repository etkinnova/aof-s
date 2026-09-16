# Diagramas Conceptuales — AOF‑S

**Versión:** 1.6  
**Fecha:** 29 de agosto de 2026  
**Documento conceptual asociado:** `aofs-conceptual_es.md` v1.6  
**Naturaleza:** Ilustrativo. Ante cualquier discrepancia, prevalece `aof-s.yaml` v1.6.

---

## 1. Taxonomía de rangos cognitivos

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, sección 4.2 — 29 de agosto de 2026

```mermaid
graph LR
    R00[00 Identidad] --> Q00["¿Quién soy?"]
    R01[01 Especificación del agente] --> Q01["¿Qué capacidades tengo?"]
    R02[02 Contexto estático] --> Q02["¿En qué entorno fijo opero?"]
    R03[03 Misión] --> Q03["¿Qué debo lograr?"]
    R04[04 Tareas y planificación] --> Q04["¿Qué voy a hacer?"]
    R05[05 Hipótesis y predicciones] --> Q05["¿Qué espero que ocurra cuando actúe?"]
    R06[06 Skills] --> Q06["¿Qué sé hacer?"]
    R07[07 Modelo del mundo] --> Q07["¿Cómo es el mundo que percibo?"]
    R08[08 Memoria] --> Q08["¿Qué he aprendido?"]
    R09[09 Validación] --> Q09["¿Cómo verifico que cumplo las reglas?"]
    R10[10 Observaciones] --> Q10["¿Qué está ocurriendo?"]
    R11[11 Valores y gobernanza] --> Q11["¿Bajo qué principios y con quién gobierno?"]
    R12[12 Identidad noosistémica] --> Q12["¿Cómo me comunico con otros agentes?"]
```

Cada rango agrupa artefactos que responden a una misma pregunta cognitiva. El orden es conceptual, no secuencial.

---

## 2. Estructura canónica del directorio `.aof/` por niveles

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, secciones 5 y 6 — 29 de agosto de 2026

```mermaid
graph TD
    subgraph N1
        N1A[00_manifest.yaml]
        N1B[01_agent_spec/agent_spec.yaml]
        N1C[02_context_static.md]
        N1D[03_mission.yaml]
        N1E[04_tasks/current_plan.md]
        N1F[06_skills/]
        N1G[09_validation_spec.md]
        N1H[10_observations/feedback.yaml]
    end

    subgraph N2
        N2A[Añade:]
        N2B[05_hypotheses/active.md]
        N2C[07_world_model/]
        N2D[08_memory/]
        N2E[10_observations/anomalies.md]
        N2F[11_values/]
    end

    subgraph N3
        N3A[Añade:]
        N3B[05_hypotheses/active/validated/falsified]
        N3C[07_world_model/expert_corpus/]
        N3D[08_memory/meta/]
        N3E[11_values/registry.yaml]
        N3F[12_ASA.yaml]
    end
```

N1 proporciona la base. N2 añade hipótesis, mundo, memoria y gobernanza local. N3 completa el modelo con orquestación y comunicación noosistémica.

---

## 3. Estructura de una skill

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, sección 8 — 29 de agosto de 2026

```mermaid
graph TD
    SK[06_skills/health-check/] --> SKILL[SKILL.md]
    SK --> SCRIPTS[scripts/]
    SK --> REFS[references/]
    SK --> ASSETS[assets/]
    SK --> TEMPLATES[templates/]
    SK --> ENDPOINTS[endpoints.yaml]
```

`SKILL.md` es el único archivo obligatorio. Los demás son opcionales. La skill es la unidad reutilizable de capacidad compatible con `agentskills.io`.

---

## 4. Especificación del agente y subartefactos

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, sección 9 — 29 de agosto de 2026

```mermaid
graph TD
    SPEC[01_agent_spec/agent_spec.yaml] --> ROLE[role]
    SPEC --> VCV[vcv]
    SPEC --> TOOLS[allowed_tools]
    SPEC --> INF[inference]
    SPEC --> MODE[mode]
    SPEC --> HYP[hypothesis]
    SPEC --> ROUTE[llm_routing]
    SPEC --> TRUST[trust_score_config]
    SPEC --> PLAN[plan]
    SPEC --> HB[heartbeat_*]
    SPEC --> EVAL[evaluation]
```

El contrato central agrupa capacidades, restricciones, inferencia, planificación y confianza. En el futuro, subartefactos opcionales podrán residir en el mismo directorio.

---

## 5. Carga progresiva de contexto

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, sección 13 — 29 de agosto de 2026

```mermaid
graph LR
    START[startup] --> MAN[00_manifest]
    START --> SPEC[01_agent_spec]
    START --> CTX[02_context_static]
    START --> MISS[03_mission]
    START --> STATE[07_world_model/state]
    START --> CONST[11_values/constitution]
```

Solo se cargan los artefactos necesarios para la fase actual. La carga progresiva reduce el consumo de tokens y mantiene el foco cognitivo.

---

## 6. Importación de AOF Packages

> Referencia conceptual: `aofs-conceptual_es.md` v1.6, sección 11 — 29 de agosto de 2026

```mermaid
graph TD
    AGENT[Agente .aof/] -->|imports en 00_manifest.yaml| PKG[AOF Package]
    PKG --> S[06_skills/]
    PKG --> W[07_world_model/expert_corpus/]
    PKG --> V[11_values/]
    AGENT -.->|include| S
    AGENT -.->|include| W
    AGENT -.->|include| V
```

Un paquete reutiliza rangos completos mediante `imports`, con políticas de conflicto `merge`, `override` o `skip`. No puede modificar identidad ni especificación del agente.

---

**Documento ilustrativo.** Para dudas normativas, consultar `aof-s.yaml` v1.6.