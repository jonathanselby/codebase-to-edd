# Illustrative Service — Engineering Design Document

> This is a deliberately small fictional example showing the expected output shape. It is not the result of analysing a real repository.

## 1. Executive system description

Illustrative Service is a small HTTP application that accepts a job request, validates it, stores the job, invokes one external worker service, and records the returned result.

The example is intentionally narrow. Its purpose is to demonstrate how the EDD separates runtime behaviour, state, failure paths and uncertainty.

## 2. Scope and evidence basis

### Repository analysed

Fictional example only.

### Included

- HTTP request path;
- job persistence;
- outbound worker call;
- success/failure behaviour.

### Excluded

- deployment platform;
- production networking;
- authentication implementation;
- scaling model.

### Evidence basis

No real source repository. All findings below are illustrative by construction.

## 3. Repository map

| Area | Responsibility | Evidence |
|---|---|---|
| `cmd/server` | process startup | illustrative |
| `internal/http` | request parsing and response handling | illustrative |
| `internal/jobs` | job orchestration | illustrative |
| `internal/store` | job persistence | illustrative |
| `internal/worker` | outbound worker client | illustrative |

## 4. Component model

| Component | Purpose | Inputs | Outputs | State / dependencies | Confidence |
|---|---|---|---|---|---|
| HTTP handler | Accept job requests | HTTP request | HTTP response | job service | KNOWN |
| Job service | Coordinate job execution | validated job | stored result | store, worker client | KNOWN |
| Store | Persist jobs/results | job records | job records | SQLite | KNOWN |
| Worker client | Invoke external processor | job payload | worker result | external Worker API | KNOWN |

## 5. Runtime flows

### Flow A — submit job

1. Client sends `POST /jobs`.
2. Handler parses and validates the request.
3. Invalid requests return `400` without creating a job.
4. Valid requests are passed to Job Service.
5. Job Service writes a `pending` record to the Store.
6. Job Service calls the external Worker API.
7. On success, Job Service stores the returned result and marks the job `complete`.
8. Handler returns `200` with the job result.
9. On worker failure, Job Service records the failure and returns an application error.

## 6. State and persistence

| Store / state | Content | Read by | Written by | Authority | Lifecycle | Confidence |
|---|---|---|---|---|---|---|
| SQLite | job request, status, result/error | Job Service | Job Service | authoritative for application job state | persistent | KNOWN |
| process memory | request-scoped values | handler/service | handler/service | transient | request lifetime | KNOWN |

## 7. Interfaces

| Interface | Direction | Contract | Used by | Evidence |
|---|---|---|---|---|
| `POST /jobs` | inbound | JSON request/response | HTTP handler | illustrative |
| Worker API | outbound | JSON over HTTP | Worker client | illustrative |
| SQLite | internal persistence | SQL | Store | illustrative |

## 8. External dependencies

| Dependency | Purpose | Invocation | Failure impact | Evidence |
|---|---|---|---|---|
| Worker API | Process job payload | synchronous HTTP call | job cannot complete | illustrative |
| SQLite | persist job state | local SQL | requests requiring persistence fail | illustrative |

## 9. Configuration

| Setting | Source | Purpose | Default / required | Evidence |
|---|---|---|---|---|
| `HTTP_ADDR` | environment | listen address | default `:8080` | illustrative |
| `DB_PATH` | environment | SQLite file path | required | illustrative |
| `WORKER_URL` | environment | Worker API endpoint | required | illustrative |

## 10. Failure and recovery behaviour

| Failure | Detection | Behaviour | Recovery | Confidence |
|---|---|---|---|---|
| invalid request | validation error | return `400` | client must correct request | KNOWN |
| database write fails | returned store error | request fails | no retry shown | KNOWN |
| worker call fails | HTTP/client error | failure recorded; request fails | no automatic retry shown | KNOWN |
| result write fails after worker success | returned store error | request fails; external work may already have occurred | reconciliation behaviour unknown | UNKNOWN |

The final case is important: a fluent system description could easily hide the partial-success problem.

## 11. Security boundaries

The example assumes externally supplied HTTP input crosses into the application at the handler.

Input validation is represented, but authentication and authorisation are deliberately unspecified. They should therefore be recorded as **UNKNOWN**, not assumed.

## 12. Operational behaviour

### Known

- application has a long-running HTTP process;
- failures propagate as application errors;
- state persists to SQLite.

### Unknown

- metrics;
- tracing;
- structured logging;
- graceful shutdown;
- health checks;
- deployment supervision;
- timeout values.

## 13. Known / inferred / unknown

### Known

For this fictional example, the stated runtime flow and persistence model are treated as known inputs.

### Inferred

None required.

### Unknown

- authentication and authorisation;
- production deployment topology;
- retry policy;
- concurrency limits;
- reconciliation after partial success;
- backup/recovery for the SQLite store.

## 14. Open questions

1. Is a failed Worker API call safe to retry?
2. What happens when the worker succeeds but recording the result fails?
3. Is the request synchronous by design or only for the current implementation?
4. What process supervises and restarts the service?
5. Are job payloads sensitive?

## 15. Architecture diagrams

See [`architecture.txt`](architecture.txt).
