# Skills

Reusable Agent Skills for co-designing living plans from overview to detail and
turning approved plans into independently mergeable implementation issues.

## Install

Use the interactive installer to choose one or more skills:

```bash
npx skills@latest add skyhuang233/skills
```

List the skills without installing them:

```bash
npx skills@latest add skyhuang233/skills --list
```

Install every skill globally for Codex without prompts:

```bash
npx skills@latest add skyhuang233/skills --skill '*' --agent codex --global --yes
```

Omit `--global` to install into the current project's agent-skills directory.

## Included skills

- `grill-with-docs` — discuss a design from overview to detail while keeping its
  living plan, glossary, and necessary ADRs current.
- `to-plan` — maintain and independently review an overview-to-detail living
  design plan.
- `plan-to-issues` — turn an approved plan into dense landing-unit issues whose
  PRs independently merge from the current target branch.

Recommended sequence:

```text
grill-with-docs → to-plan → user approval → plan-to-issues
```
