---
name: plan-to-issues
description: Create landing-unit issues from an approved local plan. Use when splitting a plan into agent implementation tasks, drafting issues, or repairing issue sets whose PRs require an unmerged sibling, merge order, or integration branch to land.
---

# Plan to Issues

Turn an approved plan or plan suite into dense, self-contained **landing units**. A landing
unit is one issue whose Agent can start from the current target branch, produce
one green PR, and merge it without any planned sibling already existing.

The issue is both an execution brief and its compact technical design record.
It contains the selected design, relevant architecture, invariants, failure
rules and proof. A plan is provenance, never required execution context.

## Landing-unit rule

Use `main` as the target branch unless the user names another already-existing
target branch. A candidate is a landing unit only when all answers are yes:

1. Can an Agent check out the target branch and implement it using only tracked
   repository code and this issue?
2. Does its PR compile, test and make a coherent, reviewable change before any
   other planned PR merges?
3. Does merging it add a complete behavior or a complete, safe capability—not
   merely a seam that a planned sibling must finish?
4. Can its Agent own every new interface, configuration field, dependency and
   assembly change it needs without another candidate creating the same thing?

When an answer is no, take the candidate's **closure**: merge into it every
planned change that makes the answer yes. Repeat until every remaining
candidate passes. A shared base branch, stubs, a merge queue or an intended
merge order does not shrink that closure.

Two landing units are parallel-ready only when both pass individually and their
PRs have isolated ownership: each uses interfaces already on the target branch,
owns distinct new symbols/files, and has no contested configuration, registry,
dependency-lock or integration-test edit. Merge candidates when that ownership
cannot be made explicit.

Keep live credentials, account setup, approvals and production execution as
human gates. An issue may commit an opt-in harness, but its real run is a
plan-level operation, not a code dependency.

## Process

### 1. Read the evidence

Read the complete plan set (every document when the plan uses more than one file), ADRs, supplied parent issue/comments, relevant code and
tests, plus repository issue conventions. Classify evidence as authoritative,
supporting or stale. Resolve every material conflict in protocol, lifecycle,
ownership or security behavior from primary evidence before drafting.

Treat local plans, scratch files and untracked notes as drafting input only.
Copy every execution-relevant decision into its issue.

Maintain a private coverage ledger:

| Plan requirement | Landing unit | Proof |
| --- | --- | --- |
| `<requirement>` | `<issue>` | `<test or observation>` |

**Complete when:** every requirement has one landing-unit owner or a declared
human gate, and all material decisions are resolved.

### 2. Map change closure

Inspect current seams, tests, configuration, registries, dependencies and
validation commands. For each planned change, record what current tracked code
it consumes and what new artifact it introduces.

For every tentative issue, recursively include its planned prerequisites. Also
include any planned work that shares a new type, config shape, constructor,
registry, dependency lock, lifecycle test or integration point. Apply the
landing-unit rule before writing prose.

**Complete when:** each remaining unit can start from the target branch and
leave it green after one PR; no unit relies on an unmerged plan artifact.

### 3. Draft the landing unit

Write a closed-book issue: a fresh Agent receives only the issue, target
repository and repository-native instructions. Give it enough design context to
implement without source-plan archaeology, while leaving only local coding
choices open.

Use a diagram only when it proves a local ownership, data flow, state change or
failure branch more clearly than prose.

```markdown
# <Outcome-oriented title>

Status: planned

## Outcome

<Complete behavior or capability that this one PR makes mergeable.>

## Existing boundary

<Current tracked behavior, relevant seams and why this unit owns the complete
change.>

## Chosen approach and invariants

<Selected design, rationale, stable identity/ownership/lifecycle/security
rules, and a diagram or worked example when it makes a rule auditable.>

## Scope

- <Every code, config, dependency and assembly change required for the outcome>
- <Explicitly retained behavior>

## Implementation plan

1. `<verified path or symbol>` — <exact change and ordered behavior>
2. `<verified path or symbol>` — <integration and failure behavior>
3. `<test name>` — <fixture, action and expected observation>

## Acceptance criteria

- [ ] <Observable outcome>
- [ ] <Failure, retry, compatibility or security rule>
- [ ] <Target branch remains green without another planned PR>

## Validation

- `<repository-native command>`
- <Scenario and expected evidence>

## Human gates

- <Manual resource, approval or live run and its evidence>, or `None`.

## Source context

- <Optional public primary URL or tracked repository path; never required to
  execute the issue>

## Comments

On completion append commit/PR, validation results, sanitized diagnostics and
any correction to the stated contract.
```

When current evidence differs from the plan, state the prior rule, corrected
rule, proof and impact in `## Chosen approach and invariants`. Keep every
execution-relevant decision inside the issue.

**Complete when:** the issue passes the landing-unit rule and contains all
behavior, rationale and proof needed for its PR.

### 4. Check publication accessibility

Inspect every path and link before publication. Keep public primary URLs and
repository paths verified tracked at the target revision. Use
`git ls-files --error-unmatch <path>` for the current checkout and
`git ls-tree -r <target-ref> -- <path>` when a target revision is known.

State files created by the issue as create targets in Scope or the
Implementation plan; they are not source context. Copy decisions out of local
paths rather than citing them.

**Complete when:** a remote reviewer can follow every citation, and closing all
citations still leaves the issue executable.

### 5. Audit and publish

Audit every draft:

- **Landing:** it passes all four landing-unit questions from the current target
  branch.
- **Parallel readiness:** any proposed companion has isolated ownership and
  independent green validation.
- **Closed-book:** no plan, scratch file, sibling Issue or hidden branch is
  required reading.
- **Technical density:** architecture, trade-offs, invariants and proof are
  legible; diagrams earn their space.
- **Coverage:** the ledger assigns every plan requirement once.
- **Accessibility:** no local-only source is cited or required.

Present each issue with its outcome, why it is a landing unit and its human
gates. If closure produces one unit, present one issue rather than artificial
parallelism. Publish one native issue per approved unit, with `Status: planned`
for manual dispatch. Native issue bodies carry their own execution brief; they
contain no cross-issue state, tracker relation or ordering section.

**Complete when:** every published issue is independently mergeable from the
target branch, all citations render, and the set is reported as planned.
