# Deletion Audit

Read during step 4. Use as a hunting list, not a checklist to fill.

## Per-Finding Record

For each finding worth acting on:

1. ID and group: `D` safe deletion, `R` refactor, `M` needs a migration plan.
2. Exact paths: files, symbols, line ranges, and the code path from entry point (route, command, job, UI event) to the code in question.
3. Requirement it supposedly serves, and whether that requirement is real, historical, accidental, or speculative.
4. Why it is unnecessary or more complex than needed, in one or two sentences.
5. Evidence: searches run and their result, callers found or absent, dynamic-access check, tests, git history, runtime or data observation.
6. What can be deleted, and the smallest valid replacement.
7. Risk, split into behavior, data, and deployment. Write "none found" only after you looked.
8. Confidence per the scale below.
9. Estimated removable lines, net of the replacement.
10. Verification that proves the change.
11. Concrete future condition that would justify adding the machinery back.

## Groups

- **Safe deletion (D):** evidence bar met, no data or schema change, no public contract change. Revert is a plain code revert.
- **Refactor (R):** behavior stays, shape changes: merge duplicates, inline a one-caller wrapper, replace a registry with a switch, collapse layers. Needs the retained behavior covered before and after.
- **Needs migration plan (M):** touches schema, stored data, applied migration history, a public API, a config or file format in the field, or deploy order. Always carries a plan with steps, verification, and rollback.

A finding goes to the strictest group any part of it needs. Split it when one part is safe and another needs a plan.

## Confidence

- **High:** evidence bar met. Static search, dynamic-access check, and config or runtime evidence all agree. No external consumer is possible, or you checked it.
- **Medium:** static evidence is clean, but one gap remains: reflection or string dispatch not ruled out, an external consumer is possible, or production data or applied-state is not visible. Name the gap and the check that closes it.
- **Low:** a smell or a pattern with incomplete evidence. Report it as a lead with the next check. Never plan a deletion on it.

Only high-confidence findings go into the safe-deletion group.

## Line Estimate

- Count lines per finding: whole files deleted, plus line ranges removed, minus replacement lines. Use `git ls-files` with a line count, or `codebase-inspection`, not guesses.
- Report separately: source, tests, migrations and schema, config and scripts, docs. Exclude generated files and lockfiles, or list them apart.
- Sum per group and in total. Mark it as an estimate. It informs the plan; it is not the goal.

## Ranking

Order candidates by deletion value against behavioral risk. Break ties with maintenance cost, migration complexity, user impact, and confidence. Land high-value low-risk slices first so later slices inherit a smaller repo.

Delete whole systems when the evidence supports it. A subsystem with no live entry point goes out as one finding, not as fifty small ones.

## Dead And Unreachable

- Routes, handlers, jobs, commands, or listeners defined but never mounted, registered, or scheduled
- Feature flags that are always on or always off in every environment; remove the flag and the dead branch
- Branches on constants, impossible states, or environment names that no longer exist
- Modules reachable only from tests, stories, or other dead code
- Commented-out code and disabled files kept "for later"
- Whole subsystems whose only entry point was removed

Before calling any of these dead, check: string-based routing, reflection, dependency-injection containers, plugin manifests, config files, cron and job schedulers, CLI registration, templates, serverless and framework file-based routing, and other repos or services that call this one.

## Duplicate Logic

- Two or more implementations of the same validation, formatting, mapping, query, or client call
- Parallel DTOs, models, or schemas for the same entity inside one boundary
- Copy-pasted modules that drifted apart; compare them before merging, the drift may be a bug or a real difference

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

## Obsolete Compatibility

- Shims for old API versions, old clients, old SDKs, or deprecated config keys with no remaining user
- Readers for old file or data formats that no stored data still uses
- Dual-read or dual-write paths left after a finished cutover
- Polyfills for runtimes the repo no longer supports

Check the supported-version policy, release notes, telemetry, and stored data before you call a layer obsolete. Anything that touches stored data or schema follows [migrations.md](migrations.md).

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
