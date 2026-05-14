---
name: do-work
description: Executes a GitHub issue by following the repo's `to-orc` orchestration labels, branch rules, and commit naming convention. Use when a user wants an agent to pick the next unblocked issue, choose `dev` versus a worktree, and branch from the correct dependency stage.
---

# Do Work

Execute a GitHub issue that already has a `to-orc` orchestration label.

If `docs/agents/orchestration-labels.md` exists, treat it as the repo's source of truth. Otherwise use the default `to-orc` format:

- foundation: `f<N>-<C>`
- parallel: `p<N><L>-<C>`

## Process

1. Gather context.
   Read the issue body, labels, blockers, and any repo docs that define branching or orchestration rules.

2. Parse the orchestration label.
   Extract:
   - stage number `N`
   - lane letter for parallel stages
   - chat id `C`

3. Choose the next valid issue.
   Start only unblocked work.
   Prefer the lowest dependency stage number.
   Within the same stage, prefer the lowest issue number unless the user specifies a different lane or issue.
   Do not start a higher required stage while a lower one is still open or unmerged.

4. Handle convergence before selecting new foundation work.
   If the next issue is a foundation stage whose previous dependency stage was parallel, first confirm whether the completed parallel branches should now merge into `dev`.
   Do not start the new foundation issue yet.
   Ask the user for explicit approval to merge.
   If the user approves, merge the required parallel branches into `dev` one at a time in ascending issue number unless the user specifies a different order.
   If any merge conflicts, stop and ask before resolving it.
   Create the merge commit(s), close the merged parallel issues, and only then continue to the new foundation issue.

5. Confirm the chat before implementing.
   After selecting the issue, tell the user:
   - selected issue
   - parsed orchestration label
   - why it is unblocked
   - chosen base branch or worktree
   - expected commit message format

   Ask `Proceed in this chat?` before coding so the user can verify this is the correct chat for that issue.

6. Choose the workspace strategy.
   - `f<N>-<C>`: work on the local `dev` branch
   - `p<N><L>-<C>`: create or use a separate worktree and create branch `issue/<issue-number>`

   If `issue/<issue-number>` already exists locally or remotely, reuse it only if it clearly belongs to that issue. Otherwise stop and ask before creating or reusing a branch or worktree.

7. Choose the base commit correctly.
   - foundation stages build on the latest `dev` commit that already contains the previous required stage
   - parallel stages branch from the latest `dev` commit that already contains the immediately previous dependency stage
   - convergence foundation stages after a parallel fanout start only after every required parallel lane for the prior stage is merged back into `dev`

8. Implement the issue.
   Keep the issue scope narrow. Respect the blocker graph from the issue body and orchestration label.

9. Verify, commit, push, and close the issue correctly.
   Run the relevant verification for the change.
   Commit with the required format:
   Use:
   - `#120 Refactor preview class`
   - `#121 Cover all cases`

   Format rule: `#<issue-number> <imperative or descriptive summary>`
   Push the branch to the remote.
   If this is foundation work being delivered on `dev`, close the GitHub issue with a comment that includes the branch name and commit SHA or merge SHA.
   If this is parallel work on `issue/<issue-number>`, do not close the issue just because the branch was pushed. Close it when that branch is merged into `dev`, and include a closing comment with the branch name and merge SHA, or follow the repo's explicit policy if it differs.

## Decision rules

- Do not treat the chat suffix as a dependency signal. The stage number defines ordering.
- Parallel siblings in the same stage may start from the same parent commit and proceed independently.
- A later foundation stage must not start until all required parallel siblings from the prior stage are merged into `dev`.
- When a completed parallel stage unlocks a new foundation stage, ask before merging those parallel branches into `dev`.
- Closing rules depend on integration state, not just push state. A pushed parallel branch is not automatically complete.
- If the correct base is unclear, inspect merged history and blocker issues before coding.

## Example

For labels:

- `f1-1`
- `f2-1`
- `p3a-2`
- `p3b-3`
- `f4-1`

Execution order:

1. `f1-1` on `dev`
2. `f2-1` on `dev`
3. `p3a-2` in a worktree on branch `issue/<issue-number>` based on the `dev` commit after `f2-1`
4. `p3b-3` in a separate worktree on branch `issue/<issue-number>` based on the same `f2-1` result
5. when `p3a`, `p3b`, and any other required `p3*` lanes are done, ask whether to merge them into `dev`
6. after approval, merge those branches into `dev` and close the merged `p3*` issues
7. then pick up `f4-1` on `dev`

## Output

Before implementation, state:

- selected issue
- parsed orchestration label
- why it is unblocked
- chosen base branch or worktree
- expected commit message format

If the issue is a convergence foundation stage after parallel work, ask first whether to merge the completed parallel branches into `dev`.

Then ask for confirmation that this is the correct chat for the issue.
