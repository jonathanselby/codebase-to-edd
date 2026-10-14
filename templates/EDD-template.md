# Engineering Design Document

## 1. Executive system description

Summarise the system in plain English.

Cover:

- what the system appears to do;
- who or what invokes it;
- its major runtime responsibilities;
- the most important state and dependencies;
- material uncertainty;
- any behaviour that materially qualifies the apparent contract.

## 2. Scope and evidence basis

### Repository analysed

- Repository: `<canonical repository URL>`
- Commit / immutable ref: `<SHA or tag>`
- Branch at analysis time: `<branch, if relevant>`
- Source licence: `<licence>`

### Included

- `<scope>`

### Excluded

- `<scope>`

### Evidence basis

List the main code, tests, configuration and runtime artefacts used. Prefer links pinned to the analysed commit where practical.

## 3. Repository map

Describe only the parts that materially affect system behaviour.

| Area | Responsibility | Evidence |
|---|---|---|
| `<path>` | `<runtime responsibility>` | `<file/reference>` |

## 4. Component model

| Component | Purpose | Inputs | Outputs | State / dependencies | Confidence |
|---|---|---|---|---|---|
| `<component>` | `<purpose>` | `<inputs>` | `<outputs>` | `<state/dependencies>` | KNOWN / INFERRED |

## 5. Runtime flows

### Flow A — `<name>`

1. `<trigger>`
2. `<validation>`
3. `<control flow>`
4. `<state read/write>`
5. `<external call>`
6. `<result>`
7. `<failure path>`

Repeat for representative flows only.

Call out edge conditions where loops, batching, integer division, ordering, concurrency, cancellation or partial success change observable behaviour.

## 6. State and persistence

| Store / state | Content | Read by | Written by | Authority | Lifecycle | Confidence |
|---|---|---|---|---|---|---|
| `<store>` | `<content>` | `<component>` | `<component>` | authoritative / derived / cached / transient / unknown | `<lifecycle>` | KNOWN / INFERRED |

## 7. Interfaces

| Interface | Direction | Contract | Used by | Evidence |
|---|---|---|---|---|
| `<interface>` | inbound / outbound / internal | `<protocol/schema/API>` | `<component>` | `<reference>` |

For libraries, distinguish library guarantees from responsibilities retained by the caller.

## 8. External dependencies

| Dependency | Purpose | Invocation | Failure impact | Evidence |
|---|---|---|---|---|
| `<dependency>` | `<purpose>` | `<how called>` | `<impact>` | `<reference>` |

## 9. Configuration

Document material runtime configuration only.

| Setting | Source | Purpose | Default / required | Evidence |
|---|---|---|---|---|
| `<setting>` | env / file / flag / API | `<purpose>` | `<value/status>` | `<reference>` |

## 10. Failure and recovery behaviour

For each material failure mode, state what the code actually does.

| Failure | Detection | Behaviour | Recovery | Confidence |
|---|---|---|---|---|
| `<failure>` | `<signal>` | `<behaviour>` | `<recovery>` | KNOWN / INFERRED / UNKNOWN |

Call out explicitly where recovery is absent or cannot be established.

Also inspect zero-result, empty-state, partial-success, retry and cancellation paths where relevant.

## 11. Security boundaries

Record only controls and boundaries supported by evidence.

Consider:

- authentication;
- authorisation;
- untrusted input;
- privileged operations;
- credentials;
- TLS/certificate behaviour;
- sensitive data;
- external trust boundaries.

## 12. Operational behaviour

Document evidence for:

- logging;
- metrics;
- tracing;
- health checks;
- graceful shutdown;
- cancellation semantics;
- concurrency;
- deployment assumptions;
- restart/persistence assumptions;
- resource-sensitive behaviour.

## 13. Known / inferred / unknown

### Known

Directly supported by code, configuration, tests or runtime artefacts.

### Inferred

Reasonable conclusions from evidence that are not explicit.

### Unknown

Material questions the repository does not answer.

## 14. Open questions

Questions for maintainers or operators that would materially improve understanding.

1. `<question>`

## 15. Architecture diagrams

Include at minimum:

- system/context diagram;
- representative runtime-flow diagram.

Use `references/diagram-conventions.md`.
