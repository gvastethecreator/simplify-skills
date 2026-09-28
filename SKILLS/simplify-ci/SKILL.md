---
name: simplify-ci
description: "CI reduction: prune overlapping jobs, matrices, extra runtimes, and heavy GitHub Actions so every project keeps the lightest required pipeline, or audit-only with grouped findings and an ordered plan. Use for bloated workflows, slow checks, duplicate lint/test/build jobs, CI audits, or when adding CI."
---

# Simplify CI

Keep the cheapest pipeline that still proves required gates. Job count, matrices, and extra runtimes are not quality. Work within the requested repo.

## Modes

- **Clean up** (default for reduce, prune, or clean requests): steps 1–4.
- **Audit-only**: the user asks for a review, audit, or plan, or says don't edit yet. Run steps 1–2 read-only and deliver the [audit report](#audit-report). No edits, no commits, no GitHub settings changes. Say so at the start. A later "go" runs steps 3–4 on the approved plan steps and reuses audit evidence that is still valid.

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
- Check consumers before calling a job or workflow unused: `needs:` chains, `workflow_run` triggers, `workflow_call` callers in this and other repos, required-check names in branch protection and rulesets, artifacts downloaded later, deploy or release docs, and README badges.
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

## Audit Report

Deliver in chat when short. For a long audit, write it to the path the user names or the repo's docs or plans folder, and summarize in chat. Offer `html-report` for a shareable file.

Per finding:

- **ID and group:** `D` safe removal, `C` consolidate, `G` needs settings or explicit authority.
- **Exact paths:** workflow file, job id, step name, line range, trigger, the local command it runs, and its required-check name if any.
- **Why:** duplicate of which retained job, no consumer, unsupported OS or runtime, cannot fail when the claimed contract breaks, template leftover.
- **Change:** what to remove or merge, and which retained job still catches the failure.
- **Risk:** the failure that could go uncaught, merges blocked by a missing required check, release or deploy breakage, secrets or caches other workflows still use.
- **Confidence:** high when you read the job YAML and recent runs, the retained job runs the same command, and the job is not required or its rename is in the plan; medium when one gap remains (protection not readable, no run history, possible external `workflow_call` callers), named with its closing check; low is a lead only. Only high goes to `D`.
- **Estimate:** jobs and steps removed, YAML lines, and runner minutes per run and per month from existing run history, counting the matrix multiplier. No history means "unknown".

Groups:

- **D:** duplicate jobs or steps, scheduled runs with no consumer, unsupported OS or runtime entries, template leftovers, now-unused caches, services, and setup steps. Never a required check.
- **C:** merge into the existing aggregate command, add path filters that skip unrelated installs, drop matrix dimensions with no supported target.
- **G:** required-check renames or removals in branch protection or rulesets, release, deploy, or publish workflows, mandatory security scans, secret deletion, Actions enable state, larger runners.

Report sections: scope and coverage (repo, owner, visibility, Actions state, workflows read, run history used, what was not readable); summary table (ID, group, finding, paths, confidence, estimate, risk); findings by group; checked and kept, with the unique gate each job owns; leads; totals per group; decisions owed.

Ordered plan: `D` first, then `C`, then `G`. One step is one reviewable change. Per step: files and jobs touched, the retained gate for each removed job, checks after the step (YAML validation with the repo's existing linter if any, the retained local command, one remote run only on a public repo when the step affects that job), required-check settings to change and who changes them, rollback, and any decision needed first.

## Ordinary work after cleanup

Default to **no new job**. Extend the existing workflow before adding another file. A focused local or existing executable check can be enough; it does not require CI. Add CI only for a new required public gate, and keep that addition on the default shape above.
