# Report

Read during step 9. Keep it evidence-first and short per section. Offer `html-lab` when the user wants a shareable artifact.

## Sections

**Summary.** What changed, why, and the maintenance and product impact.

**Baseline.** Initial gate results, pre-existing failures, environment limits.

**Old shape.** Previous entry points, call paths, ownership problems, duplicated or speculative machinery.

**New shape.** Public interface, domain operations, call paths, module ownership, persistence and validation boundaries.

**Removed.** Concepts, states, branches, wrappers, files deleted, files consolidated, exports, dependencies, configuration, compatibility layers. Not a line count.

**Interface.** Flow corrections, visual consistency, accessibility, responsiveness, feedback, animation fixes, gallery coverage. Omit when the repo ships no UI.

**Public API.** Added, removed, renamed. Breaking changes with migration instructions.

**Delta.** When measurable: source and test line change, files added and deleted, tests added and replaced, dependency delta. State plainly that line reduction is not the success metric.

**Verification.** Per executed check: command, result, relevant output. Separate automated from manual. Separate fixed baseline failures, remaining baseline failures, new failures, and manual checks still owed.

**Docs and tracking.** Docs updated, removed, or archived. Specs and tickets updated or created. Historical plans marked superseded.

**Git.** Branch, commits, push status, PR status, links when they exist. If push or PR was impossible, give the exact remaining command and the blocker.

**Risks.** Unverified behavior, hardware, network, or credential dependent checks, manual verification owed, known limitations.

**Deliberately omitted machinery.** Each mechanism considered and not added — dynamic registry, queue, custom cancellation, retry state machine, compatibility wrapper, extra configuration, DI layer, distributed lock, extension point — with the concrete future condition that would justify reconsidering it.

## Tracker Rules

Keep issues open unless the user authorized closing them. Update active items to the new architecture, drop obsolete implementation instructions, keep unresolved product decisions visible, add acceptance criteria when missing. Preserve historical plans as history; note that direction changed and link the replacement. Never rewrite an old plan to look consistent with the new implementation.

File a ticket only for genuine deferred work: product decisions that cannot be inferred, external blockers, manual integration, migrations outside the safe scope. Never as a substitute for work this pass could finish.
