---
name: simplify-ci
description: "CI reduction: prune overlapping jobs, matrices, extra runtimes, and heavy GitHub Actions so every project keeps the lightest required pipeline. Use for bloated workflows, slow checks, duplicate lint/test/build jobs, or when adding CI."
---

# Simplify CI

Keep the cheapest pipeline that still proves required gates. Job count, matrices, and extra runtimes are not quality. Work within the requested repo; a review reports candidates, while a cleanup request authorizes supported local edits.

Failing checks are `fix-ci`. Do not delete a red job to obtain green. Suite reduction is `simplify-tests`. Actions enable/disable and token permissions are `github-hygiene`.

## Default for any project

Start from nothing, then add only a required gate.

- Private personal repo: keep Actions disabled unless the user asked to enable it.
- No workflow until a public merge, release, or documented required check exists. A small or docs-only tree can stay without CI.
- One workflow file. One job. `ubuntu-latest`. One existing local command, or the smallest subset it already includes.
- Reuse package manager, lockfile, and documented scripts. Do not invent a second lint, type, test, build, coverage, or audit layer.
- Skip OS, version, and shard matrices. Skip coverage, mutation, benchmarks, scheduled sweeps, Docker-in-Docker, and extra linters unless that exact check is a current required gate with no cheaper owner.
- Cache only when install time is the measured bottleneck on a required job.
- Done: the default is the lightest pipeline that preserves the stated gate, including the valid case of no CI.

## 1. Find the cost and the contract

- Inspect Git state, owner, visibility, Actions enabled state, workflow files, required status checks, branch protection/rulesets, and the local commands those jobs run. Reuse known GitHub reads.
- Map each job to the product failure it claims to catch and to the local command it duplicates. Use existing run logs and timings first. Do not start a full remote matrix to obtain a baseline.
- Separate required merge/release gates from habitual jobs, path-unfiltered runs, and template leftovers.
- Done: required gates, overlapping work, and the bounded target are clear.

## 2. Decide what earns its cost

For each job or step, ask: **Which plausible product failure does this catch, and where else is that failure already caught?**

- **Keep** the unique required gate: build that must compile, the smallest test or check that protects supported behavior, packaging when a release artifact is required, or a security scan the repo already treats as mandatory.
- **Remove** a job that only repeats a sibling, a second OS/runtime with no supported target, a coverage/mutation/report job, a path-unfiltered run for an unrelated package, a scheduled job with no consumer, or a step that cannot fail when the claimed contract breaks.
- **Consolidate** equivalent lint, type, test, and build steps into the one existing aggregate command, or into that command's relevant parts, not both. One job that runs the existing check beats three jobs that split the same work.
- A slow or flaky job may still be the unique gate. Fix its command or retain it with the problem recorded. Do not hide it with retries, longer timeouts, `continue-on-error`, or larger runners.
- Done: each removal has evidence of duplication, missing consumers, or lack of unique signal. Retain uncertain required checks and state the evidence gap.

## 3. Apply one coherent reduction

- Preserve unrelated work and production behavior. Edit workflow YAML, required-check names, and matching docs together. After removing jobs, drop now-unused secrets, caches, service containers, and setup steps.
- Prefer deleting a workflow or job over adding `if` filters that keep dead paths. Path filters are valid when they prevent unrelated packages from paying a full install.
- Do not add a runner, coverage service, matrix, reusable workflow, or composite action to make this cleanup possible. Changing branch-protection required checks needs that scope.
- Stop when further cuts would lose a required unique gate or add machinery.
- Done: the diff removes queued work and preserves each retained gate.

## 4. Verify once and report the limits

- Validate workflow YAML and confirm job/step names that protection still requires. Run the retained local command that the remaining job will invoke. Do not wait on a new remote matrix to claim the reduction.
- On a public repo with Actions enabled, one successful run of the retained workflow is enough when the change can affect that job. Reuse a current green run when the workflow did not change. Private personal repos stay disabled unless the user asked to run Actions.
- Report removed jobs/steps, retained gates, local commands run or skipped, and any required-check rename still owed in GitHub settings.
- Done: reduction has evidence, retained gates still have an owner, and the result states what was not exercised remotely.

## Ordinary work after cleanup

Default to **no new job**. Extend the existing workflow before adding another file. A focused local or existing executable check can be enough; it does not require CI. Add CI only for a new required public gate, and keep that addition on the default shape above.
