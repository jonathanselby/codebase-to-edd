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
- Explain runtime behaviour, not merely directory structure.
- Prefer a few useful diagrams over decorative ones.
- Do not invent missing architecture.
- Mark uncertainty explicitly.
- Treat configuration, data stores and external services as part of the system.
- Record material failure paths, not just the happy path.

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
│   └── illustrative-service/
│       ├── EDD.md
│       └── architecture.txt
└── LICENSE
```

## How to use it

Give a capable coding agent access to the target repository together with [`SKILL.md`](SKILL.md).

The agent should inspect the repository before writing the document, trace representative execution paths, identify state and dependencies, and then populate the EDD structure in [`templates/EDD-template.md`](templates/EDD-template.md).

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

## What this is not

This is not:

- a source-code summariser;
- an architecture scoring framework;
- a substitute for talking to maintainers;
- a guarantee that undocumented production behaviour can be recovered from a repository;
- a tool for inventing intent that the implementation does not reveal.

The output is a recoverable engineering view of the evidence available in the codebase.

## Example

[`examples/illustrative-service`](examples/illustrative-service) shows the expected shape of an output for a deliberately small fictional service. It is an example of the document standard, not evidence that the method has yet been benchmarked against that implementation.

A real public-repository cross-test is the next planned increment.

## Licence

MIT.
