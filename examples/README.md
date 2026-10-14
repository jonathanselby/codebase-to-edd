# Worked examples

These examples show `codebase-to-edd` applied to real public repositories as well as one deliberately fictional example used to illustrate partial-success and uncertainty handling.

## Real-world examples

### Go — `rakyll/hey`

Source: https://github.com/rakyll/hey  
Pinned commit: `5626f79b8698df6daf9b25799c9805c6acc96740`  
Source licence: Apache License 2.0

Outputs:

- [`rakyll-hey/EDD.md`](rakyll-hey/EDD.md)
- [`rakyll-hey/architecture.txt`](rakyll-hey/architecture.txt)

The analysis reconstructs the CLI-to-worker-to-reporter flow and surfaces two implementation facts that are easy to miss in a package summary: request allocation ignores `N % C`, and the benchmark transport disables TLS certificate verification.

### Python — `dbader/schedule`

Source: https://github.com/dbader/schedule  
Pinned commit: `82a43db1b938d8fdf60103bd41f329e06c8d3651`  
Source licence: MIT

Outputs:

- [`dbader-schedule/EDD.md`](dbader-schedule/EDD.md)
- [`dbader-schedule/architecture.txt`](dbader-schedule/architecture.txt)

The analysis reconstructs the in-process scheduler state model and makes an important operating semantic explicit: `run_pending()` does not replay every missed interval.

## Illustrative example

[`illustrative-service`](illustrative-service/) is fictional. It exists to show how an EDD should describe partial-success ambiguity and unknowns when the evidence does not support a confident answer.

## Reproducibility rule

Real-world examples must record the exact repository and commit used for analysis. A branch name alone is not sufficient because branches move.

The EDDs in this repository are original analysis and do not reproduce the source repositories. Source code remains under the licence of its respective project.
