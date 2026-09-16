# AOF-S — AI Orchestration Files for Systemic Agents

[![Standard](https://img.shields.io/badge/standard-AOF--S%20v1.6-blue)]()
[![Status](https://img.shields.io/badge/status-canonical-green)]()
[![License](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0-green)](LICENSE.txt)
[![Spec](https://img.shields.io/badge/spec-aof--s.yaml-orange)](spec/aof-s.yaml)

> **An open standard for versionable AI orchestration files that materialize the cognitive artifacts of Adaptive Systemic Agents (ASA).**

---

## Table of Contents

- [What is AOF-S?](#what-is-aof-s)
- [What AOF-S Is Not](#what-aof-s-is-not)
- [Ecosystem Context](#ecosystem-context)
- [The Cognitive Ranges](#the-cognitive-ranges)
- [Agency Levels (N1 / N2 / N3)](#agency-levels-n1--n2--n3)
- [Directory Structure](#directory-structure)
- [The Skill as a Unit of Capability](#the-skill-as-a-unit-of-capability)
- [Key Mechanisms](#key-mechanisms)
- [Repository Structure](#repository-structure)
- [Using This Repository](#using-this-repository)
- [Compatibility](#compatibility)
- [Documentation](#documentation)
- [Validation](#validation)
- [Contributing](#contributing)
- [Evolution of the Standard](#evolution-of-the-standard)
- [License](#license)

---

## What is AOF-S?

**AOF-S (AI Orchestration Files for Systemic Agents)** is an open standard that defines **how AI agents are packaged, structured, and configured** inside the Noosystem ecosystem. It specifies:

- **What artifacts** compose an agent.
- **Where** they are located.
- **What format** they have.
- **What rules** they must follow.

AOF-S is the **format specification**. It does not execute anything. Runtimes (such as `asa_agent`) interpret AOF-S artifacts and execute the ASA cycle.

The standard is materialized as a `.aof/` directory containing YAML and Markdown files organized by **cognitive ranges**. This design makes agents:

- **Portable** — the same artifacts run on any AOF-S-compatible runtime.
- **Versionable** — all configuration is text, diff-able, and auditable.
- **Local-First** — everything resides on the local file system.
- **Declarative** — no code is embedded in the agent's configuration.

---

## What AOF-S Is Not

| Concept | Is it AOF-S? | Reason |
|---------|--------------|--------|
| Runtime / execution engine | ❌ No | AOF-S defines format only. See `asa_agent` for a runtime. |
| Agent cognitive architecture | ❌ No | That is ASA. AOF-S materializes ASA artifacts. |
| Ecosystem topology | ❌ No | That is Noosystem. AOF-S references it but does not define it. |
| Orchestration framework | ❌ No | AOF-S is declarative; it provides no APIs. |
| Prompting technique | ❌ No | AOF-S is a file structure, not prompt engineering. |
| Programming language | ❌ No | AOF-S is a markup standard, not Turing-complete. |
| Secrets manager | ❌ No | Secrets are never stored in artifacts; only references. |

---

## Ecosystem Context

AOF-S is one pillar of a three-part architectural triangle:

```
┌─────────────────────────────────────────────────────────────────┐
│                     NOOSYSTEM (ecosystem)                       │
│  Digital cognitive ecosystem: topology, profiles, orchestration │
│                          ↓                                      │
│                       ASA (architecture)                        │
│      Cognitive architecture: cycle, memory, meta-cognition      │
│                          ↓                                      │
│                      AOF-S (this standard)                      │
│        File format & packaging: artifacts, skills, templates    │
│                          ↓                                      │
│                     runtimes (implementations)                  │
│              Interpret artifacts and execute the cycle          │
└─────────────────────────────────────────────────────────────────┘
```

- **Noosystem** — where agents collaborate. Defined in `noosystem.yaml`.
- **ASA** — the cognitive architecture of each agent. Defined in `asa.yaml`.
- **AOF-S** — the file format that materializes ASA artifacts. Defined in this repository.
- **Runtimes** — implementations that consume AOF-S. See [`asa_agent`](https://github.com/etkinnova/asa_agent).

> **Note on governance**: Cross-project architecture decisions live in the separate [`noosystem-design`](https://github.com/etkinnova/noosystem-design) repository. This repository contains the **AOF-S standard itself**: its specification, conceptual documentation, and reusable packages.

---

## The Cognitive Ranges

AOF-S organizes artifacts into **thirteen cognitive ranges** (numbered `00` to `12`). Each range answers a **precise cognitive question**. The numeric prefix is a stable taxonomy, not an execution order.

| Range | Domain | Cognitive Question | Root Path |
|-------|--------|--------------------|-----------|
| `00` | Identity | Who am I? | `00_manifest.yaml` |
| `01` | Agent Specification | What capabilities do I have? | `01_agent_spec/` |
| `02` | Static Context | In what fixed environment do I operate? | `02_context_static.md` |
| `03` | Mission | What must I achieve? | `03_mission.yaml` |
| `04` | Tasks and Planning | What am I going to do? | `04_tasks/` |
| `05` | Hypotheses and Predictions | What do I expect to happen when I act? | `05_hypotheses/` |
| `06` | Skills | What do I know how to do? | `06_skills/` |
| `07` | World Model | How is the world that I perceive? | `07_world_model/` |
| `08` | Memory | What have I learned? | `08_memory/` |
| `09` | Validation | How do I verify compliance with the rules? | `09_validation_spec.md` |
| `10` | Observations | What is happening? | `10_observations/` |
| `11` | Values and Governance | Under which principles and with whom do I govern? | `11_values/` |
| `12` | Noosystemic Identity | How do I communicate with other agents? | `12_ASA.yaml` |

**Rules of the ranges:**

- Each artifact belongs to exactly **one** range.
- Ranges are **stable**; numbers are never reused for other purposes.
- All agency levels (N1, N2, N3) use the **same** range for shared artifacts.

---

## Agency Levels (N1 / N2 / N3)

AOF-S defines three **progressive agency levels**. Higher levels reuse lower-level artifacts and add new ones.

| Level | Name | Memory | World Model | Cycle Phases | Orchestrates |
|-------|------|--------|-------------|--------------|--------------|
| **N1** | Reactive | None | None | 4 | No |
| **N2** | Systemic | Episodic + Semantic + Procedural | Variables + State | 6 + Evidence Gate | No |
| **N3** | Orchestrated | Quadruple (+ Meta-cognitive) | + Causal graph, delays, expert corpus | 7 + Evidence Gate | Yes |

### N1 — Reactive (minimum)

Required:
- `00_manifest.yaml`
- `01_agent_spec/agent_spec.yaml`

Full N1 includes:
- `02_context_static.md`
- `03_mission.yaml`
- `04_tasks/current_plan.md`
- `06_skills/`
- `09_validation_spec.md`
- `10_observations/feedback.yaml`

**Ultra-light mode**: only 3 artifacts (manifest, agent_spec, context_static). No skills, no LLM.

### N2 — Systemic

Adds (to N1):
- `04_tasks/executed/`
- `04_tasks/plan_context.yaml`
- `05_hypotheses/active.md`
- `07_world_model/variables.yaml`
- `07_world_model/state.md`
- `08_memory/episodic/`
- `08_memory/semantic/learned_rules.yaml`
- `08_memory/procedural/policies.yaml`
- `10_observations/anomalies.md`
- `11_values/constitution.md`
- `11_values/preferences.yaml`
- `11_values/ledger.yaml`
- `11_values/trust_score.yaml`

### N3 — Orchestrated

Adds (to N2):
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

## Directory Structure

The full canonical `.aof/` layout for an N3 agent:

```
.aof/
├── 00_manifest.yaml
├── 01_agent_spec/
│   └── agent_spec.yaml
├── 02_context_static.md
├── 03_mission.yaml
├── 04_tasks/
│   ├── current_plan.md
│   ├── plan_context.yaml          # N2/N3
│   ├── executed/                  # N2/N3
│   └── delegated/
│       └── current_steps.md       # N3
├── 05_hypotheses/                 # N2/N3
│   ├── active.md                  # N2
│   ├── active/                    # N3
│   ├── validated/                 # N3
│   └── falsified/                 # N3
├── 06_skills/
│   └── <skill-name>/
│       ├── SKILL.md
│       ├── scripts/               # optional
│       ├── references/            # optional
│       ├── assets/                # optional
│       ├── templates/             # optional
│       └── endpoints.yaml         # optional
├── 07_world_model/                # N2/N3
│   ├── variables.yaml
│   ├── state.md
│   ├── causal_graph.proto         # N3
│   ├── delays.yaml                # N3
│   └── expert_corpus/             # N3
│       ├── vector_store/
│       ├── source_documents/
│       └── sectors/
├── 08_memory/                     # N2/N3
│   ├── episodic/
│   ├── semantic/
│   │   ├── learned_rules.yaml
│   │   └── domain_facts.md        # N3
│   ├── procedural/
│   │   └── policies.yaml
│   └── meta/
│       └── meta_strategies.yaml   # N3
├── 09_validation_spec.md
├── 10_observations/
│   ├── feedback.yaml
│   └── anomalies.md               # N2/N3
├── 11_values/
│   ├── constitution.md            # N2/N3
│   ├── preferences.yaml           # N2/N3
│   ├── registry.yaml              # N3
│   ├── ledger.yaml                # N2/N3
│   └── trust_score.yaml           # N2/N3
└── 12_ASA.yaml                    # N3
```

**Format classification** (the runtime must parse by classification, never by auto-detection):

| Type | Applies to |
|------|-----------|
| **YAML pure** | `00_manifest.yaml`, `agent_spec.yaml`, `03_mission.yaml`, `plan_context.yaml`, `variables.yaml`, `learned_rules.yaml`, `policies.yaml`, `feedback.yaml`, `preferences.yaml`, `registry.yaml`, `ledger.yaml`, `trust_score.yaml`, `12_ASA.yaml` |
| **Markdown pure** | `02_context_static.md`, `current_plan.md`, hypotheses, `state.md`, `09_validation_spec.md`, `anomalies.md`, `constitution.md` |
| **Markdown + frontmatter** | `06_skills/*/SKILL.md` (only this format uses frontmatter) |

---

## The Skill as a Unit of Capability

A skill lives in `06_skills/<skill-name>/` and is composed of a `SKILL.md` file (YAML frontmatter + Markdown body) plus optional subdirectories.

### Format

```
06_skills/
└── health-check/
    ├── SKILL.md
    ├── scripts/
    ├── references/
    ├── assets/
    ├── templates/
    └── endpoints.yaml
```

### `SKILL.md` structure

```markdown
---
name: health-check
description: "Checks HTTP endpoint health"
version: "1.0.0"
tools:
  - name: http_health
    description: "Performs an HTTP request to a URL"
    executor: "native"
    native:
      action: "http_request"
    defaults:
      method: "GET"
      timeout_ms: 5000
llm:
  mode: "on_error"
---

Check the endpoint and log the result.
```

### Tool types (`executor`)

| Executor | Description |
|----------|-------------|
| `native` | Tool built into the runtime (default). |
| `mcp` | Tool provided by an MCP server. |
| `system_command` | System command execution. |
| `function` | Tool delegated to the LLM with a JSON Schema. |

### LLM modes

| Mode | Behavior |
|------|----------|
| `never` | Deterministic execution, no LLM call. |
| `always` | LLM consulted before every execution. |
| `on_error` | LLM consulted only if the previous cycle had errors. |
| `on_change` | LLM consulted if mission, context, or endpoints change. |
| `cached` | Uses semantic memory if confidence is high; otherwise consults and stores. |

### Agent Skills compatibility

AOF-S is fully compatible with the [agentskills.io](https://agentskills.io) standard. If a skill does **not** define `tools`, the runtime injects the skill's Markdown body as instructions to the LLM. This ensures any community skill works out of the box.

---

## Key Mechanisms

### Progressive Context Loading

The agent loads only the artifacts relevant to the current phase, keeping token usage between 2k and 12k:

| Phase | Artifacts loaded |
|-------|------------------|
| `startup` | manifest, agent_spec, context_static, mission, state, constitution |
| `reason` | episodic (top-N), policies, variables, domain_facts |
| `hypothesize` | causal_graph, delays, expert_corpus (RAG), simulation skill |
| `verify` | verification_rules, semantic_contracts, security_policies |
| `execute` | current_steps, specific_skills |
| `evaluate` | feedback_log, anomaly_notes |
| `reflect` | hypothesis, semantic_rules, policies, meta_cognitive |

### Evidence Gate

Defined in `asa.yaml` and implemented by the runtime: if `total_errors == 0` after evaluation, the agent skips the `hypothesize` and `reflect` phases to save tokens.

**`total_errors` is the sum of:**
- Operational errors (tools, MCP, LLM, validation, anomalies, ledger)
- Task failures
- Mission failure (binary)

### Secrets Management

Secrets are **never** stored in plain text within `.aof/`. Artifacts use references:

- `${env:VARIABLE_NAME}` — resolved from environment variables.
- `${secret:path/to/secret}` — resolved by the deployment profile backend.

Resolution depends on the deployment profile:

| Profile | Backend |
|---------|---------|
| `standalone` | Environment variables / `.env` |
| `edge` | Encrypted file (Age/SOPS) with local master key |
| `enterprise` | Vault / AWS Secrets Manager / SPIFFE |

Every resolution is logged without recording the secret value.

### Deployment Profiles

| Profile | Agents | Components | Secrets |
|---------|--------|------------|---------|
| `standalone` | 1 | `.aof/` + binary | Env / `.env` |
| `edge` | 10–50 | `.aof/`, NATS, optional LanceDB/SQLite | Age/SOPS |
| `enterprise` | 100s | `.aof/`, Kubernetes, Istio, SPIRE, OPA, Kafka | Vault / SPIFFE |

### Protocols

AOF-S relies on open standards — no proprietary layer:

| Layer | Protocol | Role |
|-------|----------|------|
| Transport | SLIM (AGNTCY) | Secure low-latency messaging |
| Syntactic | A2A + MCP | A2A for discovery and delegation; MCP for external tools |
| Semantic | VCV + NSL conventions over A2A/MCP | Semantic discovery without a proprietary layer |

---

## Repository Structure

```
aof-s/
├── README.md                # This file
├── about.txt                # Short introduction for newcomers
├── LICENSE.txt              # MIT OR Apache-2.0
├── spec/                    # The standard specification
│   ├── aof-s.yaml           # Canonical specification (v1.6)
│   └── aof-s.templates.yaml # Spec-Driven generation templates
├── docs/                    # Conceptual and normative documentation
│   └── aofs-conceptual-es.md
└── packages/                # Reusable AOF Packages
    └── <package-name>/      # Each package contains skills, knowledge, etc.
```

### Contents

| Directory | Purpose |
|-----------|---------|
| `spec/` | Canonical YAML specifications. The runtime loads these at startup. |
| `docs/` | Conceptual and normative documentation in Markdown (Spanish; translations welcome). |
| `packages/` | Reusable AOF Packages that agents can import via `00_manifest.yaml`. |

---

## Using This Repository

### 1. Clone the repository

**Linux / macOS:**
```bash
git clone https://github.com/etkinnova/aof-s.git
cd aof-s
```

**Windows (PowerShell):**
```powershell
git clone https://github.com/etkinnova/aof-s.git
cd aof-s
```

### 2. Install the specifications in the central ecosystem directory

Runtimes read AOF-S from `~/.noosystem/specs/` (or `$NOOSYSTEM_PATH/specs/`).

**Linux / macOS:**
```bash
mkdir -p ~/.noosystem/specs
cp spec/aof-s.yaml ~/.noosystem/specs/
cp spec/aof-s.templates.yaml ~/.noosystem/specs/
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.noosystem\specs"
Copy-Item spec\aof-s.yaml "$env:USERPROFILE\.noosystem\specs\"
Copy-Item spec\aof-s.templates.yaml "$env:USERPROFILE\.noosystem\specs\"
```

### 3. Import a package into an agent

Edit the agent's `00_manifest.yaml`:

```yaml
framework: AOF-S v1.6
level: 2
name: "my-agent"
profile: "edge"
imports:
  - path: "~/.noosystem/packages/monitoring"
    include: ["06_skills", "07_world_model/expert_corpus"]
    conflict_policy: "merge"
```

### 4. Validate a `.aof/` directory

Using an AOF-S-compatible runtime:

```bash
# With the asa_agent runtime (N3)
ASA_USER_REQUEST="Validate ./.aof" cargo run -p asa-n3
```

Future versions of `noctl` will provide:

```bash
noctl validate ./.aof
```

---

## Compatibility

AOF-S v1.6 is compatible with:

- **AOF-S v1.2, v1.3, v1.4, v1.5** — with migration notes.
- **[agentskills.io](https://agentskills.io)** — skills are drop-in compatible.
- **MCP (Model Context Protocol)** — tool integration via MCP servers.
- **A2A (Agent-to-Agent)** — communication and discovery via signed Agent Cards.
- **VCV (Versioned Capability Vectors)** — semantic agent discovery.

### Breaking changes between versions

| Version | Change |
|---------|--------|
| v1.4 → v1.5 | Reorganized cognitive ranges; unified artifact paths across levels. |
| v1.5 → v1.6 | Renamed `11_ASA.yaml` to `12_ASA.yaml`; moved `plan_context.yaml` into `04_tasks/`; introduced explicit placeholder convention (`<...>`) in examples. |

---

## Documentation

### In this repository

| Document | Description |
|----------|-------------|
| [`spec/aof-s.yaml`](spec/aof-s.yaml) | **Canonical specification.** Always prevails in case of discrepancy. |
| [`spec/aof-s.templates.yaml`](spec/aof-s.templates.yaml) | Spec-Driven generation templates for N3. |
| [`docs/aofs-conceptual-es.md`](docs/aofs-conceptual-es.md) | Conceptual and normative document (Spanish). |
| [`about.txt`](about.txt) | Short introduction for newcomers. |

### Related repositories

| Repository | Purpose |
|------------|---------|
| [`asa`](https://github.com/etkinnova/asa) | ASA architecture specification and conceptual docs. |
| [`noosystem`](https://github.com/etkinnova/noosystem) | Noosystem ecosystem specification and conceptual docs. |
| [`noosystem-design`](https://github.com/etkinnova/noosystem-design) | Cross-project ADRs and architecture decision records. |
| [`asa_agent`](https://github.com/etkinnova/asa_agent) | Reference runtime implementation (Rust). |

---

## Validation

A `.aof/` directory is **conformant** to AOF-S v1.6 if:

- Its structure matches the requirements of its declared level.
- All mandatory artifacts exist and have the correct format.
- Ranges and paths follow the canonical taxonomy.
- Secrets are not stored in plain text (only references are used).
- Skills conform to the `SKILL.md` syntax.
- `framework: AOF-S v1.6` is declared in `00_manifest.yaml`.
- Examples in specifications use placeholders (`<...>`), not literal values.
- If the strategy requires it, `plan_context.yaml` exists in N2 and N3.

Validation can be performed with `validate_aof` in an N3 runtime, or with future versions of `noctl validate`.

---

## Contributing

We welcome contributions from:

- **Implementers** of AOF-S-compatible runtimes.
- **Authors** of skills and AOF Packages.
- **Designers** of standards, protocols, and tooling.

### How to contribute

1. **Read the specification**: [`spec/aof-s.yaml`](spec/aof-s.yaml) and [`docs/aofs-conceptual-es.md`](docs/aofs-conceptual-es.md).
2. **Check existing issues** to avoid duplication.
3. **Open a discussion** for substantive changes to the standard.
4. **Submit a pull request** for:
   - Bug fixes in the specification (typos, ambiguities).
   - New reusable packages under `packages/`.
   - Translations of conceptual documents.
   - Documentation improvements.

### Normative changes

Substantive changes to `aof-s.yaml` are managed as **Architecture Decision Records (ADRs)** in the [`noosystem-design`](https://github.com/etkinnova/noosystem-design) repository, because they affect the whole ecosystem (runtime, architecture, tooling).

### Adding a reusable package

1. Create a directory under `packages/<package-name>/`.
2. Include one or more valid ranges (e.g., `06_skills/`, `07_world_model/`).
3. Add a `catalog.yaml` entry describing the package.
4. Ensure the package does not modify `00_manifest.yaml` or `01_agent_spec/` of the consuming agent.
5. Document usage examples in the package's own `README.md`.

---

## Evolution of the Standard

The AOF-S standard evolves through **Architecture Decision Records (ADRs)**. It follows **semantic versioning**:

- **MAJOR** changes: incompatible with previous versions (rare).
- **MINOR** changes: backward-compatible additions (new optional fields, new artifact types).
- **PATCH** changes: clarifications, typo fixes, documentation.

Compatibility is maintained across `compatible_versions` declared in the metadata.

### Compatible changes

- Addition of **optional** fields.
- Addition of new artifacts to existing ranges.
- Clarification of ambiguous wording.

### Incompatible changes

- Relocation of ranges or renumbering.
- Removal of artifacts.
- Renaming of existing fields.

Incompatible changes require an ADR and coordination with all implementations.

---

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

---

## Acknowledgements

AOF-S is part of the **Noosistema** ecosystem and is developed under the vision of a **Local-First, Zero-Trust** future for AI agents.

Special thanks to the contributors of the ASA architecture, the Noosystem ecosystem, and the broader open-source agent community.

---

**Authors:**  
Jorge Y. Hernández García (ETKinnova) and contributors

**Repository:** [https://github.com/etkinnova/aof-s](https://github.com/etkinnova/aof-s)

**Standard version:** AOF-S v1.6

**Related specifications:** [`asa.yaml`](https://github.com/etkinnova/asa), [`noosystem.yaml`](https://github.com/etkinnova/noosystem)