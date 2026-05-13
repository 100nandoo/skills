---
name: to-orc
description: Designs orchestration labels and dependency structure for implementation issues so multiple agent chats can work in parallel safely. Use when breaking a PRD or plan into staged issues, assigning chat lanes, preparing multi-chat execution, or when the user mentions orchestration.
---

# To Orc

Design the dependency graph, parallel lanes, and chat routing for a set of implementation issues.

If `docs/agents/orchestration-labels.md` exists, treat it as the repo's source of truth. Otherwise use the default scheme in [REFERENCE.md](./REFERENCE.md).

## Process

1. Gather the source material.
   Accept a PRD, plan, issue list, or parent issue. If needed, fetch referenced issues from the configured issue tracker.

2. Understand the rollout shape.
   Identify:
   - serial foundation work
   - work that can fan out in parallel
   - convergence work after the fanout
   - any slices that should stay in one chat to reduce coordination cost

3. Propose the orchestration plan.
   Present a numbered list of slices with:
   - `Title`
   - `Type`: `HITL` or `AFK`
   - `Blocked by`
   - `Orchestration label`
   - `Why this lane/chat`

4. Choose packing mode.
   Explain whether the plan uses:
   - `simple` packing: fewer chats, lower coordination overhead
   - `optimized` packing: more chats, more parallelism

5. Validate the graph.
   Check:
   - every issue has exactly one orchestration label
   - foundation stages are serial
   - only parallel stages use lane letters
   - explicit blockers in the issue body match the stage structure
   - chat ids do not imply dependencies on their own

6. Publish or hand off.
   If the user wants publishing, apply the orchestration labels when creating or updating issues. If the user only wants planning, return the approved mapping in a compact table they can reuse.

## Output format

Use a table with these columns:

| Slice | Type | Blocked by | Label | Chat | Notes |
| --- | --- | --- | --- | --- | --- |

Then list:
1. the chosen packing mode
2. the critical path
3. which slices can start in parallel
