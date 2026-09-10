---
name: simplify-repo
description: "Repo simplification refactor: delete over-engineering, land the smaller architecture, interface pass, verify against baseline, honest report."
disable-model-invocation: true
---

# Simplify Repo

Subtraction, implemented. Leave fewer concepts, states, branches, files, exports, dependencies, and maintenance obligations than you found, with real behavior, safety, and accessibility intact.

This is an implementation skill. Ending at an audit, proposal, or recommendation list is failure unless the user asked for a report or a stop condition below fired.

Optimize for the product that exists today. Not for the platform someone imagined.

## When To Use

- "simplify this repo", "remove over-engineering", "repo-wide refactor", "make this maintainable again"
- flows nobody can trace: wrappers, registries, queues, retry state machines, lifecycle flags with no measured need
- parallel old and new implementations living side by side
- a repo where docs, tests, and tasks describe an architecture that no longer exists
- a rewrite request where the honest answer is deletion, not another layer

Prefer specialized skill:

- `ponytail`: one change, one simplicity decision.
- `simplify-tests`: suite reduction. No architecture change.
- `simplify-ci`: pipeline reduction. No architecture change. Failing checks stay `fix-ci`.
- `technical-debt-audit`: inventory, classification, priorities. No implementation.
- `project-maintenance`: root clutter, accidental git-history junk, stale docs to `.scratch`, logs, scripts/deps, dead files, and in-architecture low-value code. No architecture change.
- `improve-codebase-architecture`: approval-gated batch of N improvements with value reports.
- `project-tune-up`: existing-repo puesta a punto. `rust-http-api`, `rust-cli`, `bevy-app`, `tauri-desktop` for those contracts.
- `improve-ui`: interface-only work.
- `diagnose`: one hard bug.
- `scope-control`: user wants the minimal patch and nothing else.

## Compose

- `ponytail` when a keep-or-delete decision needs its simplification ladder.
- `codebase-design` for module, seam, depth, interface vocabulary.
- `explore-codebase` or `codebase-inspection` to map the repo in step 2.
- `planning-with-files` for the ledger when the pass is long, multi-session, or AFK.
- Reuse the active implementation workflow. Load `code-review` or `verification-quality-gate` for a distinct unresolved review or evidence need; do not start a new workflow per slice.
- `research` only for official migration guides, deprecations, platform limits, security detail.
- `improve-ui`, `better-accessibility`, `review-animations` for the interface pass.
- `to-tickets` / `to-spec` for work that genuinely cannot land in this pass.
- `html-lab` when the user wants the final report as a shareable HTML artifact.
- `hablar-claro` for operator chat.

## Hard Rules

- Repo instructions win: `AGENTS.md`, `CONTRIBUTING.md`, `README`, package-level rules, lint/format/test/commit conventions, ADRs. If a convention is obsolete, change it in the same pass and say why.
- Start with `git status -sb` and the merge base. Preserve unrelated, user, and other-agent changes.
- No `reset --hard`, `clean`, `restore`, force-push, rebase, or history rewrite without explicit user consent.
- No repo-wide reformat, rename sweep, or unrelated cleanup. It hides the real diff.
- No deletion without the evidence bar below.
- No parallel architectures at the end of a slice. Replacement lands, old machinery goes.
- No compatibility wrapper unless backward compatibility is a current, stated requirement.
- No new dependency to replace a few straightforward lines. No custom framework to avoid one dependency call.
- Never fake verification, commits, pushes, or pull requests. Never claim hardware, network, credential, or integration behavior was exercised when it was not.
- Secrets stay out of logs, diffs, reports, and commits.

## Safety Floor

Do not simplify these away. They are the behavior, not the ceremony.

- Validation at trust boundaries, and parsing of untrusted input or protocol messages
- Authentication, authorization, credential redaction, secret handling
- Atomic writes where interruption corrupts data, and restrictive permissions on sensitive files
- Data-loss protection, and migrations still required by supported versions
- Accessibility behavior and explicitly supported user behavior
- Errors and recovery paths that let a caller or user decide something

Internal layers do not need to re-defend values already validated at a clear boundary. That duplication is fair game.

## Process

1. Frame and protect.
   - Confirm scope (whole repo or named surface), mode (implement, default; audit-only on request), and any blocking repo convention.
   - `git status -sb`, current branch, merge base, pre-existing modified and untracked files.
   - Open or create the ledger in the repo's existing planning, spec, or issue system. Do not build a parallel tracker.
   - Done: scope, mode, protected pre-existing changes, and ledger location written down.

2. Map product and ownership.
   - Product purpose, main user flows, entry points, runtime targets, packages, public APIs, state and persistence owners, UI entry points, design tokens, tests, build, CI, scripts, docs, ADRs, open issues touching the area.
   - Which files are authoritative and which only restate them.
   - Done: you can name every real flow and who owns its state.

3. Baseline.
   - Reuse current baseline results and inspect the relevant contracts. Run a baseline probe only when before/after behavior cannot otherwise be established or its answer determines the next edit. Do not run every available gate before editing.
   - Classify every failure: pre-existing, environment, missing credential, hardware, network, or reproducible product bug.
   - Done: a comparable baseline exists. Later results are diffed against it, and pre-existing failures are never blamed on this pass.

4. Trace flows, then audit for deletion.
   - Per flow: entry point, domain operation, state transitions, validation boundary, persistence, network, protocol mapping, error path, recovery path, user-visible feedback.
   - Find authoritative state versus mirrored state, uncalled code, unconsumed exports, abstractions with one implementation, checks that repeat prior validation, machinery serving a hypothetical.
   - Rank candidates by deletion value against behavioral risk. Read [references/deletion-audit.md](references/deletion-audit.md) for the smell catalog, per-finding record, and ranking.
   - Done: a ranked candidate ledger where each entry names its evidence and its smallest replacement.

5. Choose the target shape.
   - Smallest architecture that serves today's domain operations: domain verbs, one implementation per real backend, one persistence owner, thin entry points, honest concurrency, platform cancellation, a small decision-oriented error model.
   - Read [references/target-shape.md](references/target-shape.md) before designing replacements.
   - Name breaking changes now, before implementing them.
   - Done: target described in terms of current product behavior, with breaking changes listed.

6. Land deletion slices.
   - One slice = one coherent behavior: replacement in, machinery out, tests moved to the retained behavior, docs and comments for the deleted design removed.
   - Fix at the ownership point, not per caller. Finish half-built behavior when callers, tests, UI, docs, or siblings make the intent inferable.
   - During the batch, inspect diffs and record meaningful decisions. Add only missing behavior coverage; hold test runs and full validation for final integration.
   - Keep changes coherent and remove obsolete paths within the requested cutover. Use an early diagnostic only if its answer blocks the next edit.
   - Done: the source batch has no dead remainder and is ready for final proof. Commits require authorization.

7. Interface pass.
   - Only when the repo ships a UI. Flow completeness, token and theme use, layout and overflow, accessibility, feedback states, animation lifecycles, gallery coverage.
   - Read [references/interface-pass.md](references/interface-pass.md), and route the deep work to `improve-ui`, `better-accessibility`, `review-animations`.
   - Done: every reviewed screen and state either fixed or recorded with a reason.

8. Verify against baseline, then re-review for maintainability.
   - Select the applicable final gate or focused checks for changed risks. Avoid commands already covered by an aggregate gate. Inspect the final scoped diff and follow [references/verification.md](references/verification.md).
   - Second critical pass on your own result: new abstraction that can go, split ownership you introduced, one-caller wrapper, one-implementation interface, duplicated state, impossible branch, config that never varies, leaked lifecycle, ceremony test, surviving compat layer or placeholder.
   - Resolve findings now when it is safe. Record the rest.
   - Done: results separated into fixed baseline failures, remaining baseline failures, new failures, and manual checks still owed.

9. Close out.
   - Update docs, specs, tickets, and ADRs to the implemented system. Archive or mark superseded plans without rewriting their history.
   - Commit, push, or update a PR only within explicit authorization. Credentials establish capability, not consent. Keep authorized commits independently reviewable.
   - Final report per [references/report.md](references/report.md), including machinery you deliberately did not add and the condition that would justify it later.
   - Done: docs match code, tracker matches reality, report states truthfully what ran and what did not.

## Deletion Evidence Bar

Delete when at least one holds, and the evidence is in the transcript:

- No caller: symbol and string search across source, tests, config, scripts, docs, and generated call sites, plus dynamic-access patterns for the language.
- Export with no consumer inside or outside the repo, and not a supported public API.
- Behavior fully covered by a retained path you traced end to end.
- Generated, cached, or artifact file that is ignored or should be.
- Doc, test, or fixture that only preserves machinery this pass removed.

Stop and ask when the target may be a supported public API, a release artifact, security evidence, user notes, migration history, or when ownership cannot be established.

Age, naming, and vibes are not evidence.

## Stop Conditions

Continue through routine phases without asking. Pause only for: destructive or irreversible action, two equally plausible product behaviors, a breaking change to a supported public contract with unclear intent, missing credentials or hardware or services, or a repo instruction that requires approval.

When blocked: finish everything independent of the blocker, record the blocker and the missing evidence, name the smallest decision needed, file it in the tracker, and never fabricate the result.

## Anti-Goals

- Line-count targets. Deletion of concepts is the metric, not lines.
- Moving files to look organized.
- Replacing machinery with fashionable machinery.
- Deferring to a ticket what could land in this pass.
- Weakening tests to turn the suite green.

## Output Shape

Terse final report, expanded per [references/report.md](references/report.md): status; what changed and why; baseline versus final gates; old and new call paths; what was removed (concepts, states, branches, files, exports, dependencies, config); public API and breaking changes; interface fixes; docs and tracker updates; branch, commits, PR status; remaining risks and manual checks; machinery deliberately omitted with its reinstatement trigger.
