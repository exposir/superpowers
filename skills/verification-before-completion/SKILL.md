---
name: verification-before-completion
description: Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always (在声称工作已完成、修复或通过之前，在提交或创建 PR 之前使用 - 要求在做出任何成功声明之前运行验证命令并确认输出；始终是证据先于断言)
---

# Verification Before Completion

# 完成前验证

## Overview

## 概览

Claiming work is complete without verification is dishonesty, not efficiency.

在没有验证的情况下声称工作已完成是不诚实的，而不是效率。

**Core principle:** Evidence before claims, always.

**核心原则：**始终是证据先于声明。

**Violating the letter of this rule is violating the spirit of this rule.**

**违反此规则的字面意思就是违反此规则的精神。**

## The Iron Law

## 铁律

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

（没有新的验证证据，不得声明完成）

If you haven't run the verification command in this message, you cannot claim it passes.

如果你没有在此消息中运行验证命令，你就不能声称它通过了。

## The Gate Function

## 把关功能

```
BEFORE claiming any status or expressing satisfaction:

1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = lying, not verifying
```

（在声称任何状态或表示满意之前：

1. 识别：什么命令证明此声明？
2. 运行：执行完整命令（新的，完整的）
3. 阅读：完整输出，检查退出代码，计算失败
4. 验证：输出是否确认声明？
   - 如果否：用证据陈述实际状态
   - 如果是：用证据陈述声明
5. 只有那时：做出声明

跳过任何步骤 = 撒谎，未验证）

## Common Failures

## 常见失败

| Claim                 | Requires                        | Not Sufficient                 |
| --------------------- | ------------------------------- | ------------------------------ |
| Tests pass            | Test command output: 0 failures | Previous run, "should pass"    |
| Linter clean          | Linter output: 0 errors         | Partial check, extrapolation   |
| Build succeeds        | Build command: exit 0           | Linter passing, logs look good |
| Bug fixed             | Test original symptom: passes   | Code changed, assumed fixed    |
| Regression test works | Red-green cycle verified        | Test passes once               |
| Agent completed       | VCS diff shows changes          | Agent reports "success"        |
| Requirements met      | Line-by-line checklist          | Tests passing                  |

| 声明         | 需要                 | 不足以                      |
| ------------ | -------------------- | --------------------------- |
| 测试通过     | 测试命令输出：0 失败 | 之前的运行，“应该通过”      |
| Linter 清洁  | Linter 输出：0 错误  | 部分检查，推断              |
| 构建成功     | 构建命令：退出 0     | Linter 通过，日志看起来不错 |
| 错误已修复   | 测试原始症状：通过   | 代码已更改，假设已修复      |
| 回归测试工作 | 红-绿循环已验证      | 测试通过一次                |
| 代理已完成   | VCS diff 显示更改    | 代理报告“成功”              |
| 需求满足     | 逐行检查清单         | 测试通过                    |

## Red Flags - STOP

## 危险信号 - 停止

- Using "should", "probably", "seems to"

- 使用“应该”，“可能”，“似乎”

- Expressing satisfaction before verification ("Great!", "Perfect!", "Done!", etc.)

- 在验证之前表示满意（“太棒了！”，“完美！”，“完成了！”，等）

- About to commit/push/PR without verification

- 即将在没有验证的情况下提交/推送/PR

- Trusting agent success reports

- 信任代理成功报告

- Relying on partial verification

- 依赖部分验证

- Thinking "just this once"

- 想着“就这一次”

- Tired and wanting work over

- 累了想结束工作

- **ANY wording implying success without having run verification**

- **任何暗示成功但未运行验证的措辞**

## Rationalization Prevention

## 防止合理化

| Excuse                                  | Reality                |
| --------------------------------------- | ---------------------- |
| "Should work now"                       | RUN the verification   |
| "I'm confident"                         | Confidence ≠ evidence  |
| "Just this once"                        | No exceptions          |
| "Linter passed"                         | Linter ≠ compiler      |
| "Agent said success"                    | Verify independently   |
| "I'm tired"                             | Exhaustion ≠ excuse    |
| "Partial check is enough"               | Partial proves nothing |
| "Different words so rule doesn't apply" | Spirit over letter     |

| 借口                     | 现实             |
| ------------------------ | ---------------- |
| “现在应该工作了”         | **运行**验证     |
| “我有信心”               | 信心 ≠ 证据      |
| “就这一次”               | 没有例外         |
| “Linter 通过了”          | Linter ≠ 编译器  |
| “代理说成功了”           | 独立验证         |
| “我累了”                 | 疲惫 ≠ 借口      |
| “部分检查就足够了”       | 部分证明不了什么 |
| “措辞不同所以规则不适用” | 精神高于字面     |

## Key Patterns

## 关键模式

**Tests:**

**测试：**

```
✅ [Run test command] [See: 34/34 pass] "All tests pass"
❌ "Should pass now" / "Looks correct"
```

**Regression tests (TDD Red-Green):**

**回归测试 (TDD 红-绿)：**

```
✅ Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass)
❌ "I've written a regression test" (without red-green verification)
```

**Build:**

**构建：**

```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter doesn't check compilation)
```

**Requirements:**

**需求：**

```
✅ Re-read plan → Create checklist → Verify each → Report gaps or completion
❌ "Tests pass, phase complete"
```

**Agent delegation:**

**代理委派：**

```
✅ Agent reports success → Check VCS diff → Verify changes → Report actual state
❌ Trust agent report
```

## Why This Matters

## 为什么这很重要

From 24 failure memories:

来自 24 个失败记忆：

- your human partner said "I don't believe you" - trust broken

- 你的人类合作伙伴说“我不相信你” - 信任破裂

- Undefined functions shipped - would crash

- 发布了未定义的函数 - 会崩溃

- Missing requirements shipped - incomplete features

- 发布了缺失的需求 - 不完整的功能

- Time wasted on false completion → redirect → rework

- 时间浪费在虚假的完成上 → 重定向 → 返工

- Violates: "Honesty is a core value. If you lie, you'll be replaced."

- 违反：“诚实是核心价值观。如果你撒谎，你会被替换。”

## When To Apply

## 何时应用

**ALWAYS before:**

**始终在以下情况之前：**

- ANY variation of success/completion claims

- 任何形式的成功/完成声明

- ANY expression of satisfaction

- 任何满意的表达

- ANY positive statement about work state

- 任何关于工作状态的积极陈述

- Committing, PR creation, task completion

- 提交、创建 PR、任务完成

- Moving to next task

- 移动到下一个任务

- Delegating to agents

- 委派给代理

**Rule applies to:**

**规则适用于：**

- Exact phrases

- 确切的短语

- Paraphrases and synonyms

- 释义和同义词

- Implications of success

- 成功的含义

- ANY communication suggesting completion/correctness

- 任何暗示完成/正确性的沟通

## The Bottom Line

## 底线

**No shortcuts for verification.**

**验证没有捷径。**

Run the command. Read the output. THEN claim the result.

运行命令。阅读输出。**然后**声称结果。

This is non-negotiable.

这是不可商量的。
