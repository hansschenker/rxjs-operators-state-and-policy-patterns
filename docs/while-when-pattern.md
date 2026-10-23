# The While–Then / When–Then Pattern

## Definition

The While–Then / When–Then Pattern is a compact notation for describing an RxJS operator as a state machine with an outward action policy.

```text
while <state/condition> then <State Pattern>
when  <trigger/event> then <Action Policy>
```

The interpretation is:

- **while** — what state is maintained or transformed while the machine is in a condition;
- **when** — what outward action is selected when a relevant event occurs.

The underlying machine equation is:

```text
State + Event -> New State + Action
```

and the teaching-level decomposition is:

```text
RxJS Operator Behavior
= State Transition Pattern
+ Action Policy Pattern
```

## Canonical example: audit

```text
while duration active then replace pending value
when duration ends then emit pending latest value
```

Expanded:

```text
idle + source(value)
  -> auditing(value) + start duration

auditing(previous) + source(value)
  -> auditing(value) + emit nothing

auditing(latest) + duration-end
  -> idle + emit latest
```

The two-line notation deliberately compresses the transition table into the operator's characteristic behavior.

## Event vocabulary

Depending on the operator, meaningful events can include:

- source `next(value)`;
- source `complete`;
- source `error`;
- duration/timer next or completion;
- notifier next;
- outer next;
- inner next / complete / error;
- subscription;
- unsubscription / teardown;
- scheduler action.

## Action vocabulary

Typical Action Policies include:

- emit / forward;
- suppress;
- transform then emit;
- store / replace;
- buffer / flush;
- queue;
- start inner;
- cancel inner;
- ignore new work;
- subscribe / resubscribe;
- complete;
- error;
- switch to fallback;
- open / close a window;
- connect / disconnect shared execution.

## Scope

The two-line form captures the characteristic behavior of an operator. It is not intended to erase configuration-sensitive details. For operators whose semantics vary materially with options—such as `throttle`, `share`, `repeat`, `retry`, `timeout`, or `groupBy`—the catalog states the default or central policy and notes the configurable boundary.
