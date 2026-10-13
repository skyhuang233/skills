# 好测试与坏测试

## 好测试

**集成风格**：通过真实接口测试，而不是 mock 内部零件。

```typescript
// 好：测试可观察的行为
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特征：

- 测试用户/调用者真正关心的行为
- 只使用公开 API
- 能在内部重构后存活
- 描述做什么（WHAT），而不是怎么做（HOW）
- 每个测试一个逻辑断言

## 坏测试

**实现细节测试**：与内部结构耦合。

```typescript
// 坏：测试实现细节
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

危险信号：

- mock 内部协作者
- 测试私有方法
- 断言调用次数或调用顺序
- 重构后行为没变，测试却红了
- 测试名描述的是怎么做（HOW）而不是做什么（WHAT）
- 通过外部手段验证，而不是通过接口验证

```typescript
// 坏：绕过接口去验证
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// 好：通过接口验证
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

**同义反复测试**：期望值复述了实现，于是测试按构造必然通过。

```typescript
// 坏：期望值用代码的算法重算了一遍
test("calculateTotal sums line items", () => {
  const items = [{ price: 10 }, { price: 5 }];
  const expected = items.reduce((sum, i) => sum + i.price, 0);
  expect(calculateTotal(items)).toBe(expected);
});

// 好：期望值是一个独立的、已知的字面量
test("calculateTotal sums line items", () => {
  expect(calculateTotal([{ price: 10 }, { price: 5 }])).toBe(15);
});
```

## 入口可以换，契约不变

同一个契约，先用最外层入口测；内部结构调整后，换到新入口，断言一字不改。

```typescript
// 契约：保存后恢复，读回的状态与保存前一致。
// 入口 v1：checkpoint_snapshot() / restore()，当时是最外层。
test("restore returns the state that was saved", async () => {
  const session = await openSession();
  session.learn(events);
  const before = session.snapshotId();

  const checkpoint = await session.checkpoint_snapshot();
  const restored = await restore(checkpoint);

  expect(restored.snapshotId()).toBe(before);
});

// 内部结构变了：snapshot 系列合并成一个入口。断言没有变。
test("restore returns the state that was saved", async () => {
  const session = await openSession();
  session.learn(events);
  const before = session.stateId();

  const checkpoint = await session.save();
  const restored = await load(checkpoint);

  expect(restored.stateId()).toBe(before);
});
```

断言沉在观察上——「读回的状态和保存前一致」——而不是沉在 `checkpoint_snapshot` 这个名字上。
名字换了就跟着搬，测试不会因此变红。反过来，如果一开始断言的是「`checkpoint_snapshot()` 被
调用了一次」，这次合并就会变成几十个红灯，而没有任何行为变化。
