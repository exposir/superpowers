# Testing Anti-Patterns

# 测试反模式

**Load this reference when:** writing or changing tests, adding mocks, or tempted to add test-only methods to production code.

**当：**编写或更改测试、添加模拟或想向生产代码添加仅限测试的方法时加载此参考。

## Overview

## 概览

Tests must verify real behavior, not mock behavior. Mocks are a means to isolate, not the thing being tested.

测试必须验证真实行为，而不是模拟行为。模拟是隔离的一种手段，而不是被测试的东西。

**Core principle:** Test what the code does, not what the mocks do.

**核心原则：**测试代码做什么，而不是模拟做什么。

**Following strict TDD prevents these anti-patterns.**

**遵循严格的 TDD 可以防止这些反模式。**

## The Iron Laws

## 铁律

```
1. NEVER test mock behavior
2. NEVER add test-only methods to production classes
3. NEVER mock without understanding dependencies
```

```
1. 永远不要测试模拟行为
2. 永远不要向生产类添加仅限测试的方法
3. 永远不要在不了解依赖关系的情况下进行模拟
```

## Anti-Pattern 1: Testing Mock Behavior

## 反模式 1：测试模拟行为

**The violation:**

**违规行为：**

```typescript
// ❌ BAD: Testing that the mock exists
test("renders sidebar", () => {
  render(<Page />);
  expect(screen.getByTestId("sidebar-mock")).toBeInTheDocument();
});
```

**Why this is wrong:**

- You're verifying the mock works, not that the component works
- Test passes when mock is present, fails when it's not
- Tells you nothing about real behavior

**为什么这是错的：**

- 你正在验证模拟工作，而不是组件工作
- 当模拟存在时测试通过，当它不存在时失败
- 没有任何关于真实行为的信息

**your human partner's correction:** "Are we testing the behavior of a mock?"

**你的人类合作伙伴的纠正：**“我们在测试模拟的行为吗？”

**The fix:**

**修复：**

```typescript
// ✅ GOOD: Test real component or don't mock it
test("renders sidebar", () => {
  render(<Page />); // Don't mock sidebar
  expect(screen.getByRole("navigation")).toBeInTheDocument();
});

// OR if sidebar must be mocked for isolation:
// Don't assert on the mock - test Page's behavior with sidebar present
```

### Gate Function

```
BEFORE asserting on any mock element:
Ask: "Am I testing real component behavior or just mock existence?"

IF testing mock existence:
STOP - Delete the assertion or unmock the component

Test real behavior instead
```

```
在断言任何模拟元素之前：
问：“我是在测试真实的组件行为还是仅仅是模拟的存在？”

如果测试模拟存在：
停止 - 删除断言或取消模拟组件

改为测试真实行为
```

## Anti-Pattern 2: Test-Only Methods in Production

## 反模式 2：生产中的仅限测试的方法

**The violation:**

**违规行为：**

```typescript
// ❌ BAD: destroy() only used in tests
class Session {
  async destroy() {
    // Looks like production API!
    await this._workspaceManager?.destroyWorkspace(this.id);
    // ... cleanup
  }
}

// In tests
afterEach(() => session.destroy());
```

**Why this is wrong:**

- Production class polluted with test-only code
- Dangerous if accidentally called in production
- Violates YAGNI and separation of concerns
- Confuses object lifecycle with entity lifecycle

**为什么这是错的：**

- 生产类被仅限测试的代码污染
- 如果在生产中意外调用会很危险
- 违反 YAGNI 和关注点分离
- 混淆对象生命周期与实体生命周期

**The fix:**

**修复：**

```typescript
// ✅ GOOD: Test utilities handle test cleanup
// Session has no destroy() - it's stateless in production

// In test-utils/
export async function cleanupSession(session: Session) {
  const workspace = session.getWorkspaceInfo();
  if (workspace) {
    await workspaceManager.destroyWorkspace(workspace.id);
  }
}

// In tests
afterEach(() => cleanupSession(session));
```

### Gate Function

### 门控功能

```
BEFORE adding any method to production class:
Ask: "Is this only used by tests?"

IF yes:
STOP - Don't add it
Put it in test utilities instead

Ask: "Does this class own this resource's lifecycle?"

IF no:
STOP - Wrong class for this method
```

```
在向生产类添加任何方法之前：
问：“这是否仅用于测试？”

如果是：
停止 - 不要添加它
把它放在测试实用程序中

问：“这个类是否拥有这个资源的生命周期？”

如果否：
停止 - 对于此方法来说是错误的类
```

## Anti-Pattern 3: Mocking Without Understanding

## 反模式 3：在不了解的情况下进行模拟

**The violation:**

**违规行为：**

```typescript
// ❌ BAD: Mock breaks test logic
test("detects duplicate server", () => {
  // Mock prevents config write that test depends on!
  vi.mock("ToolCatalog", () => ({
    discoverAndCacheTools: vi.fn().mockResolvedValue(undefined),
  }));

  await addServer(config);
  await addServer(config); // Should throw - but won't!
});
```

**Why this is wrong:**

- Mocked method had side effect test depended on (writing config)
- Over-mocking to "be safe" breaks actual behavior
- Test passes for wrong reason or fails mysteriously

**为什么这是错的：**

- 被模拟的方法有测试依赖的副作用（编写配置）
- 为了“安全”过度模拟破坏了实际行为
- 测试因错误原因通过或莫名其妙地失败

**The fix:**

**修复：**

```typescript
// ✅ GOOD: Mock at correct level
test("detects duplicate server", () => {
  // Mock the slow part, preserve behavior test needs
  vi.mock("MCPServerManager"); // Just mock slow server startup

  await addServer(config); // Config written
  await addServer(config); // Duplicate detected ✓
});
```

### Gate Function

### 门控功能

```
BEFORE mocking any method:
STOP - Don't mock yet

1. Ask: "What side effects does the real method have?"
2. Ask: "Does this test depend on any of those side effects?"
3. Ask: "Do I fully understand what this test needs?"

IF depends on side effects:
Mock at lower level (the actual slow/external operation)
OR use test doubles that preserve necessary behavior
NOT the high-level method the test depends on

IF unsure what test depends on:
Run test with real implementation FIRST
Observe what actually needs to happen
THEN add minimal mocking at the right level

Red flags: - "I'll mock this to be safe" - "This might be slow, better mock it" - Mocking without understanding the dependency chain
```

```
在模拟任何方法之前：
停止 - 先不要模拟

1. 问：“真正的方法有什么副作用？”
2. 问：“此测试是否依赖于任何这些副作用？”
3. 问：“我完全理解这个测试需要什么吗？”

如果依赖副作用：
在较低级别进行模拟（实际缓慢/外部操作）
或者使用保留必要行为的测试替身
不是测试依赖的高级方法

如果不确定测试依赖什么：
首先运行带有真实实现的测试
观察实际上需要发生什么
在正确的级别添加最小的模拟

危险信号： - “我要模拟这个以防万一” - “这可能会很慢，最好模拟它” - 在不了解依赖链的情况下进行模拟
```

## Anti-Pattern 4: Incomplete Mocks

## 反模式 4：不完整的模拟

**The violation:**

**违规行为：**

```typescript
// ❌ BAD: Partial mock - only fields you think you need
const mockResponse = {
  status: "success",
  data: { userId: "123", name: "Alice" },
  // Missing: metadata that downstream code uses
};

// Later: breaks when code accesses response.metadata.requestId
```

**Why this is wrong:**

- **Partial mocks hide structural assumptions** - You only mocked fields you know about
- **Downstream code may depend on fields you didn't include** - Silent failures
- **Tests pass but integration fails** - Mock incomplete, real API complete
- **False confidence** - Test proves nothing about real behavior

**为什么这是错的：**

- **部分模拟隐藏了结构假设** - 你只模拟了你知道的字段
- **下游代码可能依赖于你未包含的字段** - 沉默的失败
- **测试通过但集成失败** - 模拟不完整，真实 API 完整
- **错误的信心** - 测试不能证明真实行为

**The Iron Rule:** Mock the COMPLETE data structure as it exists in reality, not just fields your immediate test uses.

**铁律：**模拟现实中存在的**完整**数据结构，而不仅仅是你直接测试使用的字段。

**The fix:**

**修复：**

```typescript
// ✅ GOOD: Mirror real API completeness
const mockResponse = {
  status: "success",
  data: { userId: "123", name: "Alice" },
  metadata: { requestId: "req-789", timestamp: 1234567890 },
  // All fields real API returns
};
```

### Gate Function

### 门控功能

```
BEFORE creating mock responses:
Check: "What fields does the real API response contain?"

Actions: 1. Examine actual API response from docs/examples 2. Include ALL fields system might consume downstream 3. Verify mock matches real response schema completely

Critical:
If you're creating a mock, you must understand the ENTIRE structure
Partial mocks fail silently when code depends on omitted fields

If uncertain: Include all documented fields
```

```
在创建模拟响应之前：
检查：“真实的 API 响应包含哪些字段？”

行动： 1. 检查来自文档/示例的实际 API 响应 2. 包含系统下游可能消耗的所有字段 3. 验证模拟完全匹配实际响应模式

关键：
如果你正在创建一个模拟，你必须了解整个结构
当代码依赖于省略的字段时，部分模拟会静默失败

如果不确定：包含所有记录的字段
```

## Anti-Pattern 5: Integration Tests as Afterthought

## 反模式 5：集成测试作为事后想法

**The violation:**

**违规行为：**

```
✅ Implementation complete
❌ No tests written
"Ready for testing"
```

**Why this is wrong:**

- Testing is part of implementation, not optional follow-up
- TDD would have caught this
- Can't claim complete without tests

**为什么这是错的：**

- 测试是实施的一部分，而不是可选的后续行动
- TDD 会抓住这一点
- 没有测试就不能声称完成

**The fix:**

**修复：**

```
TDD cycle:
1. Write failing test
2. Implement to pass
3. Refactor
4. THEN claim complete
```

## When Mocks Become Too Complex

## 当模拟变得太复杂时

**Warning signs:**

- Mock setup longer than test logic
- Mocking everything to make test pass
- Mocks missing methods real components have
- Test breaks when mock changes

**警告信号：**

- 模拟设置比测试逻辑长
- 模拟一切以使测试通过
- 模拟缺少真实组件具有的方法
- 当模拟改变时测试中断

**your human partner's question:** "Do we need to be using a mock here?"

**你的人类合作伙伴的问题：**“我们需要在这里使用模拟吗？”

**Consider:** Integration tests with real components often simpler than complex mocks

**考虑：**带有真实组件的集成测试通常比复杂的模拟更简单

## TDD Prevents These Anti-Patterns

## TDD 防止这些反模式

**Why TDD helps:**

1. **Write test first** → Forces you to think about what you're actually testing
2. **Watch it fail** → Confirms test tests real behavior, not mocks
3. **Minimal implementation** → No test-only methods creep in
4. **Real dependencies** → You see what the test actually needs before mocking

**TDD 为什么通过帮助：**

1. **先写测试** → 强迫你思考你实际上在测试什么
2. **看它失败** → 确认测试测试真实行为，而不是模拟
3. **最小实现** → 没有仅限测试的方法潜入
4. **真实依赖** → 你在模拟之前看到测试实际上需要什么

**If you're testing mock behavior, you violated TDD** - you added mocks without watching test fail against real code first.

**如果你正在测试模拟行为，你违反了 TDD** - 你添加了模拟而没有先看着测试在真实代码上失败。

## Quick Reference

## 快速参考

| Anti-Pattern                    | Fix                                           |
| ------------------------------- | --------------------------------------------- |
| Assert on mock elements         | Test real component or unmock it              |
| Test-only methods in production | Move to test utilities                        |
| Mock without understanding      | Understand dependencies first, mock minimally |
| Incomplete mocks                | Mirror real API completely                    |
| Tests as afterthought           | TDD - tests first                             |
| Over-complex mocks              | Consider integration tests                    |

| 反模式                   | 修复                             |
| ------------------------ | -------------------------------- |
| 对模拟元素进行断言       | 测试真实组件或取消模拟           |
| 生产中的仅限测试方法     | 移动到测试实用程序               |
| 在不了解的情况下进行模拟 | 首先了解依赖关系，最小程度地模拟 |
| 不完整的模拟             | 完全镜像真实 API                 |
| 测试作为事后想法         | TDD - 测试优先                   |
| 过于复杂的模拟           | 考虑集成测试                     |

## Red Flags

## 危险信号

- Assertion checks for `*-mock` test IDs
- Methods only called in test files
- Mock setup is >50% of test
- Test fails when you remove mock
- Can't explain why mock is needed
- Mocking "just to be safe"

- 断言检查 `*-mock` 测试 ID
- 仅在测试文件中调用的方法
- 模拟设置 >50% 的测试
- 当你移除模拟时测试失败
- 无法解释为什么需要模拟
- 模拟“只是为了安全”

## The Bottom Line

## 底线

**Mocks are tools to isolate, not things to test.**

**模拟是隔离的工具，而不是被测试的东西。**

If TDD reveals you're testing mock behavior, you've gone wrong.

如果 TDD 显示你正在测试模拟行为，你就错了。

Fix: Test real behavior or question why you're mocking at all.

修复：测试真实行为或质疑你为什么要模拟。
