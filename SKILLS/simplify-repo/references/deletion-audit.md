# Deletion Audit

Read during step 4. Use as a hunting list, not a checklist to fill.

## Per-Finding Record

For each finding worth acting on:

1. Requirement it supposedly serves.
2. Whether that requirement is real, historical, accidental, or speculative.
3. Evidence: callers found, callers absent, tests, git history, runtime observation.
4. What can be deleted.
5. Smallest valid replacement.
6. Remaining risk.
7. Verification that proves the replacement.
8. Concrete future condition that would justify adding the machinery back.

## Ranking

Order candidates by deletion value against behavioral risk. Break ties with maintenance cost, migration complexity, user impact, and confidence. Land high-value low-risk slices first so later slices inherit a smaller repo.

## Indirection And Ceremony

- Generic command, `execute`, `dispatch`, or `operation` APIs hiding a small fixed set of domain verbs
- Result envelopes, receipts, confirmations, ordinals, timestamps, or metadata no caller reads
- Wrapper functions that only rename another function
- Catch-and-rethrow that adds no context
- Interfaces with one implementation, factories with no construction choice, dependency injection with no substitution
- Deep inheritance where a function would do
- Boolean arguments selecting unrelated behaviors
- Generic utility modules whose logic has one caller
- Tiny packages that exist to preserve an architecture diagram
- Public types with no external consumer

## Lifecycle And Concurrency

- Queues where a sequential call or a busy error is enough
- Retry or reconnect state machines with no measured failure case
- Multiple flags describing the same connection or operation state
- Custom cancellation or timeout frameworks duplicating `AbortSignal` or the platform equivalent
- Locks, PID files, stale-owner recovery, wait loops, or ownership tokens in a single-process product
- Distributed coordination for a product that intentionally runs once

## Duplicate Ownership

- Two owners for the same config, registry, schema, parser, serializer, or path resolution
- Repeated normalization across trusted internal layers
- Defensive cloning or "safe JSON" of application-controlled data
- Repeated null, type, or state checks after a successful validation boundary
- State that mirrors another object instead of deriving from it

## Speculative Surface

- Dynamic adapter registration for a fixed implementation set
- Capability maps for static features
- Approval gates for an action the caller already chose
- Extension points with no extension, placeholder modules, empty packages
- Future-facing interfaces with no current implementation requirement
- Configuration values that never vary
- Compatibility code for unsupported or unused formats
- Parallel old and new implementations

## Reinvention

- Hand-rolled file writing, WebSocket, CLI parsing, locking, or serialization that the platform or an installed dependency already covers
- Error taxonomies that only rename low-level failures
- Logging wrappers where shell redirection or one script suffices

## Tests, Docs, Deps

- Tests asserting wrapper existence, factory usage, private call order, or deleted abstractions
- Fixtures for removed machinery
- Comments explaining an abstraction that should not survive
- Docs describing the deleted design as current
- Dependencies unused, duplicate in purpose, used only by deleted code, obsolete polyfills, deprecated with a safe supported replacement, or replaceable by a trivial platform capability

For dependency upgrades: read the official migration guide, name the breaking changes, change code intentionally, verify focused, and record deferred upgrades with their blocker. Do not bump every package to its newest major.
