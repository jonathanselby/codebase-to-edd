# Codebase to EDD — Analysis Skill

## Objective

Inspect an unfamiliar software repository and produce an Engineering Design Document that explains the running system, not merely the source tree.

The output must separate what is directly evidenced from what is inferred or unknown.

## Core rule

Do not write the EDD from filenames alone.

Trace representative execution paths from entry point to side effect. Read configuration, interfaces, persistence code, external clients, tests and operational files where they materially affect runtime behaviour.

## Required workflow

### 1. Establish repository scope

Identify:

- primary language(s);
- build and dependency system;
- executable entry points;
- major top-level packages/modules;
- configuration files;
- tests;
- deployment/runtime files;
- generated code or vendored dependencies that should not dominate the analysis.

State any scope exclusions explicitly.

### 2. Identify entry points

Find how the system starts and how work enters it.

Examples include:

- `main` packages;
- HTTP servers;
- CLI commands;
- queue consumers;
- scheduled jobs;
- event handlers;
- library entry points.

For each material entry point, identify the first meaningful application component it invokes.

### 3. Build the component model

Group code by runtime responsibility rather than directory name where necessary.

For each important component record:

- purpose;
- inputs;
- outputs;
- state owned or accessed;
- important dependencies;
- evidence references.

Avoid creating components for trivial helper packages unless they materially affect system behaviour.

### 4. Trace representative runtime flows

Trace enough end-to-end flows to explain the system.

For each flow identify:

1. trigger;
2. validation;
3. control flow;
4. state reads/writes;
5. external calls;
6. returned or emitted result;
7. failure branches.

Prefer two or three representative flows over exhaustive call-graph transcription.

### 5. Identify state and authority

Find all material state stores, including:

- SQL/NoSQL databases;
- files;
- caches;
- queues;
- object stores;
- in-memory process state;
- external systems treated as authoritative.

For each store record:

- what is stored;
- read/write ownership;
- lifecycle;
- whether it appears authoritative, derived, cached or transient;
- uncertainty if authority cannot be established.

### 6. Identify interfaces and dependencies

Record material interfaces such as:

- HTTP/REST;
- gRPC;
- CLI;
- message queues;
- database contracts;
- filesystem contracts;
- SDK/API clients;
- environment variables;
- config files.

Distinguish internal interfaces from external dependencies.

### 7. Inspect failure behaviour

Do not stop at the happy path.

Look for:

- returned errors;
- retries;
- timeouts;
- circuit breakers;
- dead-letter behaviour;
- process termination;
- fallback behaviour;
- partial writes;
- compensating actions;
- ignored errors;
- panic/exception paths.

If recovery behaviour is absent, say so rather than assuming infrastructure handles it.

### 8. Inspect security boundaries

Identify only what the repository supports, including:

- authentication;
- authorisation;
- trust boundaries;
- credential handling;
- encryption use;
- externally supplied input;
- privileged operations;
- sensitive state.

Do not claim a control exists because it would be expected in production.

### 9. Inspect operational behaviour

Where evidence exists, record:

- logging;
- metrics;
- tracing;
- health checks;
- graceful shutdown;
- configuration loading;
- deployment assumptions;
- concurrency model;
- resource-sensitive behaviour.

### 10. Classify findings

Every material conclusion should be one of:

- **KNOWN** — directly supported by repository evidence;
- **INFERRED** — reasonable interpretation of evidence but not explicit;
- **UNKNOWN** — material question not established by the repository.

Use inline evidence references where practical, for example:

```text
KNOWN — requests enter through cmd/server/main.go and are registered in internal/http/routes.go.
```

Do not disguise inference as fact.

## Required EDD sections

Use `templates/EDD-template.md` and cover:

1. Executive system description
2. Scope and evidence basis
3. Repository map
4. Component model
5. Runtime flows
6. State and persistence
7. Interfaces
8. External dependencies
9. Configuration
10. Failure and recovery behaviour
11. Security boundaries
12. Operational behaviour
13. Known / inferred / unknown
14. Open questions
15. Architecture diagrams

## Diagram rules

Use the conventions in `references/diagram-conventions.md`.

At minimum produce:

- one system/context diagram;
- one runtime-flow diagram where the system has a meaningful request/event path.

ASCII diagrams should survive terminals, Markdown, email and plain-text review.

## Evidence standard

Prefer evidence in this order:

1. executable code paths;
2. tests demonstrating behaviour;
3. configuration and schemas;
4. deployment/runtime files;
5. repository documentation;
6. comments.

Documentation may be stale. Code may also be incomplete. Note contradictions rather than silently reconciling them.

## Things not to do

Do not:

- invent business intent;
- infer production topology without evidence;
- produce a package-by-package prose dump;
- describe every utility function;
- hide uncertainty;
- assume tests prove production configuration;
- assume an interface is used merely because it exists;
- turn the EDD into an architecture scorecard;
- recommend a redesign unless explicitly asked.

## Completion test

The EDD is good enough when a competent engineer unfamiliar with the repository can answer:

- what the system is for;
- how work enters and moves through it;
- which components matter;
- where state lives;
- which external systems it depends on;
- what happens when important things fail;
- where trust boundaries sit;
- what is known versus inferred;
- what they still need to ask a maintainer.
