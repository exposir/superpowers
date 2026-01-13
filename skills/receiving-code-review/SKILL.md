---
name: receiving-code-review
description: Use when receiving code review feedback, before implementing suggestions, especially if feedback seems unclear or technically questionable - requires technical rigor and verification, not performative agreement or blind implementation (当接收代码审查反馈时，在实施建议之前使用，特别是如果反馈看起来不清楚或在技术上有疑问 - 需要技术严谨和验证，而不是表面上的同意或盲目实施)
---

# Code Review Reception

# 代码审查接收

## Overview

## 概览

Code review requires technical evaluation, not emotional performance.

代码审查需要技术评估，而不是情感表达。

**Core principle:** Verify before implementing. Ask before assuming. Technical correctness over social comfort.

**核心原则：**实施前验证。假设前询问。技术正确性高于社交舒适度。

## The Response Pattern

## 响应模式

```
WHEN receiving code review feedback:

1. READ: Complete feedback without reacting
2. UNDERSTAND: Restate requirement in own words (or ask)
3. VERIFY: Check against codebase reality
4. EVALUATE: Technically sound for THIS codebase?
5. RESPOND: Technical acknowledgment or reasoned pushback
6. IMPLEMENT: One item at a time, test each
```

（当接收代码审查反馈时：

1. 阅读：完整阅读反馈而不做出反应
2. 理解：用自己的话重述需求（或询问）
3. 验证：对照代码库现实进行检查
4. 评估：对此代码库在技术上是否合理？
5. 响应：技术确认或有理有据的反驳
6. 实施：一次一项，测试每一项）

## Forbidden Responses

## 禁止的响应

**NEVER:**

**绝对不要：**

- "You're absolutely right!" (explicit CLAUDE.md violation)

- “你是绝对正确的！”（明确违反 CLAUDE.md）

- "Great point!" / "Excellent feedback!" (performative)

- “很好的观点！” / “极好的反馈！”（表演性的）

- "Let me implement that now" (before verification)

- “让我现在就实施……”（在验证之前）

**INSTEAD:**

**取而代之的是：**

- Restate the technical requirement

- 重述技术需求

- Ask clarifying questions

- 询问澄清问题

- Push back with technical reasoning if wrong

- 如果错误，用技术理由反驳

- Just start working (actions > words)

- 直接开始工作（行动 > 言语）

## Handling Unclear Feedback

## 处理不清楚的反馈

```
IF any item is unclear:
  STOP - do not implement anything yet
  ASK for clarification on unclear items

WHY: Items may be related. Partial understanding = wrong implementation.
```

（如果任何项目不清楚：
停止 - 暂时不要实施任何东西
对不清楚的项目寻求澄清

为什么：项目可能是相关的。部分理解 = 错误的实施。）

**Example:**

**示例：**

```
your human partner: "Fix 1-6"
You understand 1,2,3,6. Unclear on 4,5.

❌ WRONG: Implement 1,2,3,6 now, ask about 4,5 later
✅ RIGHT: "I understand items 1,2,3,6. Need clarification on 4 and 5 before proceeding."
```

## Source-Specific Handling

## 特定来源的处理

### From your human partner

### 来自你的人类合作伙伴

- **Trusted** - implement after understanding

- **受信任的** - 理解后实施

- **Still ask** if scope unclear

- 如果范围不清楚，**仍然要问**

- **No performative agreement**

- **没有表演性的同意**

- **Skip to action** or technical acknowledgment

- **跳过直接行动**或技术确认

### From External Reviewers

### 来自外部审查者

```
BEFORE implementing:
  1. Check: Technically correct for THIS codebase?
  2. Check: Breaks existing functionality?
  3. Check: Reason for current implementation?
  4. Check: Works on all platforms/versions?
  5. Check: Does reviewer understand full context?

IF suggestion seems wrong:
  Push back with technical reasoning

IF can't easily verify:
  Say so: "I can't verify this without [X]. Should I [investigate/ask/proceed]?"

IF conflicts with your human partner's prior decisions:
  Stop and discuss with your human partner first
```

**your human partner's rule:** "External feedback - be skeptical, but check carefully"

**你的人类合作伙伴的规则：**“外部反馈 - 保持怀疑，但要仔细检查”

## YAGNI Check for "Professional" Features

## 对“专业”功能的 YAGNI 检查

```
IF reviewer suggests "implementing properly":
  grep codebase for actual usage

  IF unused: "This endpoint isn't called. Remove it (YAGNI)?"
  IF used: Then implement properly
```

**your human partner's rule:** "You and reviewer both report to me. If we don't need this feature, don't add it."

**你的人类合作伙伴的规则：**“你和审查者都向我汇报。如果我们不需要这个功能，就不要添加它。”

## Implementation Order

## 实施顺序

```
FOR multi-item feedback:
  1. Clarify anything unclear FIRST
  2. Then implement in this order:
     - Blocking issues (breaks, security)
     - Simple fixes (typos, imports)
     - Complex fixes (refactoring, logic)
  3. Test each fix individually
  4. Verify no regressions
```

（对于多项反馈：

1. 首先澄清任何不清楚的地方
2. 然后按此顺序实施：
   - 阻塞性问题（中断、安全）
   - 简单修复（拼写错误、导入）
   - 复杂修复（重构、逻辑）
3. 单独测试每个修复
4. 验证没有回归）

## When To Push Back

## 何时反驳

Push back when:

在以下情况反驳：

- Suggestion breaks existing functionality

- 建议破坏了现有功能

- Reviewer lacks full context

- 审查者缺乏完整的上下文

- Violates YAGNI (unused feature)

- 违反 YAGNI（未使用的功能）

- Technically incorrect for this stack

- 对此技术栈在技术上不正确

- Legacy/compatibility reasons exist

- 存在遗留/兼容性原因

- Conflicts with your human partner's architectural decisions

- 与你的人类合作伙伴的架构决策冲突

**How to push back:**

**如何反驳：**

- Use technical reasoning, not defensiveness

- 使用技术理由，而不是防御

- Ask specific questions

- 询问具体问题

- Reference working tests/code

- 引用工作的测试/代码

- Involve your human partner if architectural

-如果是架构问题，让你的人类合作伙伴参与

**Signal if uncomfortable pushing back out loud:** "Strange things are afoot at the Circle K"

**如果不愿意大声反驳，发出信号：**“Circle K 发生了奇怪的事情”

## Acknowledging Correct Feedback

## 确认正确的反馈

When feedback IS correct:

当反馈是正确的时候：

```
✅ "Fixed. [Brief description of what changed]"
✅ "Good catch - [specific issue]. Fixed in [location]."
✅ [Just fix it and show in the code]

❌ "You're absolutely right!"
❌ "Great point!"
❌ "Thanks for catching that!"
❌ "Thanks for [anything]"
❌ ANY gratitude expression
```

**Why no thanks:** Actions speak. Just fix it. The code itself shows you heard the feedback.

**为什么不感谢：**行动说明一切。直接修复它。代码本身表明你听到了反馈。

**If you catch yourself about to write "Thanks":** DELETE IT. State the fix instead.

**如果你发现自己正要写“谢谢”：**删除它。陈述修复。

## Gracefully Correcting Your Pushback

## 优雅地纠正你的反驳

If you pushed back and were wrong:

如果你反驳了但错了：

```
✅ "You were right - I checked [X] and it does [Y]. Implementing now."
✅ "Verified this and you're correct. My initial understanding was wrong because [reason]. Fixing."

❌ Long apology
❌ Defending why you pushed back
❌ Over-explaining
```

State the correction factually and move on.

实事求是地陈述纠正并继续。

## Common Mistakes

## 常见错误

| Mistake                      | Fix                                 |
| ---------------------------- | ----------------------------------- |
| Performative agreement       | State requirement or just act       |
| Blind implementation         | Verify against codebase first       |
| Batch without testing        | One at a time, test each            |
| Assuming reviewer is right   | Check if breaks things              |
| Avoiding pushback            | Technical correctness > comfort     |
| Partial implementation       | Clarify all items first             |
| Can't verify, proceed anyway | State limitation, ask for direction |

| 错误               | 修复                 |
| ------------------ | -------------------- |
| 表演性同意         | 陈述需求或直接行动   |
| 盲目实施           | 先对照代码库验证     |
| 批量不测试         | 一次一个，测试每一个 |
| 假设审查者是对的   | 检查是否破坏事物     |
| 避免反驳           | 技术正确性 > 舒适度  |
| 部分实施           | 先澄清所有项目       |
| 无法验证，仍然继续 | 陈述限制，寻求指导   |

## Real Examples

## 真实示例

**Performative Agreement (Bad):**

**表演性同意（坏）：**

```
Reviewer: "Remove legacy code"
❌ "You're absolutely right! Let me remove that..."
```

**Technical Verification (Good):**

**技术验证（好）：**

```
Reviewer: "Remove legacy code"
✅ "Checking... build target is 10.15+, this API needs 13+. Need legacy for backward compat. Current impl has wrong bundle ID - fix it or drop pre-13 support?"
```

**YAGNI (Good):**

**YAGNI（好）：**

```
Reviewer: "Implement proper metrics tracking with database, date filters, CSV export"
✅ "Grepped codebase - nothing calls this endpoint. Remove it (YAGNI)? Or is there usage I'm missing?"
```

**Unclear Item (Good):**

**不清楚的项目（好）：**

```
your human partner: "Fix items 1-6"
You understand 1,2,3,6. Unclear on 4,5.
✅ "Understand 1,2,3,6. Need clarification on 4 and 5 before implementing."
```

## GitHub Thread Replies

## GitHub 线程回复

When replying to inline review comments on GitHub, reply in the comment thread (`gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`), not as a top-level PR comment.

当回复 GitHub 上的内联审查评论时，在评论线程中回复 (`gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`)，而不是作为顶级 PR 评论。

## The Bottom Line

## 底线

**External feedback = suggestions to evaluate, not orders to follow.**

**外部反馈 = 评估的建议，而不是遵循的命令。**

Verify. Question. Then implement.

验证。质疑。然后实施。

No performative agreement. Technical rigor always.

没有表演性的同意。始终保持技术严谨。
