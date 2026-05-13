# Orchestration Labels

This document defines a stable GitHub labeling scheme for planning dependency order, parallel execution, and Codex chat routing.

Use this scheme when breaking a larger implementation into issues that may be worked by multiple agent chats.

## Goals

- Keep the dependency graph visible from the label itself
- Keep chat routing machine-readable for a future agent orchestrator
- Keep planning labels stable from the start instead of changing them as work progresses
- Allow either simple packing or maximum parallelism without changing the stage structure

## Label format

Each issue gets exactly one orchestration label.

Foundation stages use:

```text
f<N>-<C>
```

Parallel stages use:

```text
p<N><L>-<C>
```

Where:

- `N` is the dependency stage number
- `L` is the parallel lane letter within that stage
- `C` is the chat identifier

## Meaning

Examples:

- `f1-1`: foundation stage 1, assigned to chat 1
- `f2-1`: foundation stage 2, also assigned to chat 1
- `p3a-2`: parallel stage 3, lane `a`, assigned to chat 2
- `p3b-3`: parallel stage 3, lane `b`, assigned to chat 3
- `f4-4`: foundation stage 4, assigned to chat 4

The stage portion encodes dependency order.

- `f1` must finish before `f2`
- `f2` must finish before stage `3` work starts
- `p3a` and `p3b` are sibling lanes in the same stage and may run in parallel once their blockers are cleared
- `f4` represents convergence after the stage-3 fanout

The suffix after `-` encodes chat routing.

- Issues with the same chat id belong in the same Codex conversation thread
- Issues with different chat ids belong in different chats

## Planning rules

- Orchestration labels are structural planning metadata and should be assigned when the issue is created
- Orchestration labels should not change as issues become unblocked or completed
- If the plan itself changes, relabel only when the dependency structure was actually wrong
- Keep exact blockers in the issue body even when the stage label already implies ordering

## Chat packing

This scheme supports both a simple mode and an optimized mode.

### Simple mode

Use the same chat id for multiple related issues when reducing coordination overhead matters more than maximum concurrency.

Example:

- `f1-1`
- `f2-1`
- `p3a-2`
- `p3b-2`
- `f4-3`

### Optimized mode

Use different chat ids for independent lanes when maximizing parallel execution matters more than minimizing thread count.

Example:

- `f1-1`
- `f2-1`
- `p3a-2`
- `p3b-3`
- `f4-4`

## Status labels

This scheme is for planning and routing, not live status.

Use separate status labels when needed, for example:

- `ready-for-agent`
- `blocked`

Those labels may change over time. The orchestration label should usually remain stable.
