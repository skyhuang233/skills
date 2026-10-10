# Skills

一组可复用的 Agent Skills：从概要设计逐步追问到详细设计、共写一份活计划，再依据这份计划
（或一段口述描述）实现并提交工作。

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

- `software-engineering-principle` —— 21 条软件工程原则的参考，设计、实施与审查时引用取词。
- `grill-with-docs` —— 从概要到详细地讨论设计，并同步维护一份活计划。
- `grilling` —— 一次只追问一个决策，并为每个决策推荐最小充分机制。
- `to-plan` —— 维护并独立审核一份从概要展开到详细的活计划。
- `implement` —— 实现一项已设计好的工作并提交到当前分支，内部走 `tdd` 与 `code-review`。
- `tdd` —— 红绿循环的参考：好测试、seam、反模式与循环规则。
- `code-review` —— 沿 Standards 与 Spec 两条轴并行审查改动。

## 推荐使用顺序

分为两块，块之间由**用户手动发起**：

```text
块 1（设计）：grilling + grill-with-docs → to-plan → .scratch/<feature>/plan.md

块 2（实施）：implement（内部走 tdd → code-review）
```
