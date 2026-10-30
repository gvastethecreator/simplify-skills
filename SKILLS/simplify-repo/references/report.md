# Report

Read during step 9 in implement mode. Audit-only mode uses [audit-report.md](audit-report.md). Keep it evidence-first and short per section. If HTML is requested, render the same facts as a self-contained document with escaped content and links to the evidence.

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

Include a deferred design decision only when it affects the result or the user needs to choose it.

## Tracker Rules

Keep issues open unless the user authorized closing them. Update active items only within the tracker contract and authorized scope. Preserve historical plans as history; never rewrite an old plan to make it look consistent with the new implementation.

Use the existing tracker for genuine deferred work only when its contract requires it and the action is authorized. Never file a ticket as a substitute for work this pass could finish.
