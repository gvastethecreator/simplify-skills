---
name: simplify-tests
description: "Test reduction: prune low-value tests, simplify fixtures and mocks, and reduce redundant test runs. Use for bloated suites or disproportionate testing in small projects."
---

# Simplify Tests

Reduce the cost of detecting useful failures. Fewer files or test names alone do not prove improvement. Work within the requested project or suite; a review reports candidates, while a cleanup request authorizes supported local edits.

## 1. Find the cost and the contract

- Inspect Git state, project rules, test scripts, CI configuration, and the affected tests with their production callers. Follow an existing current code map when the repo requires it.
- Use existing logs and timings first. Identify duplicated commands, slow setup, unreliable tests, and assertions that churn with internal refactors. Do not start with a full-suite or coverage run merely to obtain a baseline.
- For a small project, start with its main behavior and reported problems. Use a short candidate list; no new scoring system, dashboard, coverage campaign, or test framework.
- Separate required release/CI gates from habitual local commands. Keep mandatory gates unless the user authorized changing that policy.
- Done: the bounded target, costly work, useful contracts, and required gates are clear.

## 2. Decide what earns its cost

For each candidate, ask: **Which plausible product failure does this detect, and where else is that same failure detected?** Read assertions and setup; filenames and coverage percentages are insufficient.

- **Keep** a unique check of supported behavior, a past bug that can recur, or a material boundary such as permissions, persistence, money, or recovery. Small projects can still have high-risk behavior.
- **Remove** an exact duplicate, a check of removed behavior, an assertion that only restates a constant/type/library guarantee, or a test that cannot fail when the claimed product behavior breaks. A constant or default that carries a real permissions, persistence, or protocol contract can still need protection.
- **Consolidate** cases only when their setup, action, and failure class are equivalent. Keep distinct error causes and boundary conditions distinguishable. A giant test or a large parameter table is not a reduction in work.
- **Simplify** excessive mocks, fixture graphs, snapshots of implementation details, and test-only abstractions. Prefer a direct assertion at the cheapest existing public seam that observes the failure.
- A flaky or slow test may protect unique behavior. Fix its owning setup or retain it with the problem recorded; do not delete or skip it just to make the suite green. For suite-only failures, use `test-suite-diagnostics` only when diagnosis is needed.
- Done: each removal has evidence of duplication, obsolescence, or lack of useful signal. Retain uncertain cases and state the evidence gap.

## 3. Apply one coherent reduction

- Preserve unrelated work and production behavior. Remove the supported candidates and their now-unused fixtures, helpers, snapshots, imports, or scripts after checking consumers.
- Reuse or improve a retained test before adding another. Add a replacement only when it preserves useful behavior coverage more simply; zero new tests is a valid result.
- Keep setup, action, and assertions together. Use existing helpers when they shorten the case; a little clear duplication can cost less than a generic test DSL or factory system.
- Reduce repeated local commands and overlapping test layers within scope. Choose one aggregate command or its relevant parts, not both. Changing CI, required coverage thresholds, or release gates needs that scope and `simplify-ci`; do not weaken them to hide removed proof.
- Do not introduce a runner, dependency, fixture framework, production API, or architecture refactor to make this cleanup possible. Stop when further reduction would lose distinct protection or add complexity.
- Done: the diff removes maintenance or execution work and preserves each retained contract.

## 4. Verify once and report the limits

- Review existing coverage before choosing final checks. Run the smallest retained selection that observes the changed test setup and preserved behavior; check test discovery when files, names, or filters changed. Zero selected tests is not a pass.
- Use the full suite only for an explicit request, a mandatory gate, or a shared setup/runner change whose effects cannot be bounded. Reuse valid results; rerun only checks invalidated by a repair. Do not add coverage, mutation testing, shuffling, stress loops, or repeated benchmarks by default.
- Compare removed/retained behavior and setup complexity. Report counts when readily available. Claim a runtime gain only from comparable commands and environments; fewer tests alone is not timing evidence.
- Inspect the changed path for orphaned references and accidental coverage gaps. Report what was removed or simplified, what still protects the behavior, checks run or skipped, and any retained uncertainty.
- Done: reduction has evidence, discovery still works where changed, and the result states the limits of verification.

## Ordinary work after cleanup

Default to **no new test** until there is a meaningful uncovered behavior or reproduced failure that is worth maintaining. Extend one nearby case before creating a new file. A focused manual or existing executable check can be enough for a small reversible change; it does not count as automated coverage. Use test-first cycles only for an explicit TDD request. Prefer the smallest useful check set at final integration, then stop.
