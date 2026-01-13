# Defense-in-Depth Validation

# 深度防御验证

## Overview

## 概览

When you fix a bug caused by invalid data, adding validation at one place feels sufficient. But that single check can be bypassed by different code paths, refactoring, or mocks.

当你修复由无效数据引起的错误时，在一个地方添加验证感觉就足够了。但是，该单一检查可能会被不同的代码路径、重构或模拟绕过。

**Core principle:** Validate at EVERY layer data passes through. Make the bug structurally impossible.

**核心原则：**在数据经过的每一层进行验证。使错误在结构上不可能发生。

## Why Multiple Layers

## 为什么多层

Single validation: "We fixed the bug"
Multiple layers: "We made the bug impossible"

单一验证：“我们修复了 bug”
多层：“我们使 bug 变得不可能”

Different layers catch different cases:

- Entry validation catches most bugs
- Business logic catches edge cases
- Environment guards prevent context-specific dangers
- Debug logging helps when other layers fail

不同的层捕获不同的情况：

- 入口验证捕获大多数 bug
- 业务逻辑捕获边缘情况
- 环境防护防止特定上下文的危险
- 调试日志记录在其他层失败时提供帮助

## The Four Layers

## 四层

### Layer 1: Entry Point Validation

**Purpose:** Reject obviously invalid input at API boundary

### 第 1 层：入口点验证

**目的：**在 API 边界拒绝明显无效的输入

```typescript
function createProject(name: string, workingDirectory: string) {
  if (!workingDirectory || workingDirectory.trim() === "") {
    throw new Error("workingDirectory cannot be empty");
  }
  if (!existsSync(workingDirectory)) {
    throw new Error(`workingDirectory does not exist: ${workingDirectory}`);
  }
  if (!statSync(workingDirectory).isDirectory()) {
    throw new Error(`workingDirectory is not a directory: ${workingDirectory}`);
  }
  // ... proceed
}
```

### Layer 2: Business Logic Validation

**Purpose:** Ensure data makes sense for this operation

### 第 2 层：业务逻辑验证

**目的：**确数据对此操作有意义

```typescript
function initializeWorkspace(projectDir: string, sessionId: string) {
  if (!projectDir) {
    throw new Error("projectDir required for workspace initialization");
  }
  // ... proceed
}
```

### Layer 3: Environment Guards

**Purpose:** Prevent dangerous operations in specific contexts

### 第 3 层：环境防护

**目的：**防止特定上下文中的危险操作

```typescript
async function gitInit(directory: string) {
  // In tests, refuse git init outside temp directories
  if (process.env.NODE_ENV === "test") {
    const normalized = normalize(resolve(directory));
    const tmpDir = normalize(resolve(tmpdir()));

    if (!normalized.startsWith(tmpDir)) {
      throw new Error(
        `Refusing git init outside temp dir during tests: ${directory}`
      );
    }
  }
  // ... proceed
}
```

### Layer 4: Debug Instrumentation

**Purpose:** Capture context for forensics

### 第 4 层：调试仪器

**目的：**捕获取证的上下文

```typescript
async function gitInit(directory: string) {
  const stack = new Error().stack;
  logger.debug("About to git init", {
    directory,
    cwd: process.cwd(),
    stack,
  });
  // ... proceed
}
```

## Applying the Pattern

## 应用模式

When you find a bug:

1. **Trace the data flow** - Where does bad value originate? Where used?
2. **Map all checkpoints** - List every point data passes through
3. **Add validation at each layer** - Entry, business, environment, debug
4. **Test each layer** - Try to bypass layer 1, verify layer 2 catches it

当你发现 bug 时：

1. **跟踪数据流** - 坏值从哪里产生？在哪里使用？
2. **映射所有检查点** - 列出数据经过的每个点
3. **在每一层添加验证** - 入口、业务、环境、调试
4. **测试每一层** - 尝试绕过第 1 层，验证第 2 层捕获它

## Example from Session

## 会话示例

Bug: Empty `projectDir` caused `git init` in source code

**Data flow:**

1. Test setup → empty string
2. `Project.create(name, '')`
3. `WorkspaceManager.createWorkspace('')`
4. `git init` runs in `process.cwd()`

**Four layers added:**

- Layer 1: `Project.create()` validates not empty/exists/writable
- Layer 2: `WorkspaceManager` validates projectDir not empty
- Layer 3: `WorktreeManager` refuses git init outside tmpdir in tests
- Layer 4: Stack trace logging before git init

**Result:** All 1847 tests passed, bug impossible to reproduce

Bug：空的 `projectDir` 导致源代码中的 `git init`

**数据流：**

1. 测试设置 → 空字符串
2. `Project.create(name, '')`
3. `WorkspaceManager.createWorkspace('')`
4. `git init` 在 `process.cwd()` 中运行

**添加的四层：**

- 第 1 层：`Project.create()` 验证非空/存在/可写
- 第 2 层：`WorkspaceManager` 验证 projectDir 非空
- 第 3 层：`WorktreeManager` 拒绝在测试中的 tmpdir 之外进行 git init
- 第 4 层：git init 之前的堆栈跟踪日志记录

**结果：**所有 1847 个测试通过，bug 无法重现

## Key Insight

## 关键见解

All four layers were necessary. During testing, each layer caught bugs the others missed:

- Different code paths bypassed entry validation
- Mocks bypassed business logic checks
- Edge cases on different platforms needed environment guards
- Debug logging identified structural misuse

**Don't stop at one validation point.** Add checks at every layer.

这四层都是必要的。在测试期间，每层都捕获了其他层遗漏的错误：

- 不同的代码路径绕过了入口验证
- 模拟绕过了业务逻辑检查
- 不同平台上的边缘情况需要环境防护
- 调试日志记录识别了结构性误用

**不要停在一个验证点上。**在每一层添加检查。
