---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes (当遇到任何错误、测试失败或意外行为时，在提出修复方案之前使用)
---

# Systematic Debugging

# 系统化调试

## Overview

## 概览

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

随机修复浪费时间并制造新的错误。快速补丁掩盖了潜在问题。

**Core principle:** ALWAYS find root cause before attempting fixes. Symptom fixes are failure.

**核心原则：**在尝试修复之前**始终**找到根本原因。症状修复是失败的。

**Violating the letter of this process is violating the spirit of debugging.**

**违反此过程的字面意思就是违反调试的精神。**

## The Iron Law

## 铁律

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

（在根本原因调查之前不得进行修复）

If you haven't completed Phase 1, you cannot propose fixes.

如果你没有完成第一阶段，你就不能提出修复方案。

## When to Use

## 何时使用

Use for ANY technical issue:

用于任何技术问题：

- Test failures

- 测试失败

- Bugs in production

- 生产中的 Bug

- Unexpected behavior

- 意外行为

- Performance problems

- 性能问题

- Build failures

- 构建失败

- Integration issues

- 集成问题

**Use this ESPECIALLY when:**

**特别在以下情况使用：**

- Under time pressure (emergencies make guessing tempting)

- 在时间压力下（紧急情况使猜测变得诱人）

- "Just one quick fix" seems obvious

- “只是一个快速修复”似乎显而易见

- You've already tried multiple fixes

- 你已经尝试了多种修复

- Previous fix didn't work

- 之前的修复不起作用

- You don't fully understand the issue

- 你不完全理解这个问题

**Don't skip when:**

**在以下情况不要跳过：**

- Issue seems simple (simple bugs have root causes too)

- 问题看起来很简单（简单的错误也有根本原因）

- You're in a hurry (rushing guarantees rework)

- 你很匆忙（匆忙保证返工）

- Manager wants it fixed NOW (systematic is faster than thrashing)

- 经理希望现在就修复（系统化比盲目尝试更快）

## The Four Phases

## 四个阶段

You MUST complete each phase before proceeding to the next.

你必须在进行下一阶段之前完成每个阶段。

### Phase 1: Root Cause Investigation

### 第一阶段：根本原因调查

**BEFORE attempting ANY fix:**

**在尝试任何修复之前：**

1. **Read Error Messages Carefully**

   - Don't skip past errors or warnings
   - They often contain the exact solution
   - Read stack traces completely
   - Note line numbers, file paths, error codes

1. **仔细阅读错误消息**

   - 不要跳过错误或警告
   - 它们通常包含确切的解决方案
   - 完整阅读堆栈跟踪
   - 注意行号、文件路径、错误代码

1. **Reproduce Consistently**

   - Can you trigger it reliably?
   - What are the exact steps?
   - Does it happen every time?
   - If not reproducible → gather more data, don't guess

1. **一致地重现**

   - 你能可靠地触发它吗？
   - 确切步骤是什么？
   - 它每次都发生吗？
   - 如果不可重现 → 收集更多数据，不要猜测

1. **Check Recent Changes**

   - What changed that could cause this?
   - Git diff, recent commits
   - New dependencies, config changes
   - Environmental differences

1. **检查最近的更改**

   - 发生了什么变化可能导致这种情况？
   - Git diff，最近的提交
   - 新依赖项，配置更改
   - 环境差异

1. **Gather Evidence in Multi-Component Systems**

   **WHEN system has multiple components (CI → build → signing, API → service → database):**

   **BEFORE proposing fixes, add diagnostic instrumentation:**

1. **在多组件系统中收集证据**

   **当系统有多个组件时（CI → 构建 → 签名，API → 服务 → 数据库）：**

   **在提出修复之前，添加诊断仪器：**

   ```
   For EACH component boundary:
     - Log what data enters component
     - Log what data exits component
     - Verify environment/config propagation
     - Check state at each layer

   Run once to gather evidence showing WHERE it breaks
   THEN analyze evidence to identify failing component
   THEN investigate that specific component
   ```

   （
   对于每个组件边界：

   - 记录进入组件的数据
   - 记录离开组件的数据
   - 验证环境/配置传播
   - 检查每一层的状态

   运行一次以收集显示哪里中断的证据
   然后分析证据以识别失败的组件
   然后调查那个特定组件
   ）

   **Example (multi-layer system):**

   **示例（多层系统）：**

   ```bash
   # Layer 1: Workflow
   echo "=== Secrets available in workflow: ==="
   echo "IDENTITY: ${IDENTITY:+SET}${IDENTITY:-UNSET}"

   # Layer 2: Build script
   echo "=== Env vars in build script: ==="
   env | grep IDENTITY || echo "IDENTITY not in environment"

   # Layer 3: Signing script
   echo "=== Keychain state: ==="
   security list-keychains
   security find-identity -v

   # Layer 4: Actual signing
   codesign --sign "$IDENTITY" --verbose=4 "$APP"
   ```

   **This reveals:** Which layer fails (secrets → workflow ✓, workflow → build ✗)

   **这揭示了：**哪一层失败（密钥 → 工作流 ✓，工作流 → 构建 ✗）

1. **Trace Data Flow**

   **WHEN error is deep in call stack:**

1. **跟踪数据流**

   **当错误在调用堆栈深处时：**

   See `root-cause-tracing.md` in this directory for the complete backward tracing technique.

   有关完整的向后跟踪技术，请参阅此目录中的 `root-cause-tracing.md`。

   **Quick version:**

   - Where does bad value originate?
   - What called this with bad value?
   - Keep tracing up until you find the source
   - Fix at source, not at symptom

   **快速版本：**

   - 坏值从哪里产生的？
   - 什么用坏值调用了这个？
   - 继续向上跟踪，直到找到源头
   - 在源头修复，而不是在症状处修复

### Phase 2: Pattern Analysis

### 第二阶段：模式分析

**Find the pattern before fixing:**

**在修复之前找到模式：**

1. **Find Working Examples**

   - Locate similar working code in same codebase
   - What works that's similar to what's broken?

1. **查找工作示例**

   - 在同一代码库中找到类似的工作代码
   - 有什么与损坏的东西类似且工作正常的代码？

1. **Compare Against References**

   - If implementing pattern, read reference implementation COMPLETELY
   - Don't skim - read every line
   - Understand the pattern fully before applying

1. **对照参考进行比较**

   - 如果实施模式，请**完整**阅读参考实施
   - 不要略读 - 阅读每一行
   - 在应用之前充分理解模式

1. **Identify Differences**

   - What's different between working and broken?
   - List every difference, however small
   - Don't assume "that can't matter"

1. **识别差异**

   - 工作和损坏之间有什么不同？
   - 列出每一个差异，无论多么小
   - 不要假设“那没关系”

1. **Understand Dependencies**

   - What other components does this need?
   - What settings, config, environment?
   - What assumptions does it make?

1. **了解依赖关系**
   - 这还需要什么其他组件？
   - 什么设置，配置，环境？
   - 它做出了什么假设？

### Phase 3: Hypothesis and Testing

### 第三阶段：假设与测试

**Scientific method:**

**科学方法：**

1. **Form Single Hypothesis**

   - State clearly: "I think X is the root cause because Y"
   - Write it down
   - Be specific, not vague

1. **形成单一假设**

   - 清楚地陈述：“我认为 X 是根本原因，因为 Y”
   - 写下来
   - 具体，不要模糊

1. **Test Minimally**

   - Make the SMALLEST possible change to test hypothesis
   - One variable at a time
   - Don't fix multiple things at once

1. **最小化测试**

   - 进行**最小**可能的更改以测试假设
   - 一次一个变量
   - 不要一次修复多项内容

1. **Verify Before Continuing**

   - Did it work? Yes → Phase 4
   - Didn't work? Form NEW hypothesis
   - DON'T add more fixes on top

1. **继续之前进行验证**

   - 有用吗？是 → 第四阶段
   - 没用？形成**新**假设
   - **不要**在上面添加更多修复

1. **When You Don't Know**

   - Say "I don't understand X"
   - Don't pretend to know
   - Ask for help
   - Research more

1. **当你不知道时**
   - 说“我不明白 X”
   - 不要假装知道
   - 寻求帮助
   - 更多研究

### Phase 4: Implementation

### 第四阶段：实施

**Fix the root cause, not the symptom:**

**修复根本原因，而不是症状：**

1. **Create Failing Test Case**

   - Simplest possible reproduction
   - Automated test if possible
   - One-off test script if no framework
   - MUST have before fixing
   - Use the `superpowers:test-driven-development` skill for writing proper failing tests

1. **创建失败的测试用例**

   - 最简单的可能重现
   - 如果可能，自动化测试
   - 如果没有框架，一次性测试脚本
   - 修复前**必须**有
   - 使用 `superpowers:test-driven-development` 技能编写正确的失败测试

1. **Implement Single Fix**

   - Address the root cause identified
   - ONE change at a time
   - No "while I'm here" improvements
   - No bundled refactoring

1. **实施单一修复**

   - 解决确定的根本原因
   - 一次一个更改
   - 没有“顺便做”的改进
   - 没有捆绑重构

1. **Verify Fix**

   - Test passes now?
   - No other tests broken?
   - Issue actually resolved?

1. **验证修复**

   - 测试现在通过了吗？
   - 没有其他测试损坏？
   - 问题实际解决了吗？

1. **If Fix Doesn't Work**

   - STOP
   - Count: How many fixes have you tried?
   - If < 3: Return to Phase 1, re-analyze with new information
   - **If ≥ 3: STOP and question the architecture (step 5 below)**
   - DON'T attempt Fix #4 without architectural discussion

1. **如果修复不起作用**

   - 停止
   - 计数：你尝试了多少个修复？
   - 如果 < 3：返回第一阶段，用新信息重新分析
   - **如果 ≥ 3：停止并质疑架构（下面的步骤 5）**
   - 在没有架构讨论的情况下**不要**尝试修复 #4

1. **If 3+ Fixes Failed: Question Architecture**

   **Pattern indicating architectural problem:**

   - Each fix reveals new shared state/coupling/problem in different place
   - Fixes require "massive refactoring" to implement
   - Each fix creates new symptoms elsewhere

   **STOP and question fundamentals:**

   - Is this pattern fundamentally sound?
   - Are we "sticking with it through sheer inertia"?
   - Should we refactor architecture vs. continue fixing symptoms?

   **Discuss with your human partner before attempting more fixes**

   This is NOT a failed hypothesis - this is a wrong architecture.

1. **如果 3+ 个修复失败：质疑架构**

   **表明架构问题的模式：**

   - 每个修复都在不同地方揭示了新的共享状态/耦合/问题
   - 修复需要“大规模重构”才能实施
   - 每个修复都在其他地方产生新症状

   **停止并质疑基本原理：**

   - 这个模式在根本上是合理的吗？
   - 我们是否“仅仅因为惯性而坚持它”？
   - 我们应该重构架构还是继续修复症状？

   **在尝试更多修复之前与你的人类合作伙伴讨论**

   这不是一个失败的假设 - 这是一个错误的架构。

## Red Flags - STOP and Follow Process

## 危险信号 - 停止并遵循流程

If you catch yourself thinking:

- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "Add multiple changes, run tests"
- "Skip the test, I'll manually verify"
- "It's probably X, let me fix that"
- "I don't fully understand but this might work"
- "Pattern says X but I'll adapt it differently"
- "Here are the main problems: [lists fixes without investigation]"
- Proposing solutions before tracing data flow
- **"One more fix attempt" (when already tried 2+)**
- **Each fix reveals new problem in different place**

**ALL of these mean: STOP. Return to Phase 1.**

如果你发现自己在想：

- “现在快速修复，稍后调查”
- “试着改变 X 看看是否有效”
- “添加多个更改，运行测试”
- “跳过测试，我会手动验证”
- “可能是 X，让我修复它”
- “我不完全理解，但这可能会奏效”
- “模式说是 X，但我会以不同方式适应它”
- “主要问题如下：[未调查即列出修复方案]”
- 在跟踪数据流之前提出解决方案
- **“再试一次修复尝试”（当已经尝试了 2+ 次时）**
- **每个修复都在不同地方揭示了新问题**

**所有这些意味着：停止。返回第一阶段。**

**If 3+ fixes failed:** Question the architecture (see Phase 4.5)

**如果 3+ 个修复失败：**质疑架构（见第 4.5 阶段）

## your human partner's Signals You're Doing It Wrong

## 你的人类合作伙伴发出的你做错了的信号

**Watch for these redirections:**

- "Is that not happening?" - You assumed without verifying
- "Will it show us...?" - You should have added evidence gathering
- "Stop guessing" - You're proposing fixes without understanding
- "Ultrathink this" - Question fundamentals, not just symptoms
- "We're stuck?" (frustrated) - Your approach isn't working

**When you see these:** STOP. Return to Phase 1.

**注意这些重定向：**

- “那是没有发生吗？” - 你假设而没有验证
- “它会向我们展示......吗？” - 你应该添加证据收集
- “停止猜测” - 你在不理解的情况下提出修复方案
- “深度思考这个” - 质疑基本原理，不仅仅是症状
- “我们卡住了？”（沮丧） - 你的方法不起作用

**当你看到这些时：**停止。返回第一阶段。

## Common Rationalizations

## 常见的合理化

| Excuse                                       | Reality                                                                 |
| -------------------------------------------- | ----------------------------------------------------------------------- |
| "Issue is simple, don't need process"        | Simple issues have root causes too. Process is fast for simple bugs.    |
| "Emergency, no time for process"             | Systematic debugging is FASTER than guess-and-check thrashing.          |
| "Just try this first, then investigate"      | First fix sets the pattern. Do it right from the start.                 |
| "I'll write test after confirming fix works" | Untested fixes don't stick. Test first proves it.                       |
| "Multiple fixes at once saves time"          | Can't isolate what worked. Causes new bugs.                             |
| "Reference too long, I'll adapt the pattern" | Partial understanding guarantees bugs. Read it completely.              |
| "I see the problem, let me fix it"           | Seeing symptoms ≠ understanding root cause.                             |
| "One more fix attempt" (after 2+ failures)   | 3+ failures = architectural problem. Question pattern, don't fix again. |

| 借口                                 | 现实                                               |
| ------------------------------------ | -------------------------------------------------- |
| “问题很简单，不需要流程”             | 简单的问题也有根本原因。对于简单的错误，流程很快。 |
| “紧急情况，没有时间走流程”           | 系统化调试比猜测和检查的折腾**更快**。             |
| “先试这个，然后调查”                 | 第一个修复设定了模式。从一开始就做对。             |
| “我会在确认修复有效后编写测试”       | 未经测试的修复无法持久。先测试证明它。             |
| “一次修复多个可以节省时间”           | 无法隔离什么起了作用。导致新的错误。               |
| “参考太长，我会调整模式”             | 部分理解保证了错误。完整阅读它。                   |
| “我看到了问题，让我修复它”           | 看到症状 ≠ 理解根本原因。                          |
| “再试一次修复尝试”（在 2+ 次失败后） | 3+ 次失败 = 架构问题。质疑模式，不要再次修复。     |

## Quick Reference

## 快速参考

| Phase                 | Key Activities                                         | Success Criteria            |
| --------------------- | ------------------------------------------------------ | --------------------------- |
| **1. Root Cause**     | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY     |
| **2. Pattern**        | Find working examples, compare                         | Identify differences        |
| **3. Hypothesis**     | Form theory, test minimally                            | Confirmed or new hypothesis |
| **4. Implementation** | Create test, fix, verify                               | Bug resolved, tests pass    |

| 阶段            | 关键活动                           | 成功标准                 |
| --------------- | ---------------------------------- | ------------------------ |
| **1. 根本原因** | 阅读错误，复现，检查更改，收集证据 | 理解**什么**和**为什么** |
| **2. 模式**     | 查找工作示例，比较                 | 识别差异                 |
| **3. 假设**     | 形成理论，最小化测试               | 确认或新假设             |
| **4. 实施**     | 创建测试，修复，验证               | Bug 解决，测试通过       |

## When Process Reveals "No Root Cause"

## 当流程显示“无根本原因”时

If systematic investigation reveals issue is truly environmental, timing-dependent, or external:

1. You've completed the process
2. Document what you investigated
3. Implement appropriate handling (retry, timeout, error message)
4. Add monitoring/logging for future investigation

如果系统化调查显示问题确实是环境性的、依赖于时间的或外部的：

1. 你已经完成了流程
2. 记录你调查的内容
3. 实施适当的处理（重试、超时、错误消息）
4. 添加监控/日志以供将来调查

**But:** 95% of "no root cause" cases are incomplete investigation.

**但是：**95% 的“无根本原因”案例是不完整的调查。

## Supporting Techniques

## 支持技术

These techniques are part of systematic debugging and available in this directory:

- **`root-cause-tracing.md`** - Trace bugs backward through call stack to find original trigger
- **`defense-in-depth.md`** - Add validation at multiple layers after finding root cause
- **`condition-based-waiting.md`** - Replace arbitrary timeouts with condition polling

这些技术是系统化调试的一部分，可在此目录中找到：

- **`root-cause-tracing.md`** - 通过调用堆栈向后跟踪错误以找到原始触发器
- **`defense-in-depth.md`** - 在找到根本原因后，在多层添加验证
- **`condition-based-waiting.md`** - 用条件轮询替换任意超时

**Related skills:**

- **superpowers:test-driven-development** - For creating failing test case (Phase 4, Step 1)
- **superpowers:verification-before-completion** - Verify fix worked before claiming success

**相关技能：**

- **superpowers:test-driven-development** - 用于创建失败的测试用例（第 4 阶段，第 1 步）
- **superpowers:verification-before-completion** - 在声称成功之前验证修复是否有效

## Real-World Impact

## 现实世界的影响

From debugging sessions:

- Systematic approach: 15-30 minutes to fix
- Random fixes approach: 2-3 hours of thrashing
- First-time fix rate: 95% vs 40%
- New bugs introduced: Near zero vs common

来自调试会话：

- 系统化方法：15-30 分钟修复
- 随机修复方法：2-3 小时的折腾
- 首次修复率：95% vs 40%
- 引入的新错误：接近零 vs 常见
