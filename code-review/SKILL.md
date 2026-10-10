---
name: code-review
description: 沿两条轴审查自某个定点（commit、分支、tag 或 merge-base）以来的改动——Standards（代码是否遵循本仓库文档化的编码标准与 software-engineering-principle 中的原则？）与 Spec（代码是否忠实实现了预定要求？）。两条轴各由一个并行 sub-agent 执行，报告并列出示。当用户想审查一个分支、一个 PR、进行中的改动，或要求「审查自 X 以来」时使用。
---

对 `HEAD` 与用户指定定点之间的 diff 做两轴审查：

- **Standards** ——代码是否符合本仓库文档化的编码标准，以及 `software-engineering-principle` 中的原则？
- **Spec** ——代码是否忠实实现了预定要求？

两条轴各由一个 **并行 sub-agent** 执行，这样它们不会污染彼此的上下文，之后由本 skill 汇总
两份报告。

## 流程

### 1. 钉住定点

用户说的是什么，定点就是什么——commit SHA、分支名、tag、`main`、`HEAD~5` 等。如果用户没有
指定，就向用户询问。

只捕获一次 diff 命令：`git diff <fixed-point>...HEAD`（三点式，即以 merge-base 为比较基准）。
同时用 `git log <fixed-point>..HEAD --oneline` 记下提交列表。

继续之前，确认定点能够 resolve（`git rev-parse <fixed-point>`）且 diff 非空。错误的 ref 或
空的 diff 应该在这里就失败——而不是进到两个并行 sub-agent 里才失败。

### 2. 确定 spec 来源

**唯一来源：**`.scratch/<feature>/` 下匹配当前分支或 feature 的 plan。

找不到就是「no spec available」，Spec 轴跳过，并在最终报告中注明。

### 3. 确定 standards 来源

仓库中任何记录「代码应该怎么写」的文件，例如 `CODING_STANDARDS.md` 或 `CONTRIBUTING.md`。

在仓库文档化的标准之外，Standards 轴始终携带 `software-engineering-principle`：把该 skill 全文
作为常备清单与仓库标准并列。两条规则约束它：

- **仓库标准优先。** 仓库文档化的标准永远胜出；它与原则 skill 冲突之处，以仓库标准为准。
- **永远是判断，不是硬性违规。** 每条原则都是标注过的启发式（「这里可能违反了第 1 条」），
  从不构成硬性违规——并且和这里的其他标准一样，凡是工具已经强制的东西都跳过。

### 4. 并行启动两个 sub-agent

**Standards sub-agent 的 prompt**——包含：

- diff 命令与提交列表；
- 第 3 步找到的 standards 来源文件清单，**以及 `software-engineering-principle` 全文**（sub-agent
  没有其他途径拿到它）；
- 简报：「按 file/hunk 报告：（a）diff 中违反文档化标准之处——引用该标准（文件 + 规则）；
  （b）你发现的每一条原则违规——指明原则编号并引用相关 hunk。区分硬性违规与判断性意见：
  违反文档化标准可以是硬性的，但原则违规永远是判断性意见，且仓库文档化的标准优先于原则
  skill。凡是工具已经强制的东西都跳过。不超过 400 字。」

**Spec sub-agent 的 prompt**——包含：

- diff 命令与提交列表；
- spec 的路径或已取得的内容；
- 简报：「报告：（a）spec 要求但缺失或只实现了一部分的需求；（b）diff 中没被要求的行为
  （范围蔓延）；（c）看起来实现了但实现有误的需求。每条发现都引用对应的 spec 原文。不超过
  400 字。」

如果 spec 缺失，跳过 Spec sub-agent，并在最终报告中注明。

### 5. 汇总

把两份报告分别放在 `## Standards` 与 `## Spec` 标题下呈现，可以逐字照录，也可以轻度清理。
**不要**合并或重排 findings——两条轴是刻意分开的（见*为什么是两条轴*）。

最后给一行小结：每条轴的发现总数，以及*每条轴内部*最严重的问题（如果有）。不要跨轴挑出唯一
的优胜者——那正是把两条轴分开所要防止的重排。

## 为什么是两条轴

一个改动可以通过一条轴，却在另一条轴上失败：

- 遵循了每一项标准，却实现了错误的东西 → **Standards 通过，Spec 失败。**
- 完全实现了要求，却破坏了项目约定 → **Spec 通过，Standards 失败。**

分开报告，可以防止一条轴掩盖另一条轴。
