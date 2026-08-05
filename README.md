# Skills

Reusable Agent Skills for planning and turning approved plans into independently
mergeable implementation issues.

## Install

Use the interactive installer to choose one or both skills:

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

- `to-plan` — create a compact, architecture-first local implementation plan
  before creating issues.
- `plan-to-issues` — turn an approved plan into dense landing-unit issues whose
  PRs independently merge from the current target branch.
