# RxJS 7.x Operator While–Then / When–Then Catalog

This catalog mirrors the `.ts` modules in `ReactiveX/rxjs` branch `7.x`, directory `src/internal/operators`, excluding `OperatorSubscriber.ts`.

Each entry uses:

```text
while <state/condition> then <State Pattern>
when  <trigger/event> then <Action Policy>
```

The wording focuses on the operator's **characteristic machine behavior**. It is intentionally more compact than the complete implementation contract.

---

## `audit`

**while** duration active then replace the pending value with each new source value

**when** duration ends then emit the pending latest value and return to idle

*Note:* The first source value starts the duration.

---

## `auditTime`

**while** fixed audit interval active then replace the pending value with each new source value

**when** interval ends then emit the pending latest value and return to idle

*Note:* Time-based specialization of `audit`.

---

## `buffer`

**while** buffer open then append each source value to the current array

**when** closing notifier emits then emit the current array and start a fresh buffer

---

## `bufferCount`

**while** buffer(s) open then append source values and track size/start positions

**when** a buffer reaches its configured size then emit that buffer; open later buffers according to `startBufferEvery`

---

## `bufferTime`

**while** time-span buffer(s) open then append source values to every active buffer

**when** a buffer times out or reaches `maxBufferSize` then emit it and open replacements according to configuration

---

## `bufferToggle`

**while** one or more buffers open then route each source value into every active buffer

**when** an opening arrives then open a buffer; its closing notifier fires: emit and close that buffer

---

## `bufferWhen`

**while** current buffer open then append each source value

**when** closing selector's notifier emits then emit current buffer and immediately open the next buffer

---

## `catchError`

**while** source active normally then forward values without recovery state

**when** source errors then invoke selector and subscribe to the replacement Observable instead of forwarding the error

---

## `combineAll`

**while** outer active then collect inner Observables until the outer completes

**when** outer completes then subscribe/combine inners and emit whenever all have values and any inner updates

*Note:* Deprecated alias behavior built on `combineLatestAll`.

---

## `combineLatest`

**while** source plus supplied inputs active then retain the latest value from each input and a readiness mask

**when** every input has emitted at least once and any input emits then emit the tuple/projected latest combination

*Note:* Deprecated pipeable form.

---

## `combineLatestAll`

**while** combined inners active then retain each inner's latest value and readiness

**when** all active inners are ready and any inner emits then emit the latest-value tuple

*Note:* Starts combining after the higher-order source supplies/collects inners per implementation.

---

## `combineLatestWith`

**while** inputs active then retain the latest value from source and each supplied Observable

**when** all inputs have emitted and any input emits then emit the latest-value tuple

---

## `concat`

**while** current input Observable active then keep later supplied Observables pending

**when** current input completes then subscribe to the next; forward values from only the current input

*Note:* Deprecated pipeable form.

---

## `concatAll`

**while** one inner active then queue later inner Observables

**when** current inner completes then subscribe to the next queued inner; forward inner values in order

---

## `concatMap`

**while** one projected inner active then queue new outer values

**when** current inner completes then project and subscribe to the next queued outer value

*Note:* Concurrency policy: queue.

---

## `concatMapTo`

**while** one fixed inner active then queue new outer values

**when** current inner completes then subscribe to another instance of the configured inner for the next queued outer value

*Note:* Deprecated specialization.

---

## `concatWith`

**while** current Observable active then keep following Observables pending

**when** current Observable completes then subscribe to the next and continue forwarding

---

## `connect`

**while** shared connector active inside the selector scope then multicast one source execution

**when** selector output is subscribed then connect the shared source and route it through the selector result

---

## `count`

**while** source active then increment count for every value satisfying the predicate (or every value without one)

**when** source completes then emit the final count and complete

---

## `debounce`

**while** duration active for pending value then replace pending value and cancel/restart its duration on each source value

**when** current duration emits/completes before another source value then emit the pending latest value

---

## `debounceTime`

**while** silence timer active then replace pending value and restart the timer on each source value

**when** configured silence interval elapses then emit the pending latest value

---

## `defaultIfEmpty`

**while** source active then remember whether any value has been seen

**when** source completes with no value seen then emit the default value, then complete

*Note:* If values were seen, they were forwarded normally.

---

## `delay`

**while** notifications pending then schedule each source notification for later delivery

**when** its scheduled due time arrives then forward that notification

*Note:* Implemented via `delayWhen`/timer semantics in RxJS 7.x.

---

## `delayWhen`

**while** source values pending then associate each value with a duration Observable

**when** that value's duration emits then release/emit the corresponding source value

*Note:* Subscription delay may also defer source subscription.

---

## `dematerialize`

**while** source active then treat each incoming `Notification` object as protocol data

**when** a notification object arrives then execute it as next/error/complete on the destination

---

## `distinct`

**while** source active then keep a set of previously seen keys

**when** a value's key has not been seen then remember the key and emit the value; otherwise emit nothing

*Note:* Optional flush Observable clears remembered keys.

---

## `distinctUntilChanged`

**while** source active after first value then remember the last emitted value/key

**when** first value arrives or comparator says current differs from previous then emit and replace remembered value/key

---

## `distinctUntilKeyChanged`

**while** source active after first value then remember the selected key of the last emitted object

**when** first value arrives or selected key differs then emit the object and replace remembered key

---

## `elementAt`

**while** source active then count source positions

**when** configured index arrives then emit that value and complete; source completes too early: emit default or error

---

## `endWith`

**while** source active then forward source values

**when** source completes then emit configured trailing values in order, then complete

---

## `every`

**while** source active then remain in 'all passed' state while predicate is true

**when** a value fails predicate then emit false and complete; source completes without failure: emit true

---

## `exhaust`

**while** one inner active then ignore newly arriving inner Observables

**when** current inner completes then become idle and allow the next outer inner Observable to be subscribed

---

## `exhaustAll`

**while** one inner active then ignore newly arriving inner Observables

**when** current inner completes then become idle and allow the next inner

*Note:* Alias-style specialization of exhaust semantics.

---

## `exhaustMap`

**while** one projected inner active then ignore new outer values

**when** current inner completes then become idle; next outer value may start a new inner

*Note:* Concurrency policy: ignore while busy.

---

## `expand`

**while** expansion active then feed emitted values back through the projection while respecting concurrency

**when** a value is accepted for expansion then emit it and subscribe to its projected Observable recursively

---

## `filter`

**while** source active then keep no operator-specific state except index

**when** a source value arrives then emit it only if predicate(value, index) is true

---

## `finalize`

**while** subscription active then retain the finalizer for teardown

**when** source terminates or subscriber explicitly unsubscribes then invoke the finalizer exactly as teardown requires

---

## `find`

**while** source active then scan values until predicate matches

**when** first matching value arrives then emit it and complete; source completes without match: emit `undefined`

---

## `findIndex`

**while** source active then track index while scanning for first predicate match

**when** first match arrives then emit its index and complete; source completes without match: emit `-1`

---

## `first`

**while** source active then wait for first value satisfying predicate (or simply first value)

**when** first qualifying value arrives then emit and complete; source completes first: emit default or error

---

## `flatMap`

**while** projected inners active then use merge-style concurrency

**when** outer value arrives then project and subscribe according to `mergeMap` concurrency policy

*Note:* Deprecated alias of `mergeMap`.

---

## `groupBy`

**while** source active then maintain a map from keys to live grouped Subjects

**when** a value arrives then create group if needed and emit value into its group; emit each new group once to downstream

*Note:* Duration/connector options can close or customize groups.

---

## `ignoreElements`

**while** source active then maintain no value state

**when** source next arrives then emit nothing; source error/complete arrives: forward the terminal notification

---

## `isEmpty`

**while** source active before first value then remain 'possibly empty'

**when** first value arrives then emit false and complete; source completes first: emit true

---

## `joinAllInternals`

**while** inner collection/combine operation active then delegate state to chosen join strategy

**when** join strategy has enough inner values then emit projected joined result

*Note:* Internal helper used by higher-order join operators.

---

## `last`

**while** source active then retain latest value satisfying predicate (or latest value)

**when** source completes then emit retained last value; if none, emit default or error

---

## `map`

**while** source active then keep no operator-specific value state except index

**when** source value arrives then emit project(value, index)

---

## `mapTo`

**while** source active then keep no operator-specific value state

**when** source value arrives then emit the configured constant

*Note:* Deprecated specialization of `map`.

---

## `materialize`

**while** source active then convert protocol events into `Notification` values

**when** next/error/complete occurs then emit the corresponding notification object; after terminal notification value, complete normally

---

## `max`

**while** source active then retain the greatest value seen according to comparer

**when** source completes then emit retained maximum (or follow empty-reduction behavior)

*Note:* Implemented via reduction.

---

## `merge`

**while** multiple inputs active then allow concurrent subscriptions up to configured concurrency

**when** any active input emits then forward immediately; an input completes: admit pending input if capacity exists

*Note:* Deprecated pipeable form.

---

## `mergeAll`

**while** inners active then track active count and queue excess inners beyond concurrency limit

**when** an inner emits then forward immediately; capacity opens: subscribe to queued inner

---

## `mergeInternals`

**while** inners active then track active count, queue, completion state, and optional expansion

**when** outer/inner events occur then apply merge concurrency admission, forwarding, drain, and completion policy

*Note:* Internal engine behind several flattening operators.

---

## `mergeMap`

**while** projected inners active then allow overlap up to concurrency limit and queue excess outer values

**when** outer value arrives with capacity then start projected inner; inner emits: forward immediately

*Note:* Concurrency policy: allow overlap.

---

## `mergeMapTo`

**while** fixed inners active then allow overlap up to concurrency limit

**when** outer value arrives with capacity then subscribe to configured inner and forward its values

*Note:* Deprecated specialization.

---

## `mergeScan`

**while** projected accumulator inners active then retain accumulator and merge-style concurrency state

**when** outer value arrives then project `(acc, value)` to inner; inner emits: replace accumulator and emit it

---

## `mergeWith`

**while** source and supplied Observables are active then retain their concurrent subscriptions

**when** any input emits then forward immediately; complete after all inputs complete

---

## `min`

**while** source active then retain the least value seen according to comparer

**when** source completes then emit retained minimum (or follow empty-reduction behavior)

*Note:* Implemented via reduction.

---

## `multicast`

**while** connectable state active then route source through a Subject/Subject factory

**when** connection is established then subscribe subject to source; subscribers observe the shared subject

*Note:* Deprecated multicasting API.

---

## `observeOn`

**while** notifications pending on a scheduler then retain scheduled delivery tasks

**when** scheduled action executes then deliver the corresponding next/error/complete downstream

*Note:* Changes notification delivery context, not source production.

---

## `onErrorResumeNextWith`

**while** current Observable active then keep fallback Observables pending

**when** current source errors or completes then suppress the distinction and subscribe to the next fallback; complete after all finish

---

## `pairwise`

**while** source active after first value then remember previous source value

**when** next value arrives and previous exists then emit `[previous, current]`, then remember current

---

## `partition`

**while** each partition subscription active then apply complementary predicates independently

**when** source value arrives then one output forwards if predicate true, the other if false

*Note:* Deprecated pipeable partition form.

---

## `pluck`

**while** source active then keep no evolving operator state

**when** source value arrives then read configured property path and emit the result

*Note:* Deprecated; equivalent to `map` property access.

---

## `publish`

**while** connectable state active then multicast through a plain Subject

**when** connection starts then subscribe Subject to source and share that execution

*Note:* Deprecated wrapper around connectable/multicast semantics.

---

## `publishBehavior`

**while** connectable state active then multicast through BehaviorSubject retaining current value

**when** subscriber arrives then receive current retained value; connection forwards source into shared BehaviorSubject

*Note:* Deprecated.

---

## `publishLast`

**while** connectable state active then multicast through AsyncSubject retaining only final value

**when** source completes then emit retained last value to subscribers and complete

*Note:* Deprecated.

---

## `publishReplay`

**while** connectable state active then retain configured replay buffer/window in ReplaySubject

**when** subscriber arrives then replay retained notifications then receive live shared values

*Note:* Deprecated.

---

## `race`

**while** competitors pending before winner then subscribe to competing Observables

**when** first competitor emits then choose it as winner, unsubscribe losers, and forward only winner thereafter

*Note:* Deprecated pipeable form.

---

## `raceWith`

**while** competitors pending before winner then subscribe to source and supplied Observables

**when** first competitor emits then choose it, unsubscribe losers, forward winner only

---

## `reduce`

**while** source active then retain accumulator and whether accumulation has begun

**when** source completes then emit the final accumulator exactly once, then complete

*Note:* Unlike `scan`, no intermediate accumulator emissions.

---

## `refCount`

**while** shared connectable subscriber count > 0 then retain count and active connection

**when** first subscriber arrives then connect; last subscriber leaves: disconnect shared connection

---

## `repeat`

**while** source execution active then track repeat count/delay state

**when** source completes and repeats remain then resubscribe; otherwise forward completion

*Note:* Does not repeat on error.

---

## `repeatWhen`

**while** source execution active then route completion signals to notifier workflow

**when** notifier emits after a source completion then resubscribe to source; notifier completes/errors: terminate accordingly

*Note:* Deprecated-style notifier API.

---

## `retry`

**while** source execution active then track retry count/delay/reset configuration

**when** source errors and retry policy permits then resubscribe; otherwise forward final error

*Note:* Does not retry on normal completion.

---

## `retryWhen`

**while** source execution active then route error signals to notifier workflow

**when** notifier emits after an error then resubscribe; notifier completes/errors: terminate accordingly

*Note:* Deprecated-style notifier API.

---

## `sample`

**while** source active then remember latest source value and whether a new value exists since last sample

**when** sampler emits then emit remembered latest value if one is pending, then clear pending flag

---

## `sampleTime`

**while** source active then remember latest source value and pending flag between periodic sample ticks

**when** sampling interval ticks then emit remembered latest value if one is pending

---

## `scan`

**while** source active then retain accumulator

**when** source value arrives then update accumulator with reducer and emit the updated accumulator

*Note:* State is emitted on every reduction step.

---

## `scanInternals`

**while** source active then retain accumulator, index, and seed-state flag

**when** source value arrives then initialize/update accumulator and optionally emit according to helper configuration

*Note:* Internal helper behind `scan`/`reduce`.

---

## `sequenceEqual`

**while** two sequences active then queue unmatched values from each side and compare in order

**when** both sides provide the next pair then compare them; mismatch emits false and completes; matched completion emits true

---

## `share`

**while** shared subscription state active then retain connector, subscriber count, and reset state

**when** first subscriber needs source then connect; reset conditions occur: reset connector/connection according to configuration

---

## `shareReplay`

**while** shared replay state active then retain ReplaySubject buffer and shared connection state

**when** subscriber arrives then replay buffered values then receive live values; connection/reset follows `refCount` and terminal policy

---

## `single`

**while** source active then retain whether one matching value has been found and that value

**when** a second matching value arrives then error; source completes with exactly one match: emit it; otherwise empty/not-found error semantics apply

---

## `skip`

**while** source active then count seen values

**when** a value arrives after the first `count` values then emit it; earlier values emit nothing

---

## `skipLast`

**while** source active then maintain a queue/ring of the last `count` values

**when** a new value makes the buffer exceed `count` then emit the oldest buffered value

*Note:* Trailing `count` values are discarded on completion.

---

## `skipUntil`

**while** gate closed then ignore source values while waiting for notifier

**when** notifier emits then open gate permanently; subsequent source values are forwarded

*Note:* Notifier completion alone does not open the gate.

---

## `skipWhile`

**while** skipping state active then evaluate predicate for each source value

**when** predicate first becomes false then stop skipping permanently and emit that value plus all subsequent values

---

## `startWith`

**while** subscription begins then configured prefix values are pending before source

**when** output is subscribed then emit prefix values first, then subscribe/forward source

---

## `subscribeOn`

**while** source subscription is pending on a scheduler then keep the source subscription deferred

**when** scheduled subscription action executes then subscribe to the source

*Note:* Moves subscription side effect in time/context.

---

## `switchAll`

**while** one current inner active then remember only the latest inner subscription

**when** new inner Observable arrives then cancel previous inner and subscribe to the new one; forward only current inner

---

## `switchMap`

**while** one projected inner active then remember only the latest inner subscription

**when** new outer value arrives then cancel current inner, project, and subscribe to the new inner

*Note:* Concurrency policy: only latest.

---

## `switchMapTo`

**while** one fixed inner active then remember only latest inner subscription instance

**when** new outer value arrives then cancel current inner and subscribe to the configured inner again

*Note:* Deprecated specialization.

---

## `switchScan`

**while** one projected accumulator inner active then retain accumulator and current inner

**when** new outer value arrives then cancel current inner and start projection from current accumulator; inner emissions replace and emit accumulator

---

## `take`

**while** source active then count forwarded values up to limit

**when** a value arrives within limit then emit it; configured count is reached: complete and unsubscribe upstream

---

## `takeLast`

**while** source active then retain a bounded buffer of the last `count` values

**when** source completes then emit buffered trailing values in order, then complete

---

## `takeUntil`

**while** source active then forward values while notifier has not emitted

**when** notifier emits then complete output immediately and unsubscribe source

---

## `takeWhile`

**while** taking state active then evaluate predicate for each value

**when** predicate becomes false then optionally emit boundary value if `inclusive`, then complete; otherwise emit qualifying values

---

## `tap`

**while** source active then preserve source values while invoking side-effect observers

**when** next/error/complete/subscription/finalize events occur then invoke configured side effect and otherwise mirror source protocol

---

## `throttle`

**while** duration active then suppress new source values, optionally retaining a trailing value depending on config

**when** leading/trailing boundary occurs then emit according to `{leading, trailing}` policy and manage the duration

*Note:* Default is leading `true`, trailing `false`.

---

## `throttleTime`

**while** fixed throttle interval active then suppress new source values, optionally retain trailing value

**when** interval boundary occurs then emit according to leading/trailing configuration and reopen as required

---

## `throwIfEmpty`

**while** source active then remember whether any value has been seen

**when** source completes empty then error using configured error factory; otherwise complete normally after forwarding values

---

## `timeInterval`

**while** source active then remember scheduler timestamp of previous emission/subscription baseline

**when** source value arrives then emit `{value, interval}` using elapsed scheduler time and update timestamp

---

## `timeout`

**while** waiting state active then track deadline for first/each value using scheduler

**when** deadline expires before required source emission then unsubscribe source and switch to configured fallback or throw TimeoutError

---

## `timeoutWith`

**while** waiting state active then track timeout deadline

**when** deadline expires then unsubscribe source and subscribe to configured fallback Observable

*Note:* Deprecated wrapper around `timeout`.

---

## `timestamp`

**while** source active then keep no evolving value state beyond scheduler access

**when** source value arrives then emit `{value, timestamp: scheduler.now()}`

---

## `toArray`

**while** source active then append every source value to one array

**when** source completes then emit the accumulated array once, then complete

---

## `window`

**while** current window open then forward each source value into current window Subject

**when** boundary Observable emits then close current window, emit/open a new window

---

## `windowCount`

**while** one or more count-based windows open then route source values into active windows and track counts

**when** start/size boundary is reached then open or close windows according to `windowSize` and `startWindowEvery`

---

## `windowTime`

**while** time-based windows open then route values into all active windows and track age/count

**when** time span, creation interval, or max size boundary occurs then close/open windows according to configuration

---

## `windowToggle`

**while** one or more windows open then route each source value into all active windows

**when** opening emits then create/emit a new window; its closing notifier fires: close that window

---

## `windowWhen`

**while** current window open then route source values into it

**when** closing notifier fires then close current window and immediately create/emit the next window

---

## `withLatestFrom`

**while** source active then retain latest values/readiness from all secondary Observables

**when** source emits and every secondary has emitted at least once then emit source value combined with latest secondary values

*Note:* Only the source triggers output.

---

## `zip`

**while** inputs active then maintain one FIFO queue per input

**when** every queue has at least one value then dequeue one from each and emit the tuple/projected result

*Note:* Deprecated pipeable form.

---

## `zipAll`

**while** collected inner inputs active then maintain one FIFO queue per inner

**when** every inner queue has a value then dequeue one from each and emit tuple; terminate when further full tuples become impossible

---

## `zipWith`

**while** source and supplied inputs active then maintain one FIFO queue per input

**when** every queue has at least one value then dequeue one from each and emit the tuple

---
