# Skills

一组可复用的 Agent Skills：从概要设计到详细设计共写一份活计划，并把已批准的 plan
拆成可独立合并的实现 issue。

## 安装

用交互式安装器选择一个或多个 skill：

```bash
npx skills@latest add skyhuang233/skills
```

只列出 skill，不安装：

```bash
npx skills@latest add skyhuang233/skills --list
```

为 Codex 全局安装全部 skill，跳过询问：

```bash
npx skills@latest add skyhuang233/skills --skill '*' --agent codex --global --yes
```

去掉 `--global` 则装进当前项目的 agent-skills 目录。

## 包含的 skill

- `grill-with-docs` —— 从概要到详细地讨论设计，同时让它的活计划、术语表和必要 ADR 保持最新。
- `grilling` —— 一次只追问一个决策，并为每个决策推荐最小充分机制。
- `domain-modeling` —— 维护 `CONTEXT.md` 中的 ubiquitous language，并记录达到 ADR 门槛的决策。
- `to-plan` —— 维护并独立审核一份从概要展开到详细的活计划。
- `plan-to-issues` —— 把已批准的 plan 拆成密集的 landing unit issue，其 PR 各自从当前目标分支独立合并。

推荐使用顺序：

```text
grilling + domain-modeling → grill-with-docs → to-plan → 用户批准 → plan-to-issues
```
