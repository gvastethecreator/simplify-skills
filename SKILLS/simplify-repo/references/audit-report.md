# Audit Report

Read at the end of step 5 in audit-only mode. Evidence first, short per finding. The report is the deliverable and the ledger for a later implement pass.

Deliver findings in chat or the existing working record. Create a durable report or plan only when requested or required by the tracker contract; use simple English. If HTML is requested, render the findings with escaped content and evidence links.

## Sections

**Scope and coverage.** Repo and commit or merge base. What was traced: flows, entry points, routes, modules, dependencies, schema, migrations. What was not inspected and why: no database access, generated code, external consumers, private services. Tools and searches used. Baseline evidence reused.

**System map.** Short: entry points and routes, main flows, module and persistence owners, schema source, migration runner, supported environments. Only what the findings need.

**Summary table.** Use when it helps compare findings or is requested:

| ID | Group | Finding | Paths | Confidence | Est. lines | Risk |
|----|-------|---------|-------|------------|-----------|------|

Sort by group (D, R, M), then by rank.

**Findings.** Grouped as:

1. Safe deletions (D)
2. Refactors (R)
3. Needs a migration plan (M)

Each finding uses the per-finding record in [deletion-audit.md](deletion-audit.md): exact files and code paths, why it is unnecessary or too complex, what to delete or simplify, behavior, data, and deployment risk, evidence, confidence, estimated lines, verification.

**Checked and kept.** Candidates that looked dead or redundant but have a real use, with the evidence. This stops the next audit from repeating the search.

**Leads.** Low-confidence items with the check that would confirm or clear each one.

**Line estimate.** When requested or useful, estimate from scoped evidence and label it as an estimate, not a goal.

**Ordered cleanup plan.** Small, reviewable steps; one step is one reviewable change. Order:

1. High-confidence safe deletions that shrink the surface for later steps.
2. Refactors unlocked by those deletions.
3. Migration-plan changes, as expand and contract steps per [migrations.md](migrations.md).

Per step:

- findings covered and files touched
- depends on which earlier step
- checks after the step: choose the smallest relevant existing checks; include database or runtime proof when the changed contract requires it. Name exact commands when known.
- rollback: plain revert, or the data rollback for M steps
- decision or sign-off needed before the step, if any

**Decisions owed.** Product, data, or ownership questions only the user or an owner can answer, each with the smallest decision needed and the steps it blocks.

## Rules

- No finding without exact paths and evidence.
- Do not pad the report with style nits, formatting, or naming preferences.
- Do not recommend new machinery unless a finding needs it to delete more than it adds.
- State plainly what was not verified.
