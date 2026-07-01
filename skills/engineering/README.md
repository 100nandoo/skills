# Engineering

Skills I use daily for code work.

## User-invoked

Reachable only when you type them (`disable-model-invocation: true`).

- **[ask-matt](./ask-matt/SKILL.md)** — Ask which skill or flow fits your situation. A router over the user-invoked skills in this repo.
- **[grill-with-docs](./grill-with-docs/SKILL.md)** — Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates `CONTEXT.md` and ADRs inline.
- **[triage](./triage/SKILL.md)** — Move issues through a state machine of triage roles.
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)** — Scan a codebase for deepening opportunities, present them as a visual HTML report, then grill through whichever one you pick.
- **[setup-repo-skills](./setup-repo-skills/SKILL.md)** — Scaffold the per-repo config (issue tracker, triage label vocabulary, domain doc layout, orchestration-label policy) that the engineering skills consume.
- **[to-issues](./to-issues/SKILL.md)** — Break any plan, spec, or PRD into independently-grabbable issues using vertical slices.
- **[to-prd](./to-prd/SKILL.md)** — Turn the current conversation into a PRD and publish it to the issue tracker.
- **[do-work](./do-work/SKILL.md)** — Execute a GitHub issue using `to-orc` stage labels, either by selecting the next unblocked issue or by honoring an explicit target such as `do-work 67`, then choosing the correct `dev` vs worktree flow and branch base.
- **[to-orc](./to-orc/SKILL.md)** — Design dependency stages, parallel lanes, and chat routing for implementation issues so multiple agent chats can execute safely.
- **[prototype](./prototype/SKILL.md)** — Build a throwaway prototype — a runnable terminal app for state/logic questions, or several toggleable UI variations.

## Model-invoked

Model- or user-reachable (rich trigger phrasing so the model can reach for them).

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)** — Disciplined diagnosis loop for hard bugs and performance regressions: reproduce → minimise → hypothesise → instrument → fix → regression-test.
- **[tdd](./tdd/SKILL.md)** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.
- **[domain-modeling](./domain-modeling/SKILL.md)** — Actively build and sharpen a project's domain model — challenge terms, stress-test with scenarios, update `CONTEXT.md` and ADRs inline.
- **[codebase-design](./codebase-design/SKILL.md)** — Shared discipline and vocabulary for designing deep modules: small interfaces, clean seams, testable through the interface.
