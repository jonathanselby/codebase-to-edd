# Diagram conventions

The diagrams in this repository are intentionally plain text.

They should survive Markdown, terminals, email, code review and copied design notes without depending on a rendering engine.

## Principles

1. Show runtime relationships, not decorative boxes.
2. Label important edges with the action or protocol.
3. Keep direction obvious.
4. Prefer one clear diagram per question.
5. Separate context, component and runtime-flow views.
6. Do not imply topology, trust or ownership that the repository does not establish.

## Basic shapes

Use boxes for components and systems:

```text
+------------------+
|   COMPONENT      |
+------------------+
```

Use arrows for direction:

```text
CLIENT
  |
  | HTTPS
  v
SERVICE
```

Use labels on edges where they add meaning:

```text
SERVICE
  |
  | INSERT / SELECT
  v
DATABASE
```

## Context diagram

Show the system and the material things outside it.

Example:

```text
                 +------------------+
                 |      Client      |
                 +---------+--------+
                           |
                           | HTTPS
                           v
                 +------------------+
                 |   Application    |
                 +----+--------+----+
                      |        |
                reads |        | calls
                      v        v
                +---------+  +---------+
                |  Store  |  | Service |
                +---------+  +---------+
```

## Component diagram

Use when internal decomposition matters.

```text
+--------------------------------------------------+
|                  APPLICATION                     |
|                                                  |
|  +----------+    +-----------+    +-----------+  |
|  | Handler  |--->|  Service  |--->| Repository|  |
|  +----------+    +-----------+    +-----------+  |
|                        |                 |         |
+------------------------|-----------------|---------+
                         |                 |
                         v                 v
                  +-----------+      +-----------+
                  | External  |      | Database  |
                  | API       |      +-----------+
                  +-----------+
```

## Runtime-flow diagram

Show the order of a representative path.

```text
REQUEST
  |
  v
VALIDATE
  |
  +-- invalid --> RETURN 400
  |
  v
LOAD STATE
  |
  v
CALL DEPENDENCY
  |
  +-- error --> RETURN / RETRY / FAIL
  |
  v
WRITE RESULT
  |
  v
RESPOND
```

## State diagram

Use only when state transitions are material.

```text
PENDING
   |
   | accepted
   v
ACTIVE
   |
   | cancelled
   v
CLOSED
```

## Trust boundary

If the repository clearly establishes a trust boundary, mark it explicitly.

```text
UNTRUSTED INPUT
      |
      v
---------------- TRUST BOUNDARY ----------------
      |
      v
VALIDATION / AUTHENTICATION
      |
      v
INTERNAL SERVICE
```

Do not draw a trust boundary merely because one would be desirable.

## Evidence labels

Where useful, append a short evidence note below the diagram:

```text
Evidence: cmd/server/main.go, internal/http/routes.go, internal/store/sql.go
```

If a relationship is inferred, label it:

```text
SERVICE -----> CACHE
        inferred from client construction; call path not located
```

## Size rule

A diagram has failed if it becomes harder to understand than the prose it replaces.

Split large diagrams into context, component and flow views instead of expanding one diagram indefinitely.
