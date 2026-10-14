# Codebase to EDD

Turn an unfamiliar codebase into an engineering document you can actually use.

`codebase-to-edd` is a compact method for inspecting a software repository and producing an Engineering Design Document (EDD), system map and plain-text architecture diagrams.

The aim is not to restate the folder tree. It is to explain the running system: what it does, how its important parts interact, where state lives, what it depends on, how it fails, and which conclusions are facts versus inference.

## Why

Code can tell you what exists without telling you what the system is.

When I inherit or review software, I want a compact answer to a different set of questions:

- What does this system actually do?
- What are its important components?
- Where does state live?
- What calls what?
- What is authoritative?
- What happens when dependencies fail?
- What assumptions am I making because the code does not answer the question?

This method is designed to make those answers explicit.

## Outputs

A normal run should produce:

1. executive system description;
2. repository map;
3. component model;
4. runtime and request flow;
5. state and persistence model;
6. interfaces and dependencies;
7. failure modes;
8. security boundaries;
9. operational observations;
10. known / inferred / unknown findings;
11. ASCII architecture diagrams;
12. open questions.

## Design rules

- Separate observed facts from inference.
- Reference the real code where possible.
- Pin real analyses to an exact commit, not only a moving branch.
- Explain runtime behaviour, not merely directory structure.
- Prefer a few useful diagrams over decorative ones.
- Do not invent missing architecture.
- Mark uncertainty explicitly.
- Treat configuration, data stores and external services as part of the system.
- Record material failure paths, not just the happy path.
- Surface behaviour that contradicts or qualifies the apparent contract.

## Repository structure

```text
codebase-to-edd/
├── README.md
├── SKILL.md
├── templates/
│   └── EDD-template.md
├── references/
│   └── diagram-conventions.md
├── examples/
│   ├── README.md
│   ├── rakyll-hey/
│   │   ├── EDD.md
│   │   └── architecture.txt
│   ├── dbader-schedule/
│   │   ├── EDD.md
│   │   └── architecture.txt
│   └── illustrative-service/
│       ├── EDD.md
│       └── architecture.txt
└── LICENSE
```

## How to use it

Give a capable coding agent access to the target repository together with [`SKILL.md`](SKILL.md).

The agent should inspect the repository before writing the document, trace representative execution paths, identify state and dependencies, and then populate the EDD structure in [`templates/EDD-template.md`](templates/EDD-template.md).

For a public or versioned repository, analyse an exact commit and record it in the EDD. This makes the output reproducible and prevents later repository changes from silently invalidating the analysis.

The result should distinguish three classes of finding:

```text
KNOWN      directly supported by code, configuration or repository evidence
INFERRED   a reasonable conclusion from the evidence, but not explicit
UNKNOWN    material information the repository does not establish
```

The distinction matters. A fluent description of a system is not useful if it hides what has been guessed.

## A useful mental model

```text
REPOSITORY
    |
    v
ENTRY POINTS
    |
    v
RUNTIME FLOWS
    |
    +--> COMPONENTS
    +--> STATE
    +--> INTERFACES
    +--> DEPENDENCIES
    +--> FAILURE PATHS
    +--> SECURITY BOUNDARIES
    |
    v
EVIDENCE + INFERENCE + UNKNOWN
    |
    v
ENGINEERING DESIGN DOCUMENT
```

## Real-world worked examples

Two public repositories have been analysed end to end and pinned to exact commits. See [`examples/README.md`](examples/README.md) for provenance and source licences.

### Go — `rakyll/hey`

[`examples/rakyll-hey/EDD.md`](examples/rakyll-hey/EDD.md) reconstructs the CLI, worker pool, HTTP execution and reporting path. The analysis also exposes implementation behaviour that a directory summary would miss, including request-count truncation when `N` is not divisible by concurrency and disabled TLS certificate verification in the benchmark transport.

### Python — `dbader/schedule`

[`examples/dbader-schedule/EDD.md`](examples/dbader-schedule/EDD.md) reconstructs the in-process scheduling model, job lifecycle and next-run calculation. It makes the library's missed-job semantics explicit: delayed polling does not replay every missed interval.

### Fictional example

[`examples/illustrative-service`](examples/illustrative-service) remains as a deliberately small teaching example for partial-success ambiguity and unknowns.

## What this is not

This is not:

- a source-code summariser;
- an architecture scoring framework;
- a substitute for talking to maintainers;
- a guarantee that undocumented production behaviour can be recovered from a repository;
- a tool for inventing intent that the implementation does not reveal.

The output is a recoverable engineering view of the evidence available in the codebase.

## Licence

This repository is released under the **MIT License**. The complete licence text is in [`LICENSE`](LICENSE).

The public repositories used as worked examples retain their own licences; the example index records those source licences and pinned commits.
