# Given / When / Then Extension

The While–Then / When–Then notation gives a compact behavioral summary. A complementary **Given / When / Then** form expands that summary into explicit state/event transition rules.

## Mapping

```text
Given <current state / state predicate>
When  <input event>
Then  <state transition>
      <Action Policy / output>
```

This corresponds directly to:

```text
(State, Event) -> (Next State, Output)
```

or to the split Mealy functions:

```text
T : S × Σ -> S
G : S × Σ -> O
```

with:

```text
Given  -> S
When   -> Σ
Then   -> T + G
```

## debounceTime example

Compact behavior:

```text
while silence timer active
then replace pending value and restart the timer on each source value

when configured silence interval elapses
then emit the pending latest value

when source completes
then emit the pending latest value if present and complete

when source errors
then discard the pending value and error immediately
```

Expanded transition rules:

```text
Given silence timer active
When source value arrives
Then replace pending value
     restart timer
     emit nothing

Given pending value exists
When silence interval elapses
Then emit pending latest value
     clear pending value

Given pending value exists
When source completes
Then emit pending latest value
     clear pending value
     complete

Given any state
When source errors
Then discard pending value
     error immediately
```

## Compound state predicates

```text
Given condition A
And condition B
When event X
Then ...
```

means the transition applies only when `A ∧ B` holds.

## Why keep both notations?

- **While–Then / When–Then** is optimized for concise operator-level behavioral summaries.
- **Given / When / Then** is optimized for explicit transition rules.
- The explicit rules can be mapped directly into transition tables and tests.

The companion repository [rxjs-bdd](https://github.com/hansschenker/rxjs-bdd) develops this Given / When / Then layer further.
