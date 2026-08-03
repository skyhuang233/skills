# Plan Shapes

Read this after sizing the task in `SKILL.md`. Select one shape; do not combine
their file counts merely because the work mentions several technologies.

## Architecture-first rules

Every plan begins with outcome and non-goals, then a whole-task map before
implementation detail.

- Use `flowchart` for components, ownership, and cross-boundary data/resource
  flow. Label arrows with the actual operation or payload.
- Use `sequenceDiagram` only when the order of calls, state changes, retry, or
  cleanup is a decision to review.
- Keep unrelated existing systems out of the diagram. Show reused boundaries
  only when they explain what remains unchanged.
- A diagram is not decoration: follow it with the invariants and mappings it
  makes auditable.

## One-document plan

Use for a contained feature, fix, or refactor in one repository with one
coherent behavior. Write one `.scratch/<feature-slug>/<name>.plan.md` (or the
repository's documented equivalent):

~~~~markdown
---
name: <outcome>
status: draft
---

# <Outcome-oriented plan>

## Outcome and non-goals
## Whole-task map
```mermaid
flowchart LR
  <changed and reused boundaries with labeled arrows>
```
## Decisions and invariants
## Implementation and lifecycle
| Path and symbol | Change and contract | Edge rules | Proof |
| --- | --- | --- | --- |
## Validation and human gates
## Source context
~~~~

Keep all code, configuration, and test detail under `Implementation and
lifecycle`; use subheadings rather than a new file.

## Two-document plan

Use for a normal single-repository integration. The first document gives a
human an immediate model; the second carries the detailed execution record.
This is the normal shape for a provider integration when one codebase owns the
adapter, configuration, lifecycle, and tests.

```text
.scratch/<feature-slug>/
  00-architecture-and-decisions.plan.md
  01-implementation-and-verification.plan.md
```

`00-architecture-and-decisions.plan.md` contains:

1. Outcome and non-goals.
2. Whole-system architecture diagram.
3. Component ownership and unchanged boundaries.
4. Named invariants and resource/identity/configuration mappings.
5. Decision deltas, compatibility rules, human gates, and a link to document
   01.

`01-implementation-and-verification.plan.md` contains:

1. The current boundary and target behavior, with a sequence diagram only if
   lifecycle order matters.
2. A code-change table naming every path and symbol.
3. External API/client contracts, data shapes, error/retry/idempotency, and
   cleanup/rollback rules where relevant.
4. Configuration, workflow, and documentation changes as sections—not files—
   unless they independently meet a strong split signal.
5. Automated scenarios, live/manual certification, expected evidence, and
   deliberately unchanged behavior.

## Four- or five-document plan

Use only when at least two strong split signals made a two-document plan hard
to review: multiple repositories or deployable systems; a staged breaking
migration; or a separately owned control-plane, runtime, or operator workflow
that needs its own architectural review.

Keep the suite within five documents. Start with an overview, then group by
independent review question rather than technical labels:

```text
00-architecture-and-contracts.plan.md
01-<repo-or-control-plane>.plan.md
02-<runtime-or-migration>.plan.md
03-<other-independent-surface>.plan.md
04-verification-and-rollout.plan.md
```

The overview owns the single whole-system diagram, cross-repository contracts,
naming rules, invariants, and navigation. Each remaining file must introduce a
separate boundary, contract, or migration decision and include its own
path/symbol-level changes and proof. Merge empty or merely descriptive files
back into their nearest owner.

## Density gate

Regardless of shape, an implementation detail is complete only when a reviewer
can locate its code or contract, understand the before/after responsibility,
see the lifecycle and security edge rule, and identify the validation evidence.
Put prose that only repeats the diagram, table, or source code in the nearest
artifact rather than duplicating it elsewhere.
