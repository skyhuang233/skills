---
name: to-plan
description: Turn settled design work into a compact, architecture-first local implementation plan before issue creation.
disable-model-invocation: true
---

# To Plan

Create an **architecture-first plan** from a settled `/grill-with-docs`
discussion. It is the reviewable bridge between design and
`/plan-to-issues`: concrete enough to implement without rediscovery, compact
enough for a human to understand the system in one pass.

Plan depth comes from named contracts, ownership, paths, lifecycle edges, and
proof—not from the number of files. Keep the whole plan in the fewest documents
that preserve a clear architecture and a coherent review.

## 1. Establish the evidence and size

Read the settled conversation, relevant ADRs/glossary, affected source and
tests, repository conventions, and authoritative external documentation. Check
the current target branch rather than trusting old notes.

For each material seam, record current evidence, the selected rule, and the
required proof. Keep unresolved access, credentials, DNS, purchases, approvals,
and live certification as named human gates.

Then choose the smallest shape from
[plan shapes](references/plan-shapes.md):

- **One document** for one repository and one coherent behavior, where the
  architecture, implementation, and validation remain easy to review together.
- **Two documents** for the usual single-repository integration: one concise
  architecture/decision map and one implementation/verification plan. This is
  the default when code detail would otherwise bury the system model.
- **Four or five documents** only when at least two strong split signals are
  present: multiple repositories or independently deployed systems; a staged
  compatibility/data/protocol migration; or an independently owned
  control-plane, runtime, or operator workflow with its own design review.

A document earns its existence only when it carries an independent architecture
or review question. Keep adjacent material as sections of the same document;
never create a document merely because it names a technical surface.

**Complete when:** every requested behavior has a current evidence source, a
selected rule, or a named human gate, and the chosen file count is justified by
the shape rules.

## 2. Draw the system before detailing code

Put a whole-task map immediately after the outcome and non-goals—before long
tables or code-level detail. For work that crosses a component, runtime, or
external-service boundary, use a Mermaid component or sequence diagram that
shows the changed boundary, reused boundary, and labeled flows. For a contained
code change, use a small before/after module or data-flow map instead.

Make the map answer: what initiates the behavior, which layer owns each
decision, what crosses a trust or API boundary, and where observable success
or cleanup occurs. Add a focused sequence diagram only when lifecycle order is
itself a material decision.

**Complete when:** a reviewer can state the target architecture and the
change's insertion point from the first screen of the primary plan document.

## 3. Write an executable plan

Follow the selected shape closely enough to keep plans recognizable; tailor
only the sections that carry real decisions. The exact templates and split
examples are in [plan shapes](references/plan-shapes.md).

For every implementation change, state the verified path and symbol, current
and target responsibility, input/output or resource contract, error/retry and
compatibility rule, and proof. Use tables for identity/resource/configuration
mappings. Include small interface, payload, YAML, or command examples only
when they freeze a cross-boundary contract.

Cover first use, repeated execution, upgrade or migration, partial failure,
cleanup, rollback/repair, and stale/concurrent state whenever they matter.
State credentials and allow-lists at the boundary that owns them. Use concrete
non-secret examples; replace sensitive values with placeholders.

The plan is local provenance, not an Issue draft. Do not pre-split work into
PRs, Issue numbers, `Blocked by` relations, execution order, or agent status.

**Complete when:** each planned behavior has an exact owner, change location,
edge rule, and observable acceptance proof.

## 4. Audit and approve

Read the whole plan set as a reviewer. Replace vague phrases such as “add
support”, “update config”, and “add tests” with the responsible boundary,
path/symbol or external contract, lifecycle rule, and evidence.

Verify that names agree across diagrams, mappings, code plans, configuration,
and tests; that every external interaction names its caller, identity, effect,
errors, idempotency, and credential boundary; and that every validation says
whether it is automated, manual, or a human-gated live check.

Keep the primary document `Status: draft` until the user approves the complete
plan. Then mark it `approved` and hand the one document or complete plan set
to `/plan-to-issues`, which must copy the decisions into independently
mergeable Issues.

**Complete when:** the approved plan is architecture-first, has no unowned
material open decision, and lets a senior engineer implement its scope without
a new discovery pass.
