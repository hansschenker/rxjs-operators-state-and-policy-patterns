# RxJS Operators: State and Policy Patterns

A behavioral catalog of the RxJS 7.x operator implementation modules using a compact **While–Then / When–Then Pattern**.

## Core idea

> **RxJS Operator Behavior = State Transition Pattern + Action Policy Pattern**

The notation separates two questions:

```text
while <state/condition> then <state pattern>
when  <trigger/event> then <action policy>
```

- **while** describes what the operator remembers, maintains, replaces, queues, counts, or tracks.
- **when** describes what the operator does when a relevant input, boundary, or lifecycle event occurs.

The canonical motivating example is `audit`:

```text
while duration active then replace pending value
when duration ends then emit pending latest value
```

This is deliberately a behavioral notation, not a replacement for the complete RxJS contract. Error, completion, subscription, teardown, scheduler, and configuration semantics remain part of the full operator behavior and are called out where they materially distinguish an operator.

## Why this notation

Marble diagrams show examples of values over time. The While–Then / When–Then Pattern tries to state the **machine rule** that produces those examples.

It is useful for:

- teaching operator behavior;
- comparing similar operators;
- deriving state-machine models;
- designing transition-table tests;
- identifying concurrency and cancellation policies;
- separating dataflow mechanics from domain functions.

## Source baseline

The catalog tracks the modules in:

`ReactiveX/rxjs` → branch `7.x` → `src/internal/operators`

At the time this catalog was generated, that directory contained **116 operator-module `.ts` files**, excluding `OperatorSubscriber.ts`, which is operator infrastructure rather than an operator module.

The source tree contains public operators, deprecated aliases/wrappers, and internal implementation helpers. All three categories are retained in the catalog so the index mirrors the source directory.

See [docs/operators.md](docs/operators.md).

## General behavioral template

```text
Operator: <name>

while <state/condition> then <state transition / state-maintenance pattern>
when  <trigger/event> then <Action Policy>
```

For stateless operators, the state pattern can be explicitly trivial:

```text
map

while source active then keep no operator-specific state
when  source value arrives then emit project(value)
```

For higher-order operators, `when` exposes the concurrency policy:

```text
switchMap

while one inner is active then remember the current inner subscription
when  a new outer value arrives then cancel the current inner and start the new inner
```

## Contributors

- **hansschenker** — project author and While–Then / When–Then Pattern direction.
- **OpenAI GPT-5.6 Sol** — formalization of the While–Then / When–Then Pattern and initial RxJS 7.x operator behavior catalog.

## Attribution

RxJS is maintained by the ReactiveX/RxJS project. This repository is an independent behavioral-analysis and teaching project and is not an official RxJS repository.
