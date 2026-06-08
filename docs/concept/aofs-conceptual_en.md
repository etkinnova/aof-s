# AOF‑S: AI Orchestration Files for Systemic Agents — Conceptual Document

**Purpose:** This document develops the conceptual foundations of the AOF‑S (AI Orchestration Files for Systemic Agents) standard, the third pillar of the Noosystem. It complements the executable specification (`spec/aof-s.yaml`) with the narrative that explains the "why" and the "how" of its design as an open standard for the packaging, governance, and orchestration of ASA agents.

**Version:** 1.0  
**Date:** June 8, 2026  
**Part of the standard:** AOF‑S v1.0

---

## Table of Contents

1. [Definition and Foundations](#1-definition-and-foundations)
2. [Design Principles](#2-design-principles)
3. [File Structure and Levels](#3-file-structure-and-levels)
4. [Noosfile and Declarative Orchestration](#4-noosfile-and-declarative-orchestration)
5. [NSL Semantic Layer](#5-nsl-semantic-layer)
6. [Declarative Zero Trust Security](#6-declarative-zero-trust-security)
7. [Local‑First Principle in AOF‑S](#7-local-first-principle-in-aof-s)
8. [Sectorial Configurability](#8-sectorial-configurability)
9. [Relationship with ASA and the Noosystem](#9-relationship-with-asa-and-the-noosystem)

---

## 1. Definition and Foundations

### 1.1 Canonical Definition

**AOF‑S (AI Orchestration Files for Systemic Agents)** is an open standard of versionable files (Markdown + YAML) that materializes the cognitive artifacts of the ASA Model. It defines how an AI agent is packaged, governed, and orchestrated, applying the principles of **Spec‑Driven Development (SDD)** and **Local‑First** to the agent ecosystem. AOF‑S is not another static configuration format; it is the "operating system" and the "Dockerfile" of the agent: a set of executable specifications that the agent reads and writes throughout its lifecycle, optimizing token consumption through progressive loading and ensuring complete traceability of its decisions.

AOF‑S is the third pillar of the Noosystem triangle: the Noosystem defines the ecosystem where agents collaborate; ASA defines the architecture of each agent; AOF‑S defines the standard that materializes ASA and governs the Noosystem.

### 1.2 The Problem AOF‑S Solves

In 2026, the AI agent ecosystem is fragmented. Each framework (LangChain, CrewAI, AutoGen), each tool (Claude Code, Cursor, Copilot), and each project defines its own way of providing context to agents: `AGENTS.md`, `prompts.txt`, proprietary configurations. This fragmentation generates three fundamental problems:

1. **Agent amnesia:** Context files are static. The agent reads them at startup but does not write to them. Its learning is lost between sessions. There is no persistent memory or decision traceability.

2. **Vendor lock‑in:** Each platform defines its own structures. An agent designed for LangChain cannot be easily migrated to CrewAI. There is no portable format that any LLM can read and write.

3. **Lack of governance:** There is no standard mechanism for declaring security policies, evaluating agent performance, or verifying that its actions comply with rules before execution. Governance is added after the fact, if at all.

AOF‑S solves these three problems through an open, versionable, and Spec‑Driven standard that any LLM can adopt.

### 1.3 Spec‑Driven Development (SDD) Applied to Agents

AOF‑S is grounded in the **Spec‑Driven Development (SDD)** paradigm, which inverts the traditional workflow: the specification is the source of truth, and the agent's behavior is an artifact verified against that specification.

In AOF‑S, each file is an **executable specification** that the agent must comply with:

| AOF‑S Artifact | Specification it Defines |
|----------------|--------------------------|
| `Noosfile.yaml` | The desired state of the ecosystem (agents, policies, security, evaluation). |
| `ASA.yaml` | The identity, capabilities, and semantic profile of an agent. |
| `mission.yaml` | The business need to satisfy, with constraints and success metrics. |
| `hypothesis.md` | The planned intervention with higher‑order effects and validation metrics. |
| `causal_graph.proto` | The causal model of the system, with updatable probability distributions. |
| `constitution.md` | The inviolable principles of algorithmic governance. |

The ASA cycle continuously verifies that the agent's behavior conforms to its specification. If an action lacks a registered hypothesis, it is blocked. If the result deviates from the prediction, the model is updated. AOF‑S transforms the specification from a descriptive document into an executable contract.

### 1.4 AOF‑S as a Living Format

Unlike a static `AGENTS.md`, AOF‑S artifacts are **read and written by the agent** as part of its operational cycle:

- **Before acting:** The agent writes `hypothesis.md` with its predictions.
- **After acting:** The agent writes `observations/feedback_log.yaml` with the results.
- **After reflecting:** The agent updates `learned_rules.yaml` with new semantic rules and `policies.yaml` with new procedural policies.
- **Periodically:** The agent updates `causal_graph.proto` with refined probability distributions.

This makes AOF‑S a **living format** that evolves with the agent's experience, maintaining a versionable and auditable record of all learning.

## 2. Design Principles

The eight design principles of AOF‑S are not arbitrary conventions or generic best practices. They are the direct response to the limitations identified in current AI agent approaches: amnesia between sessions, format fragmentation, cloud dependency, lack of algorithmic governance, and the absence of a continuous learning model. Each principle addresses a specific dimension of the problem, and together they define the identity of the standard.

### Principle 0: Local‑First by Design

Local execution is the default path for every AOF‑S agent. The cloud is explicitly degraded to an optional fallback, configurable on demand.

**What it means in practice:**
- AOF‑S artifacts reside on the local file system of the device where the agent runs. No internet connectivity is required for the agent to operate.
- The `inference` section in `ASA.yaml` declares local inference engines (Ollama, llama.cpp, vLLM) with priorities and assigned models. The cloud fallback is explicitly configured and only activates under defined conditions (resource saturation, tasks requiring a model not available locally).
- A Reactive (N1) agent can run on a Raspberry Pi with a 3B parameter quantized model. An Orchestrated (N3) agent may require an 18GB+ VRAM GPU. In both cases, the cloud is optional.

**Why it is necessary:**
The agent industry in 2026 is converging toward local execution for three reasons: data sovereignty (sensitive context never leaves the device), elimination of per‑token costs (critical for long‑running autonomous agents), and offline operation (essential for edge computing, hospital environments, or isolated critical infrastructure). Projects like the Enterprise Local‑First Gateway (ELG), the Cognitive Loop Kernel (CLK), and SuperLocalMemory validate this trend.

### Principle 1: Cognitive Separation

Each file in the AOF‑S structure answers a precise cognitive question. The agent architecture is not defined in a monolith but in a set of specialized artifacts that can be read, written, and versioned independently.

**What it means in practice:**
- `00_manifest.yaml` answers "Who am I and at what level do I operate?"
- `01_context_static.md` answers "What is the stack, commands, and immutable rules of my project?"
- `02_mission.yaml` answers "What need must I satisfy and how is success measured?"
- `hypothesis.md` answers "What will I do, what do I expect to happen, and how will I validate it?"
- `learned_rules.yaml` answers "What have I learned from past experiences?"

**Why it is necessary:**
Cognitive separation enables progressive loading (Principle 2), facilitates auditing (each decision is linked to a specific artifact), and allows different ecosystem components to be updated without affecting others. A change in the mission does not require rewriting the world model; a new semantic rule does not force changes to the constitution.

### Principle 2: Progressive Loading

The agent does not load all AOF‑S artifacts at the start of a session. It selects only the files needed for the current phase of the ASA cycle, optimizing token consumption and keeping the context window under control.

**What it means in practice:**
The progressive loading matrix, documented in `spec/aof-s.yaml`, defines which artifacts are loaded in each phase:

| Phase | Loaded Artifacts | Approx. Tokens |
|-------|------------------|----------------|
| **Startup** | `manifest.yaml`, `context_static.md`, `mission.yaml`, `constitution.md` | ~2‑5k |
| **Reason** | `episodic/` (top‑N), `policies.yaml`, `variables.yaml` | +2‑3k |
| **Hypothesize** | `causal_graph.proto`, `delays.yaml`, `expert_corpus/` (RAG), `simulation_skill.md` | +3‑5k |
| **Verify (N3)** | `verification_rules.clp`, semantic contracts, OPA policies | +1‑2k |
| **Execute** | `current_steps.md`, specific skills | variable |
| **Evaluate** | `feedback_log.yaml`, `anomaly_notes.md` | +1k |
| **Reflect** | Active hypothesis, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml` (N3) | +2‑3k |

**Why it is necessary:**
The context window of LLMs is limited and costly. Loading all memory and world model on every cycle would saturate the window and degrade performance. Progressive loading allows an Orchestrated (N3) agent to maintain a peak of ~16k tokens, well below the 200k limit of current models, without sacrificing analytical depth.

### Principle 3: Reflexive Causal Intervention

Every agent action is a hypothesis to be verified. The agent does not merely execute tasks; it predicts the effects of its interventions, contrasts them with reality, and updates its world model based on the discrepancy.

**What it means in practice:**
- Before acting, the agent writes `hypothesis.md` with quantitative predictions of 1st, 2nd, and 3rd order effects, validation metrics, and success criteria.
- After acting, the agent writes `observations/feedback_log.yaml` with the actual results and compares them to the hypothesis.
- In the reflection phase, the agent updates the Probabilistic Causal Graph (probability distributions), extracts semantic rules (`learned_rules.yaml`), and refines procedural policies (`policies.yaml`).
- If an action lacks a registered hypothesis, the pre‑action guardrail blocks it.

**Why it is necessary:**
This principle is the antithesis of "vibe coding" applied to agents. Without hypotheses, there is no traceability. Without traceability, there is no learning. Without learning, the agent is condemned to repeat its mistakes. Reflexive causal intervention closes the loop of algorithmic governance (Fabric T1).

### Principle 4: Purposeful Cognitive Memory

The agent's memory is not a flat log or an undifferentiated vector store. It is organized into functional types —episodic, semantic, procedural, and meta‑cognitive— inspired by human cognitive architecture (ACT‑R), with mathematical mechanisms of forgetting, strengthening, and consolidation.

**What it means in practice:**
- **Episodic memory:** Chronological record of interventions with full context. Queried during the Reason phase to retrieve similar experiences.
- **Semantic memory:** Rules and facts extracted from experience. Each rule includes lifecycle fields (`confidence`, `last_accessed`, `access_count`, `decay_rate`) that allow forgetting through disuse and strengthening through use.
- **Procedural memory:** Automated action policies ("if condition then action"). Equivalent to Kahneman's System 1: fast, low‑cognitive‑cost responses.
- **Meta‑cognitive memory (N3):** Historical performance of reasoning strategies. Allows the agent to learn to reason better, dynamically selecting the most suitable strategy for each task.

**Why it is necessary:**
Flat memory does not scale. An agent accumulating thousands of episodes without forgetting mechanisms becomes increasingly slow and inefficient. Purposeful cognitive memory ensures that the agent remembers what is important, forgets what is irrelevant, and consolidates patterns into generalizable knowledge.

### Principle 5: Zero Trust Security by Construction

Security is not an after‑the‑fact layer. It is built into the standard's design from the very beginning, with verifiable identities (SPIFFE), end‑to‑end encryption (mTLS), and declarative access policies (OPA) applied progressively according to the agency level.

**What it means in practice:**
- Each agent receives a unique SPIFFE identity (`spiffe://noos.domain/agent/name`) declared in `ASA.yaml`.
- All inter‑agent communication uses mTLS managed by the service mesh (Istio).
- Access policies are declared in `Noosfile.yaml` using OPA and evaluated in real time.
- Security is progressive: minimal at N1 (OS‑level sandboxing), automatic at N2 (auto‑configured SPIFFE and mTLS), and mandatory at N3 (SPIFFE, mTLS, and OPA required).

**Why it is necessary:**
In an ecosystem where agents can create other agents, share memory, and execute autonomous actions, security cannot be optional. The history of vulnerabilities in agent platforms (such as the CVSS 9.9 of OpenClaw) demonstrates that security must be a design pillar, not a subsequent patch.

### Principle 6: Integrated Evaluability

Agent performance evaluation is not an external audit performed sporadically. It is a capability integrated into the agent's lifecycle, with metrics declared in its specification, standard benchmarks, and continuous evaluation commands.

**What it means in practice:**
- Each agent declares its evaluation metrics in `ASA.yaml` and `03_agent_spec.yaml`: hypothesis precision, falsification rate, latency, token consumption.
- The `noctl evaluate` command executes evaluation on demand, with support for external benchmarks (MCP‑AgentBench).
- Evaluation is progressive: basic metrics at N1 (success rate, execution time), hypothesis metrics at N2 (precision, falsification), ecosystem benchmarks at N3.

**Why it is necessary:**
An agent that does not evaluate itself cannot improve. Integrated evaluability closes the learning cycle: the agent formulates hypotheses, contrasts them with reality, and measures its own performance. This enables continuous improvement without constant human intervention.

### Principle 7: Progressive Agency

Complexity is activated only when needed. An agent is not born complex; it evolves from a minimal configuration to a full ecosystem orchestrator, adding capabilities on demand.

**What it means in practice:**
- **Ultra‑light mode (N1):** 2 files, 5 minutes to start. No memory, no world model, no complex security.
- **Full N1:** 9 files. Adds mission, agent specification, skills, and validation.
- **N2:** 16 files. Adds tricameral memory, world model, hypothesis, and reflection.
- **N3:** 30+ files. Adds quadruple memory, meta‑cognition, neuro‑symbolic verification, orchestration, and governance.
- The `noctl upgrade --level <n>` CLI declaratively manages progression, adding the necessary artifacts without breaking the existing configuration.

**Why it is necessary:**
Progressive agency removes the barrier to entry. A developer who only needs a reactive agent for health checks should not be forced to configure a Probabilistic Causal Graph or OPA policies. Complexity is introduced when the ecosystem requires it, and not before.

## 3. File Structure and Levels

The most visible materialization of AOF‑S is its file structure. An AOF‑S agent is neither monolithic code nor a gigantic prompt; it is a `.aof/` directory composed of specialized artifacts that the agent reads, writes, and updates throughout its lifecycle. This structure is organized into three levels of increasing complexity, in compliance with the principle of progressive agency.

### 3.1 The Three Levels of Complexity

| Level | Name | Files | Initial Tokens | Memory | World Model | Learns Alone? | Orchestrates? |
|-------|------|-------|----------------|--------|-------------|---------------|---------------|
| **1** | **Reactive** | 2‑9 | ~2‑4k | No (its evaluations feed collective memory) | No | No (systemic closure) | No |
| **2** | **Systemic** | ~16 | ~4‑7k | Episodic + Semantic + Procedural | Variables, state, Causal Graph (1st/2nd order) | Yes (locally) | No |
| **3** | **Orchestrated** | 30+ | ~7‑12k | Quadruple (+ Meta‑cognitive) | Full Probabilistic Causal Graph (1st/2nd/3rd order) | Yes (locally and systemically) | Yes, the ecosystem |

### 3.2 Ultra‑Light Mode: Agents in 5 Minutes

Ultra‑light mode materializes the promise of progressive agency: a functional agent in minutes, without sacrificing future scalability. It is the AOF‑S equivalent of a "Hello World" in programming.

**Initialization:**
```bash
noctl init --level 1 --ultra-light
```

**Resulting structure:**
```
.aof/
├── 00_manifest.yaml          # Agent identity and level
└── 01_context_static.md      # Project stack, commands, and rules
```

**Full Level 1 (Reactive) Artifacts:**
```
.aof/
├── 00_manifest.yaml          # Agent metadata (framework, level, project)
├── 01_context_static.md      # Immutable project information (stack, commands, rules)
├── 02_mission.yaml           # What need the agent must solve and how success is measured
├── 03_agent_spec.yaml        # Role, tools, model, constraints, and evaluation
├── 04_tasks/
│   └── current_plan.md       # Current action plan
├── 05_skills/                # Modular skills (testing, deployment…)
├── 06_validation_spec.md     # Acceptance criteria
└── 07_observations.log       # Metrics and events log
```

**Artifacts Added at Level 2 (Systemic):**
```
.aof/
├── 04_tasks/
│   ├── hypothesis.md         # Intervention hypothesis with 1st/2nd order effects
│   └── steps.md              # Steps derived from the hypothesis
├── 05_world_model/           # World model (variables, state)
├── 07_memory/                # Episodic and procedural memory
├── 09_observations/          # Structured metrics and anomalies
└── 10_values/                # Constitution and preferences
```

**Artifacts Added at Level 3 (Orchestrated):**
```
.aof/
├── ASA.yaml                  # Full noosystemic agent identity
├── 04_knowledge/             # Ecosystem knowledge
│   ├── world_model/          # Causal Graph, delays, global state
│   ├── expert_corpus/        # Indexed knowledge corpus for RAG
│   ├── registry.yaml         # Ecosystem connective tissue
│   └── hypotheses/           # Complete hypothesis history
├── 05_skills/                # Advanced skills: simulation, causal mapping, RAG
├── 06_memory/                # Quadruple memory (includes meta‑cognitive)
└── 08_observations/          # Logs and anomalies with full traceability
```

### 3.3 Progressive Loading: The Art of Efficiency

Progressive loading is the mechanism that makes the AOF‑S structure viable. The agent does not read all files at the start of its cycle. It selects only the artifacts needed for the current phase, keeping token consumption under control.

**Progressive Loading Matrix:**

| ASA Cycle Phase | Loaded Artifacts | Approx. Tokens (N3) |
| :--- | :--- | :--- |
| **Startup** | `00_manifest.yaml`, `01_context_static.md`, `02_mission.yaml`, `ASA.yaml`, `constitution.md` | ~3-5k |
| **Reason** | `episodic/` (top‑N), `policies.yaml`, `state.md`, `variables.yaml`, `domain_facts.md` | +2-3k |
| **Hypothesize** | `causal_graph.proto`, `delays.yaml`, `expert_corpus/` (RAG), `simulation_skill.md` | +3-5k |
| **Verify (N3)** | `verification_rules.clp`, semantic contracts (JSON Schema), OPA policies | +1-2k |
| **Execute** | `current_steps.md`, specific skills | variable |
| **Evaluate** | `feedback_log.yaml`, `anomaly_notes.md` | +1k |
| **Reflect** | Active hypothesis, `learned_rules.yaml`, `policies.yaml`, `meta_strategies.yaml` (N3) | +2-3k |

**Token trace example for an N3 (Orchestrated) agent:**
1.  **Startup:** Loads 5 files (3,200 tokens). The agent already knows who it is, where it is, and what it must do.
2.  **Reason:** Retrieves 3 similar episodes and 2 procedural policies (2,800 additional tokens).
3.  **Hypothesize:** Loads the Causal Graph and executes RAG over the expert corpus (4,500 additional tokens). Total peak: **10,500 tokens**.
4.  **Verify:** The neuro‑symbolic engine validates the hypothesis (1,200 additional tokens).
5.  **Execute:** The agent acts with a minimal context of the steps plan and the required skill.
6.  **Evaluate:** Reads post‑intervention metrics (800 tokens).
7.  **Reflect:** Updates the model and memories (2,500 tokens).

The maximum consumption peak is maintained around 12‑16k tokens during the hypothesize phase, leaving ample margin within the 200k token window of a modern LLM. This contrasts with the approach of loading a static 40KB `AGENTS.md`, which already consumes a significant portion of the window before the agent has even started reasoning.

### 3.4 Progressive Agency in Practice

The transition between levels is a declarative process managed by `noctl`:

```bash
# From an ultra‑light N1 to a full N1
noctl upgrade --level 1

# From a full N1 to a systemic N2 (with memory and world model)
noctl upgrade --level 2

# From an N2 to an orchestrated N3 (with meta‑cognition and governance)
noctl upgrade --level 3
```

Each command adds the artifacts corresponding to the new level without deleting existing ones or breaking the previous configuration. The agent evolves organically.

## 4. Noosfile and Declarative Orchestration

AOF‑S not only defines the internal structure of each agent. It also defines how the entire ecosystem is governed. The **Noosfile** is the declarative manifest that describes the desired state of a (sub)Noosystem, and **noctl** is the command‑line tool that applies that manifest, following the Infrastructure as Code (IaC) pattern popularized by Docker Compose and Kubernetes.

### 4.1 The Noosfile as the Source of Truth

The `Noosfile.yaml` is the Noosystem equivalent of a `docker-compose.yml` or a `kustomization.yaml`: a versionable file that declares which agents should exist, with what configuration, under what security policies, and with what evaluation criteria. It does not describe *how* to achieve that state, but *what* state is desired. The orchestrator (an N3 agent or the future Noosystemic Operator) is responsible for materializing it.

**Structure of a Noosfile:**

```yaml
apiVersion: "noos.cncf.io/v1alpha2"
kind: "Noosystem"
metadata:
  name: "ecommerce-ops"
  namespace: "noos://ecommerce"
spec:
  mission: "Guarantee checkout SLO (p95 < 500ms, 99.95% uptime)"
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

Each section of the Noosfile addresses a dimension of ecosystem governance:

| Section | Purpose |
|---------|---------|
| `agents` | Declares the agents that must exist, their level, scaling, and resources. |
| `security` | Defines Zero Trust security policies (SPIFFE, mTLS, OPA). |
| `shared_services` | Declares common services (vector database, message broker, knowledge graph). |
| `global_policies` | Establishes trust thresholds, circuit breakers, and restart policies. |
| `evaluation` | Configures the ecosystem's evaluation benchmarks and metrics. |
| `inference` | Defines the Local‑First inference strategy of the ecosystem. |

### 4.2 `noctl`: The Noosystem Governance CLI

`noctl` is the tool that materializes declarative orchestration. Inspired by `kubectl`, `docker`, and `git`, it allows human operators and N3 agents themselves to govern the full lifecycle of the ecosystem.

**Main commands:**

| Command | Function |
|---------|----------|
| `noctl init` | Initializes a new Noosystem with an agency level and sectorial configuration. |
| `noctl apply -f <noosfile>` | Applies a Noosfile manifest to the ecosystem, creating, updating, or removing agents. |
| `noctl upgrade --level <n>` | Evolves an agent or ecosystem to the next agency level. |
| `noctl status` | Displays the current state of the Noosystem (agents, trust scores, SLOs). |
| `noctl evaluate` | Runs evaluation of an agent or the complete ecosystem. |
| `noctl security scan` | Scans the ecosystem for OWASP vulnerabilities. |
| `noctl diagnose` | Runs a health diagnosis of the ecosystem. |
| `noctl memory compact` | Executes autonomous context compression. |

**Workflow example with `noctl`:**
```bash
# Initialize an ecosystem for a hospital
noctl init --level 2 --sector hospital

# Apply a custom Noosfile
noctl apply -f noosfile.yaml

# Check status
noctl status

# Evaluate the ecosystem
noctl evaluate ecosystem

# Scale to level 3 when advanced orchestration is needed
noctl upgrade --level 3
```

### 4.3 Adaptive Orchestration: The Noosystemic Operator

For Level 3 (Swarm) ecosystems, AOF‑S foresees the figure of the **Noosystemic Operator**: a native Kubernetes controller that observes the cluster state, detects deviations from the Noosfile, and applies the desired state autonomously, managing the full lifecycle of the agents. This operator follows the same pattern as Kubernetes operators: it observes custom resources (`Noosystem`), compares the actual state with the desired state, and executes the necessary actions to converge.

The Noosystemic Operator will be the definitive implementation of the adaptive orchestration principle, enabling an agent ecosystem to self‑manage, self‑scale, and self‑repair without constant human intervention.

## 5. NSL Semantic Layer

Communication between agents is not just a problem of transport or syntax. Two agents can exchange perfectly formed messages and still not understand each other because they do not share the meaning of the concepts. This is the semantic gap that the **Noosystem Semantic Layer (NSL)** comes to close. The NSL is not a new network protocol; it is a value‑added extension that operates over existing protocols (A2A, MCP) to give agents the ability to align their meanings, negotiate contracts, and trust each other programmatically.

### 5.1 The Problem of Semantic Isolation

In an open ecosystem, a clinical agent and an energy agent need to collaborate to pre‑cool an operating room without compromising patient safety. Both speak A2A and MCP, so they can exchange tasks. But how does the energy agent know that "setpoint temperature" for the clinical agent means "18‑20°C in the OR" and not "22‑24°C in hospitalization"? How does the clinical agent know that "pre‑cooling" will not affect life‑support equipment? Without a semantic layer, these questions have no automated answer.

### 5.2 NSL Components

#### 5.2.1 Semantic Handshake

Before executing a collaborative task, two agents compare their semantic profiles (declared in `ASA.yaml`) to verify that they share a common understanding of the domain.

**Negotiation flow:**
1. Agent A (orchestrator) sends a task request to Agent B (specialist), including its `semantic_profile`: namespace, bound ontologies, and version.
2. Agent B compares the received profile against its own.
3. If both share at least one ontology for the task domain, the handshake succeeds and the task is executed with the rules of that ontology.
4. If there is no common ontology, the agents may request human intervention, use an external alignment service, or reject the task with a semantic error.

**Example (PCI DSS in e‑commerce):**
```
Agent A (orchestrator):
  semantic_profile:
    namespace: "noos://ecommerce"
    ontology_binding: ["PCI_DSS_v4", "noos://ecommerce/checkout-domain"]

Agent B (db-provisioner):
  semantic_profile:
    namespace: "noos://ecommerce"
    ontology_binding: ["PCI_DSS_v4", "noos://ecommerce/database-domain"]

→ Both share PCI_DSS_v4. Handshake successful.
→ B applies the PCI_DSS_v4 rules that A expects.
```

#### 5.2.2 Programmatic Trust Score

Each agent in the Noosystem has a dynamic `trust_score` (0.0 to 1.0) that reflects its operational reliability. This score is calculated by the N3 orchestrator and stored in `registry.yaml`.

**Calculation factors:**

| Factor | Weight | Description |
|--------|--------|-------------|
| **Hypothesis precision** | 40% | Ratio of validated vs. falsified hypotheses. |
| **Response reliability** | 30% | Success rate in invocations (no errors or timeouts). |
| **SLA compliance** | 15% | Adherence to declared response times. |
| **Peer feedback** | 15% | Evaluations received from other agents after collaborations. |

**Trust thresholds:**
- **≥ 0.90:** High trust. Full autonomy.
- **0.70 – 0.89:** Trusted. Supervision recommended for critical actions.
- **0.50 – 0.69:** Under observation. Read‑only tasks only.
- **< 0.50:** Untrusted. Isolated (status `degraded` or `failed`).

The Trust Score is not a security credential (authentication is provided by SPIFFE), but a metric of operational reliability that allows the orchestrator to route tasks to the most capable agent and detect degraded agents before they affect the ecosystem.

#### 5.2.3 Learned Semantic Contracts

The NSL allows agents to learn from past semantic negotiations. An N2 or N3 agent can store patterns of negotiation success in its semantic memory and reuse them in future interactions.

**Example entry in `learned_contracts.yaml`:**
```yaml
contracts:
  - contract_id: "PCI_DSS_v4_compliance"
    success_rate: 0.96
    preferred_ontology: "PCI_DSS_v4"
    fallback_ontology: "ISO_27001"
    negotiation_strategy: "strict_then_fallback"
    last_negotiated: "2026-06-07T10:00:00Z"
```

#### 5.2.4 NSL‑OSI Bridge

To guarantee interoperability with the broader ecosystem of semantic standards, the NSL includes a **bridge with OSI (Open Semantic Interchange)** , the standard driven by ThoughtSpot and Snowflake. This bridge allows a Noosystem agent to translate its semantic profile to OSI definitions and vice versa, facilitating collaboration with agents external to the ecosystem.

### 5.3 NSL as a Value‑Added Layer

The NSL does not compete with A2A or MCP; it complements them. It travels as metadata in A2A request headers and in MCP tool contexts. It does not impose a new protocol or add unnecessary rigidity to the transport layer. It is simply the layer that allows agents to understand each other, trust each other, and learn to collaborate better.

## 6. Declarative Zero Trust Security

Security in AOF‑S is not implemented through proprietary code or obscure server configurations. It is **declared** in the same YAML files that define the agent and the ecosystem. This declarativity is what allows security to be progressive, auditable, and automatically enforceable by the orchestrator, without the agent developer having to write security logic in their code.

### 6.1 The Security Manifest: The `security` Section of the Noosfile

The entire security posture of a (sub)Noosystem is defined in the `security` section of `Noosfile.yaml`. This section acts as a binding contract that the N3 orchestrator enforces.

**Fragment of a Noosfile with declared security:**
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

With just these lines, the ecosystem activates:
- **Verifiable identity:** All agents must identify with SPIFFE.
- **Channel encryption:** All inter‑agent communication uses mTLS.
- **Granular access control:** The OPA engine blocks any action not explicitly allowed.
- **Execution isolation:** Each agent runs in a gVisor sandbox with a read‑only root filesystem and no external network access.
- **Regulatory compliance:** Alignment with the main security frameworks is documented.

This is the power of declarativity: a configuration of a few lines, versionable in Git, applicable with a single `noctl apply` command, enforced by the orchestrator without each agent needing to implement its own security logic.

### 6.2 How AOF‑S Materializes Security in Artifacts

| Security Concept | AOF‑S Materialization |
|------------------|----------------------|
| **Agent identity** | `security.identity.spiffe_id` field in `ASA.yaml`. Each agent declares its unique identity. |
| **Encryption in transit** | `security.mTLS: true` in the `Noosfile`. The service mesh (Istio) enforces the policy without agent intervention. |
| **Granular authorization** | `security.policies.agent_policies` in the `Noosfile`, expressed in Rego (OPA language). |
| **Isolation** | `security.sandbox` in the `Noosfile`. Defines the sandbox runtime and network/filesystem restrictions. |
| **Auditing** | `compliance.audit.enabled: true` in `ASA.yaml`. Actions are recorded in the ledger with full traceability. |
| **Critical actions** | `security.human_approval_required_for` in `ASA.yaml`. Explicit list of operations requiring human authorization. |

### 6.3 Progressive Security

Faithful to the principle of progressive agency, AOF‑S does not demand the same level of security from a Reactive agent as from an Orchestrator.

| Level | Security Requirements |
|-------|----------------------|
| **N1 (Reactive)** | Minimal security. The agent runs in a sandbox (gVisor). SPIFFE and mTLS can be managed by a transparent sidecar, with no configuration by the developer. No OPA policy declaration required. |
| **N2 (Systemic)** | Automatic security. SPIFFE and mTLS are enabled by default. A default OPA policy is applied (read and execute within its domain). |
| **N3 (Orchestrated)** | Full mandatory security. SPIFFE, mTLS, and OPA are required. Granular policies are applied. Critical actions require explicit human approval. |

This progressiveness ensures that a Reactive agent (ultra‑light mode, 2 files) does not need to configure identities or write OPA policies, while a complex enterprise ecosystem has all the guarantees of Zero Trust security.

### 6.4 Defense Against Memory Poisoning

AOF‑S protects the collective memory of the Noosystem against malicious writes. Since N1 agents publish evaluations to collective memory and N2/N3 agents consume them to learn, a compromised agent could poison the knowledge of the entire ecosystem. To prevent this, AOF‑S implements a **Bayesian defense**:

- **Architectural isolation:** Memories of different agents and domains are logically isolated. An N1 agent cannot write to the semantic memory of an N2.
- **Bayesian trust scoring:** Each write operation to collective memory is evaluated by the orchestrator. If the sender's `trust_score` is low or the operation deviates significantly from historical patterns, it is rejected.
- **Adaptive learning:** The defense system learns to classify legitimate vs. malicious operations using a lightweight Bayesian classifier, without additional LLM calls.

This defense, inspired by the mechanisms of SuperLocalMemory, is documented in the AOF‑S artifacts: the isolation policy in the `Noosfile`, the `trust_score` in `registry.yaml`, and the audit trail in `observations/anomaly_notes.md`.

### 6.5 Alignment with Reference Frameworks

AOF‑S explicitly aligns with the main security frameworks for AI applications, providing a common language and a recognized audit framework:

| Framework | Implementation in AOF‑S |
|-----------|------------------------|
| **OWASP Top 10 for Agentic Applications (2026)** | Documented mitigations for all ten cataloged risks (prompt injection, data poisoning, Denial of Wallet, excessive agency, etc.). Each risk has a specific countermeasure in AOF‑S artifacts. |
| **NIST AI RMF 1.0** | The ASA cycle (Map, Measure, Manage, Govern) implements the four pillars of the risk management framework. AOF‑S artifacts (`hypothesis.md`, `observations/`, `constitution.md`) provide the required traceability. |
| **ITU‑T Y.4814** | The recommendation on Zero Trust access control for IoT platforms is materialized in the combination of SPIFFE + OPA + mTLS declared in the `Noosfile`. |

## 7. Local‑First Principle in AOF‑S

Principle 0 of AOF‑S establishes that local execution is the default path for every agent. This section details how AOF‑S materializes that principle in its file structure, in the declarative configuration of inference, and in the cloud fallback mechanisms.

### 7.1 The `inference` Section in ASA.yaml

Each AOF‑S agent, starting from Level 2, declares its inference configuration in its identity manifest (`ASA.yaml`). This section allows specifying which local engines are available, which models each one runs, with what priority they are selected, and under what conditions cloud fallback is permitted.

**Fragment of `ASA.yaml` with inference configuration:**
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

**Fields of the `inference` section:**

| Field | Description |
|-------|-------------|
| `mode` | Inference strategy. `local_first` means local execution is preferred over the cloud. |
| `local_engines` | List of available local inference engines, each with its associated models and priority. The engine with priority 1 is used by default. |
| `resource_limits` | Hardware restrictions: maximum VRAM the agent can consume. |
| `fallback_to_cloud` | If `false`, the agent will never resort to the cloud, guaranteeing 100% data sovereignty. If `true`, it activates under defined conditions. |

### 7.2 Supported Inference Engines

AOF‑S is compatible with the three main local inference engines in the agent ecosystem in 2026:

| Engine | Optimal Use Case | Key Features |
|--------|------------------|--------------|
| **Ollama** | Rapid development, prototyping, desktop. | Simple installation, curated model library, integrated REST API, cross‑platform support. |
| **llama.cpp** | Edge production, maximum control, CPU. | Extreme portability (compiles in C++), fine quantization control (GGUF), CPU and GPU support, native JSON Schema. |
| **vLLM** | Multi‑user servers, high concurrency. | Up to 19x faster than Ollama under concurrent load, PagedAttention for efficient VRAM management, native tool calling. |

### 7.3 Association of Engines to Agency Levels

The choice of inference engine is not arbitrary; it is guided by the agency level and available resources:

| Level | Recommended Hardware | Default Engine | Example Models |
|-------|---------------------|----------------|----------------|
| **N1 (Reactive)** | CPU, Raspberry Pi 5, laptops. | llama.cpp (CPU) | Llama 3.2 3B (Q4_K_M). |
| **N2 (Systemic)** | GPU 8‑12 GB VRAM, Apple Silicon 16‑32 GB. | Ollama | Qwen 2.5 Coder 7B, Llama 3.1 8B. |
| **N3 (Orchestrated)** | GPU 18 GB+ VRAM, DGX Spark, cluster. | vLLM (under load), Ollama (isolated tasks) | Llama 3.1 70B, Qwen 2.5 32B. |

### 7.4 Cloud Fallback Policy

If the user configures `fallback_to_cloud: true`, the agent may resort to the cloud under controlled conditions. This fallback policy is defined in the same `inference` section:

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

**Fallback triggers:**

| Trigger | Description |
|---------|-------------|
| `local_engine_unavailable` | The local engine (Ollama, llama.cpp) is down for maintenance or error. |
| `resource_exhaustion` | Local VRAM or CPU are at their limit. |
| `task_requires_frontier_model` | The task requires a state‑of‑the‑art model (GPT‑4, Claude Opus) not available locally. |

Even when the fallback is activated, the agent first attempts the local route. Only if this fails due to one of the configured triggers does it resort to the cloud. This approach guarantees that the cloud is always a fallback, never a dependency.

### 7.5 Benefits of the Local‑First Principle in AOF‑S

| Benefit | Description |
|---------|-------------|
| **Data sovereignty** | AOF‑S artifacts (`memory/`, `expert_corpus/`, `observations/`) reside on the local file system. In `local_first` mode with `fallback_to_cloud: false`, 0% of data leaves the device. |
| **Zero per‑token costs** | In local mode, execution generates no billing whatsoever. This is critical for long‑running autonomous agents or ecosystems with many agents. |
| **Offline operation** | Agents can function without internet connectivity, essential for edge computing, hospital environments, or isolated critical infrastructure. |
| **Privacy by default** | Regulatory compliance (HIPAA, PCI DSS, GDPR) is simplified by not transmitting data to third parties. |
| **Provider independence** | The user decides which inference engine to use and whether to activate cloud backup. No vendor lock‑in. |

## 8. Sectorial Configurability

One of the most powerful properties of AOF‑S is its ability to adapt to different sectors without modifying its base architecture. The five layers of the Noosystem, the three ASA agency levels, and the communication protocols remain invariant. What changes is the **library of specialized agents**, the **domain skills**, and the **expert knowledge corpus**. This separation between the generic core and sectorial configuration allows AOF‑S to be a truly universal standard.

### 8.1 Principle of Adaptability

AOF‑S follows the principle of **"invariant architecture, variable configuration"**. This means that:

- **The AOF‑S core is generic:** The agency levels (Reactive, Systemic, Orchestrated), the 7‑phase ASA cycle, the file structure, the progressive loading, and the protocol stack (A2A, MCP, NSL) are identical for a hospital, a port, a data center, or a manufacturing plant.
- **Specialization is achieved through interchangeable modules:** The skills (`05_skills/`), the expert corpus (`expert_corpus/`), the agents declared in the `Noosfile`, and the user preferences (`preferences.yaml`) are the only elements that vary between sectors.
- **Adopting a new sector requires no re‑engineering:** There is no need to rewrite the orchestration engine, the ASA cycle, or the semantic layer. It suffices to configure the appropriate sectorial modules.

### 8.2 Declarative Configuration Mechanism

AOF‑S provides a specific command in its CLI to initialize a sectorial ecosystem:

```bash
# Initialize a Noosystem for a hospital
noctl sector init --sector hospital --level 2

# Initialize a Noosystem for a data center
noctl sector init --sector datacenter --level 3
```

This command performs three actions:

1. **Copies the AOF‑S template** corresponding to the requested agency level (Reactive, Systemic, or Orchestrated).
2. **Adds sectorial skills** in `05_skills/`. For example, `clinical_agent_skill.md` and `energy_optimization_skill.md` for a hospital; `cooling_optimization_skill.md` for a data center.
3. **Generates a pre‑configured `Noosfile.yaml`** with the specialized agents of the sector, their levels, resources, and security policies.

The result is an ecosystem ready to deploy, with agents that understand the domain, skills that encapsulate sectorial logic, and an expert corpus that provides reference knowledge (regulations, manuals, operating guides).

### 8.3 Mapping of Sectors to AOF‑S Configurations

| Sector | Sectorial Skills (Examples) | Specialized N2 Agents | Expert Corpus Documents |
| :--- | :--- | :--- | :--- |
| **Hospital** | Vital signs monitoring, hospital energy optimization, HIPAA auditing. | Clinical agent, energy agent, security agent. | Clinical protocols, HIPAA regulations, hospital energy efficiency guides. |
| **Data Center** | Cooling optimization, predictive load balancing, vulnerability scanning. | Cooling agent, load balancer, security agent. | PUE specifications, ISO 27001 standards, data center design guides. |
| **Port** | Logistics planning, crane optimization, port security. | Logistics agent, crane agent, perimeter security agent. | International port regulations, crane operation manuals. |
| **Manufacturing** | Predictive maintenance, quality control, production line optimization. | Maintenance agent, quality agent, production agent. | Machinery manuals, ISO 9001 standards, lean manufacturing guides. |
| **Airport** | Passenger flow management, gate assignment, energy efficiency. | Flow agent, gate agent, energy agent. | Civil aviation regulations, airport operation guides. |

### 8.4 Example: Configuring a Smart Hospital with AOF‑S

For a 300‑bed hospital with an average electricity consumption of 4.2 MW, AOF‑S generates an ecosystem with the following agents, all operating on the same five layers of the Noosystem:

- **C1 (Field) and C3 (Edge):** N1 agents such as `vital-signs-monitor` (vital signs monitor) and `hvac-local-controller` (local HVAC controller). These agents are lightweight, run on IoT gateways or edge servers, and publish telemetry to the event bus.
- **C4 (Cloud):** N2 agents such as `clinical-agent` (clinical monitoring), `energy-agent` (energy optimization), `security-agent` (vulnerability scanning and HIPAA auditing), and `logistics-agent` (bed and supply management). These agents execute the complete ASA cycle: they formulate hypotheses, verify against policies, execute, and reflect.
- **C5 (Orchestration):** An N3 agent `hospital-orchestrator` coordinates all N2s and N1s, maintains the global Causal Graph of the hospital, simulates counterfactual scenarios, and ensures compliance with clinical and energy SLOs.

The `Noosfile` declares these agents, their resources, and the sector‑specific security policies (HIPAA, ENS). The sectorial skills contain the domain logic but run on the same generic ASA cycle. The expert corpus includes documents such as "Guide to energy efficiency in hospitals" and "HIPAA regulations for clinical data," indexed in the local vector database for RAG.

### 8.5 Adaptability as an Evolutionary Advantage

AOF‑S's sectorial configurability provides several advantages:

- **Replicability:** A hospital that has validated the ecosystem can share its configuration with another hospital, which only needs to adjust its infrastructure parameters (number of beds, photovoltaic capacity, etc.).
- **Scalability:** An ecosystem that starts with two N1 agents and one N2 can evolve to a swarm of 20 agents on the same layers, simply by adding new agents to the `Noosfile`.
- **Cross‑sector knowledge sharing:** The semantic rules learned by an `energy-agent` in a hospital can be transferred to an `energy-agent` in a data center, adapting only the thresholds and constraints. AOF‑S facilitates this transfer thanks to its standardized rule format (`learned_rules.yaml`).

## 9. Relationship with ASA and the Noosystem

AOF‑S is not an independent entity. It is the third pillar of the architectural triangle that, together with the ASA Model and the Noosystem, provides a complete framework for AI agents. This section explains how these three pillars relate, why each one is necessary, and how AOF‑S acts as the bridge that turns theory into an implementable standard and a governable ecosystem.

### 9.1 The Architectural Triangle

| Pillar | What It Is | What Problem It Solves |
|--------|-----------|------------------------|
| **Noosystem** | The stratified digital cognitive ecosystem. | Defines the scenario: where agents operate, how they collaborate, how they are governed, and how they learn collectively. |
| **ASA** | The architecture of the Adaptive Systemic Agent. | Defines the agent: how it reasons (7‑phase cycle), how it remembers (quadruple memory), how it learns (continuous reflection), and how it self‑regulates (algorithmic governance). |
| **AOF‑S** | The open standard of versionable files. | Defines packaging, orchestration, and governance: how cognitive artifacts are materialized, how the ecosystem is governed, and how interoperability is guaranteed. |

**Analogy:** If the Noosystem were a smart city, ASA would be the design of the autonomous vehicles that drive through it, and AOF‑S would be the standard that defines how those vehicles are manufactured, how they communicate, and how they are regulated.

### 9.2 AOF‑S as the Materialization of ASA

ASA is an abstract architecture. It defines *what* an agent must do, but not *how* it should be implemented. AOF‑S is the answer to that "how." Every ASA concept has a direct correspondence in an AOF‑S artifact:

| ASA Concept | AOF‑S Materialization |
|-------------|----------------------|
| **Principle of Reflexive Causal Intervention** | 7‑phase cycle documented in `spec/asa.yaml`. The agent reads and writes `hypothesis.md`, `observations/`, and `policies.yaml`. |
| **Three agency levels** | Templates `templates/reactive/`, `templates/systemic/`, `templates/orchestrated/`. |
| **7‑phase cycle** | Progressive loading matrix in `spec/aof-s.yaml`. Each phase loads a subset of artifacts. |
| **Quadruple memory** | Directories `06_memory/episodic/`, `06_memory/semantic/`, `06_memory/procedural/`, and file `meta_strategies.yaml`. |
| **Probabilistic Causal Graph** | `causal_graph.proto` with nodes, edges, and probability distributions. |
| **Neuro‑symbolic verification** | CLIPS/Drools rule engine configured in `ASA.yaml`. |
| **Zero Trust security** | `security` section in `Noosfile.yaml`. SPIFFE identities, mTLS, and OPA policies. |
| **Local‑First Principle** | `inference` section in `ASA.yaml`. |

Without AOF‑S, ASA would be a theoretical speculation. With AOF‑S, it is a standard that any LLM can adopt, any developer can customize, and any organization can audit.

### 9.3 AOF‑S as the Orchestrator of the Noosystem

AOF‑S not only materializes individual agents; it also provides the tools to govern the entire ecosystem:

- **Noosfile.yaml:** The declarative manifest that describes the desired state of the (sub)Noosystem. Inspired by Kubernetes, it allows declaring which agents must exist, at what level, with how many replicas, and under what security policies.
- **noctl:** The CLI that applies the `Noosfile` (similar to `kubectl apply`), allowing initialization, scaling, evaluation, and diagnosis of the ecosystem.
- **registry.yaml:** The connective tissue that maintains the living map of agents, their capabilities, trust scores, and knowledge topology.
- **The NSL (Noosystem Semantic Layer):** Allows agents to understand each other, negotiate meanings, and trust one another, closing the semantic gap that A2A and MCP do not solve by themselves.

These four components —Noosfile, noctl, registry, and NSL— transform the Noosystem from an abstract concept into a governable, auditable, and scalable ecosystem.

### 9.4 The Complete Flow

1. **The Noosystem** defines the ecosystem where agents collaborate: their deployment layers (C1 to C5), their collective memory, and their governance policies.

2. **Each ASA agent** operates within that ecosystem following the 7‑phase cycle. It starts by loading its AOF‑S artifacts, reasons with the most appropriate strategy, formulates causal hypotheses, verifies them (if N3), executes with monitoring, evaluates the results, and reflects to update its world model and memories.

3. **The AOF‑S artifacts** are the common language that agents read and write. They are versionable, auditable, and portable. Any LLM that understands Markdown + YAML can operate as an ASA agent.

4. **The NooKepler runtime** (under development) will act as the harness that executes the ASA cycle, manages progressive context loading, and provides the secure execution environment.

The triangle is complete. ASA provides the agent's intelligence. The Noosystem provides the ecosystem where that intelligence is deployed. And AOF‑S provides the standard that makes both possible in practice.

---

## Conclusion of the AOF‑S Conceptual Document

AOF‑S is not just another configuration format. It is the standard that closes the Noosystem triangle, materializing the ASA architecture in versionable files and providing the tools to govern agent ecosystems in a declarative, secure, and scalable manner.

Throughout this document, we have seen how AOF‑S:

1. **Solves the fragmentation** of the agent ecosystem with an open, Spec‑Driven, and Local‑First standard that any LLM can adopt.

2. **Implements the eight design principles** (0‑7) that guarantee data sovereignty, token efficiency, decision traceability, and progressive complexity.

3. **Defines a living file structure** that the agent reads and writes throughout its lifecycle, enabling continuous learning and complete auditing.

4. **Provides declarative orchestration** through the `Noosfile` and the `noctl` CLI, following the Infrastructure as Code pattern proven effective in Kubernetes.

5. **Incorporates a semantic layer (NSL)** that solves the problem of semantic isolation, allowing agents to understand, negotiate, and trust each other.

6. **Integrates Zero Trust security** declaratively, progressively, and aligned with the main reference frameworks (OWASP, NIST).

7. **Guarantees the Local‑First Principle** through an inference configuration section that allows each agent to declare its local engines and cloud fallback policies.

8. **Adapts to any sector** (hospitals, ports, data centers, manufacturing) without modifying its core, changing only skills, expert corpus, and configuration.

AOF‑S is, in short, the operating system of the agent. With ASA as the mind and the Noosystem as the world, AOF‑S provides the language, the tools, and the rules for AI agents to learn, reason, and collaborate.

---
**Authors:** Jorge Y. Hernández García (ETKinnova), DeepSeek (DeepThink) as systemic design assistant.  
**Part of the standard:** AOF‑S v1.0  
**Location:** `docs/concept/aofs-conceptual.md` in the public repository.