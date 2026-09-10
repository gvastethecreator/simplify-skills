# Verification

Select final proof for changed contracts. Reuse a matching baseline; run an early comparison only when it resolves an uncertainty needed for the next edit.

## Gate Sweep

Choose among existing commands according to risk, using the repository package manager. This is a menu, not a mandatory sweep:

- install, dev startup, production build
- typecheck, lint, format check
- unit, integration, end-to-end tests
- dependency health, unused code, unused exports, duplicate code
- bundle or performance measurement when the product cares
- runtime and console output, plus accessibility and animation observation for UI

Record selected commands and results. Run an aggregate gate or its necessary components, not both. Do not install dependencies merely to check whether installation succeeds.

## Failure Classification

Every failure lands in one bucket: pre-existing, environment, missing credential, hardware, network, or reproducible product bug.

Final results separate into: fixed baseline failures, remaining baseline failures, new failures, and manual checks still owed. A new failure blocks close-out; a remaining baseline failure is reported, not hidden.

## Slice-Level Checks

Inspect the diff during coherent edits. Run a focused diagnostic only when the next edit depends on it. Batch suites, typecheck, lint, builds, and runtime proof at final integration; small behavior-preserving edits can close from the diff.

## Testing Strategy

Test retained public behavior, trust boundaries, validation, auth behavior, protocol mappings, persistence safety and atomic writes, real failure and recovery paths, supported user flows, critical UI states, and accessibility where automation is practical.

Do not test internal ceremony, wrapper existence, private call order without behavioral meaning, or removed abstractions. Preserve public-behavior coverage. Replace a test only when it covered removed machinery and the retained contract otherwise lacks proof; do not create duplicate coverage.

## Diff Review

Review the full diff against the merge base before close-out. Confirm: only intended files, no generated noise, no secrets, no unrelated user change touched, no stale wrapper, export, comment, fixture, test, or dependency left over.

## Repo Hygiene

- Manifests and lockfiles accurate and synchronized. Engine requirements and workspace references valid. Scripts concise and real.
- `.gitignore` covers dependencies, build output, temp files, generated logs, caches, coverage, editor and OS noise, local env files, secrets, runtime data. It must not ignore files needed for a reproducible build.
- Check for accidentally tracked generated, sensitive, or obsolete files. Confirm purpose before removing any tracked file.
- Editor tasks, when the repo uses them, cover dev, build, typecheck, lint, format, test, and full verify with short consistent names. No task the repo cannot execute.
- Persistent logs go to a dedicated directory with stable filenames, command context, failure output, no secrets, and an ignore rule. Prefer redirection over a logging framework.

## Honesty

State which checks ran, which did not, and why. Hardware, network, credential, and external integration behavior counts as tested only when it was actually exercised. Use `verification-quality-gate` claim mode when a claim is contested and needs a repeatable artifact.
