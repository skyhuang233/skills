# CONTEXT.md 格式

## 结构

```md
# {Context 名称}

{一到两句话，说明这个 context 是什么、为什么存在。}

## Language

**Order**：
{一到两句话，说明这个术语的含义}
_Avoid_：Purchase、transaction

**Invoice**：
交付后发给客户的付款请求。
_Avoid_：Bill、payment request

**Customer**：
下单的个人或组织。
_Avoid_：Client、buyer、account
```

## 规则

- **要有主张。** 同一个概念有多个说法时，选最好的那个，其余列入 `_Avoid_`。
- **定义要紧凑。** 最多一到两句话。定义它*是什么*，不是它*做什么*。
- **只收本 context 特有的术语。** 通用编程概念（超时、错误类型、工具模式）即使项目大量使用也不该收。加一个术语前先问：这是本 context 独有的概念，还是通用编程概念？只有前者属于这里。
- **用子标题分组。** 自然聚团出现时按子标题分组；如果所有术语属于同一个内聚领域，平铺即可。

## 单 context 与多 context 仓库

**单 context（多数仓库）：** 根目录一个 `CONTEXT.md`。

**多 context：** 根目录一个 `CONTEXT-MAP.md` 列出各 context、它们的所在位置以及彼此的关系：

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) —— 接收并追踪客户订单
- [Billing](./src/billing/CONTEXT.md) —— 生成发票并处理付款
- [Fulfillment](./src/fulfillment/CONTEXT.md) —— 管理仓库拣货与发货

## Relationships

- **Ordering → Fulfillment**：Ordering 发出 `OrderPlaced` 事件；Fulfillment 消费它们开始拣货
- **Fulfillment → Billing**：Fulfillment 发出 `ShipmentDispatched` 事件；Billing 消费它们生成发票
- **Ordering ↔ Billing**：共享 `CustomerId` 与 `Money` 类型
```

由 skill 自行推断适用哪种结构：

- 存在 `CONTEXT-MAP.md`，读它找到各 context
- 只有根目录 `CONTEXT.md`，单 context
- 两者都不存在，在第一个术语确定时按需创建根目录 `CONTEXT.md`

存在多个 context 时，推断当前话题属于哪一个。判断不清就直接问。
