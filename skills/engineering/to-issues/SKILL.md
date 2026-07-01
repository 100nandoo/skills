---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices.
disable-model-invocation: true
---

# To Issues

Break a plan into independently-grabbable issues using vertical slices (tracer bullets).

The issue tracker and triage label vocabulary should have been provided to you — run `/setup-repo-skills` if not.
If the plan produces more than one issue, or any issue is blocked by another issue, you MUST run `/to-orc` before publishing unless the user explicitly says to skip orchestration. If `docs/agents/orchestration-labels.md` exists, treat it as input to `/to-orc`; otherwise let `/to-orc` fall back to its default scheme.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes an issue reference (issue number, URL, or path) as an argument, fetch it from the issue tracker and read its full body and comments.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Issue titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change easy, then make the easy change."

### 3. Draft vertical slices

Break the plan into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

<vertical-slice-rules>

- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Any prefactoring should be done first

</vertical-slice-rules>

After drafting the slices, determine whether orchestration is required.

Orchestration is required when either of these is true:

- the plan produces more than one issue
- any slice is blocked by another slice

If orchestration is required, also draft:

- the dependency stage for each slice
- which slices share a parallel stage
- whether lanes should be packed into fewer chats or spread across more chats
- a proposed orchestration label for each slice

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories this addresses (if the source material has them)
- **Orchestration label**: if orchestration is required

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?
- If orchestration labels are included: does the stage structure and chat packing look right?

Iterate until the user approves the breakdown.

### 5. Publish the issues to the issue tracker

For each approved slice, publish a new issue to the issue tracker. Use the issue body template below. These issues are considered ready for AFK agents, so publish them with the correct triage label unless instructed otherwise.

Before publishing, validate all of the following:

- whether the approved plan produces more than one issue
- whether any approved slice is blocked by another slice
- whether the repo defines an orchestration label scheme
- whether an approved orchestration mapping already exists

If the approved plan is multi-issue or blocked, and there is no approved orchestration mapping, stop and run `/to-orc` before publishing unless the user explicitly waived orchestration.

If orchestration labels were approved, assign exactly one orchestration label per issue at creation time and preserve the explicit blockers in the issue body.

Before creating the first issue, confirm one of these invariants holds:

- every issue has exactly one orchestration label
- the user explicitly said to skip orchestration

Publish issues in dependency order (blockers first) so you can reference real issue identifiers in the "Blocked by" field.

<issue-template>
## Parent

A reference to the parent issue on the issue tracker (if the source was an existing issue, otherwise omit this section).

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it here and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- A reference to the blocking ticket (if any)

Or "None - can start immediately" if no blockers.

</issue-template>

Do NOT close or modify any parent issue.
