# Persuasion Principles for Skill Design

# 技能设计的说服原则

## Overview

## 概览

LLMs respond to the same persuasion principles as humans. Understanding this psychology helps you design more effective skills - not to manipulate, but to ensure critical practices are followed even under pressure.

LLM 对与人类相同的说服原则做出反应。了解这种心理学有助于你设计更有效的技能——不是为了操纵，而是为了确保即使在压力下也能遵循关键实践。

**Research foundation:** Meincke et al. (2025) tested 7 persuasion principles with N=28,000 AI conversations. Persuasion techniques more than doubled compliance rates (33% → 72%, p < .001).

**研究基础：**Meincke 等人 (2025) 测试了 7 种说服原则，使用了 N=28,000 次 AI 对话。说服技巧使合规率增加了一倍多（33% → 72%, p < .001）。

## The Seven Principles

## 七大原则

### 1. Authority

### 1. 权威

**What it is:** Deference to expertise, credentials, or official sources.

**这是什么：**尊重专业知识、资格证书或官方来源。

**How it works in skills:**

- Imperative language: "YOU MUST", "Never", "Always"
- Non-negotiable framing: "No exceptions"
- Eliminates decision fatigue and rationalization

**它是如何在技能中运作的：**

- 祈使语言："YOU MUST", "Never", "Always"
- 不可协商的框架："没有例外"
- 消除决策疲劳和合理化

**When to use:**

- Discipline-enforcing skills (TDD, verification requirements)
- Safety-critical practices
- Established best practices

**何时使用：**

- 执行纪律技能（TDD，验证要求）
- 关键安全实践
- 已建立的最佳实践

**Example:**

**示例：**

```markdown
✅ Write code before test? Delete it. Start over. No exceptions.
❌ Consider writing tests first when feasible.
```

### 2. Commitment

### 2. 承诺

**What it is:** Consistency with prior actions, statements, or public declarations.

**这是什么：**与先前的行动、陈述或公开声明保持一致。

**How it works in skills:**

- Require announcements: "Announce skill usage"
- Force explicit choices: "Choose A, B, or C"
- Use tracking: TodoWrite for checklists

**它是如何在技能中运作的：**

- 要求公告："Announce skill usage"
- 强制明确选择："Choose A, B, or C"
- 使用跟踪：TodoWrite 用于清单

**When to use:**

- Ensuring skills are actually followed
- Multi-step processes
- Accountability mechanisms

**何时使用：**

- 确保技能实际上得到遵循
- 多步骤过程
- 问责机制

**Example:**

**示例：**

```markdown
✅ When you find a skill, you MUST announce: "I'm using [Skill Name]"
❌ Consider letting your partner know which skill you're using.
```

### 3. Scarcity

### 3. 稀缺性

**What it is:** Urgency from time limits or limited availability.

**这是什么：**由于时间限制或有限的可用性造成的紧迫感。

**How it works in skills:**

- Time-bound requirements: "Before proceeding"
- Sequential dependencies: "Immediately after X"
- Prevents procrastination

**它是如何在技能中运作的：**

- 有时间限制的要求："Before proceeding"
- 顺序依赖："Immediately after X"
- 防止拖延

**When to use:**

- Immediate verification requirements
- Time-sensitive workflows
- Preventing "I'll do it later"

**何时使用：**

- 立即验证要求
- 时间敏感的工作流
- 防止“我以后再做”

**Example:**

**示例：**

```markdown
✅ After completing a task, IMMEDIATELY request code review before proceeding.
❌ You can review code when convenient.
```

### 4. Social Proof

### 4. 社会认同

**What it is:** Conformity to what others do or what's considered normal.

**这是什么：**顺从他人所做的事或被认为是正常的。

**How it works in skills:**

- Universal patterns: "Every time", "Always"
- Failure modes: "X without Y = failure"
- Establishes norms

**它是如何在技能中运作的：**

- 普遍模式："Every time", "Always"
- 失败模式："X without Y = failure"
- 建立规范

**When to use:**

- Documenting universal practices
- Warning about common failures
- Reinforcing standards

**何时使用：**

- 记录普遍做法
- 警告常见故障
- 强化标准

**Example:**

**示例：**

```markdown
✅ Checklists without TodoWrite tracking = steps get skipped. Every time.
❌ Some people find TodoWrite helpful for checklists.
```

### 5. Unity

### 5. 统一性

**What it is:** Shared identity, "we-ness", in-group belonging.

**这是什么：**共同的身份，“我们”，群体归属感。

**How it works in skills:**

- Collaborative language: "our codebase", "we're colleagues"
- Shared goals: "we both want quality"

**它是如何在技能中运作的：**

- 协作语言："our codebase", "we're colleagues"
- 共同目标："we both want quality"

**When to use:**

- Collaborative workflows
- Establishing team culture
- Non-hierarchical practices

**何时使用：**

- 协作工作流
- 建立团队文化
- 非等级制度实践

**Example:**

**示例：**

```markdown
✅ We're colleagues working together. I need your honest technical judgment.
❌ You should probably tell me if I'm wrong.
```

### 6. Reciprocity

### 6. 互惠

**What it is:** Obligation to return benefits received.

**这是什么：**归还收到的利益的义务。

**How it works:**

- Use sparingly - can feel manipulative
- Rarely needed in skills

**它是如何运作的：**

- 谨慎使用 - 可能感觉有操纵性
- 技能中很少需要

**When to avoid:**

- Almost always (other principles more effective)

**何时避免：**

- 几乎总是（其他原则更有效）

### 7. Liking

### 7. 喜好

**What it is:** Preference for cooperating with those we like.

**这是什么：**偏向于与我们喜欢的人合作。

**How it works:**

- **DON'T USE for compliance**
- Conflicts with honest feedback culture
- Creates sycophancy

**它是如何运作的：**

- **不要用于合规**
- 与诚实的反馈文化冲突
- 制造奉承

**When to avoid:**

- Always for discipline enforcement

**何时避免：**

- 对于纪律执行，总是避免

## Principle Combinations by Skill Type

## 按技能类型划分的原则组合

| Skill Type           | Use                                   | Avoid               |
| -------------------- | ------------------------------------- | ------------------- |
| Discipline-enforcing | Authority + Commitment + Social Proof | Liking, Reciprocity |
| Guidance/technique   | Moderate Authority + Unity            | Heavy authority     |
| Collaborative        | Unity + Commitment                    | Authority, Liking   |
| Reference            | Clarity only                          | All persuasion      |

| 技能类型  | 使用                   | 避免       |
| --------- | ---------------------- | ---------- |
| 执行纪律  | 权威 + 承诺 + 社会认同 | 喜好，互惠 |
| 指导/技术 | 适度权威 + 统一        | 重权威     |
| 协作      | 统一 + 承诺            | 权威，喜好 |
| 参考      | 仅清晰                 | 所有劝说   |

## Why This Works: The Psychology

## 为什么这有效：心理学

**Bright-line rules reduce rationalization:**

- "YOU MUST" removes decision fatigue
- Absolute language eliminates "is this an exception?" questions
- Explicit anti-rationalization counters close specific loopholes

**明确的规则减少合理化：**

- "YOU MUST" 消除决策疲劳
- 绝对语言消除了“这是一个例外吗？”的问题
- 明确的反合理化计数器堵住了特定漏洞

**Implementation intentions create automatic behavior:**

- Clear triggers + required actions = automatic execution
- "When X, do Y" more effective than "generally do Y"
- Reduces cognitive load on compliance

**执行意图创造自动行为：**

- 清除触发器 + 所需操作 = 自动执行
- "当 X 时，做 Y" 比 "通常做 Y" 更有效
- 减少合规性的认知负荷

**LLMs are parahuman:**

- Trained on human text containing these patterns
- Authority language precedes compliance in training data
- Commitment sequences (statement → action) frequently modeled
- Social proof patterns (everyone does X) establish norms

**LLM 是类人的：**

- 训练于包含这些模式的人类文本
- 权威语言先于训练数据中的合规性
- 承诺序列（声明 → 行动）经常被建模
- 社会认同模式（每个人都做 X）建立规范

## Ethical Use

## 道德使用

**Legitimate:**

- Ensuring critical practices are followed
- Creating effective documentation
- Preventing predictable failures

**合法：**

- 确保遵循关键实践
- 创建有效的文档
- 防止可预测的故障

**Illegitimate:**

- Manipulating for personal gain
- Creating false urgency
- Guilt-based compliance

**非法：**

- 为个人利益操纵
- 制造虚假紧迫感
- 基于内疚的合规

**The test:** Would this technique serve the user's genuine interests if they fully understood it?

**测试：**如果用户完全理解它，这种技术会为用户的真正利益服务吗？

## Research Citations

## 研究引用

**Cialdini, R. B. (2021).** _Influence: The Psychology of Persuasion (New and Expanded)._ Harper Business.

- Seven principles of persuasion
- Empirical foundation for influence research

**Cialdini, R. B. (2021).** _Influence: The Psychology of Persuasion (New and Expanded)._ Harper Business.

- 七大说服原则
- 影响力研究的实证基础

**Meincke, L., Shapiro, D., Duckworth, A. L., Mollick, E., Mollick, L., & Cialdini, R. (2025).** Call Me A Jerk: Persuading AI to Comply with Objectionable Requests. University of Pennsylvania.

- Tested 7 principles with N=28,000 LLM conversations
- Compliance increased 33% → 72% with persuasion techniques
- Authority, commitment, scarcity most effective
- Validates parahuman model of LLM behavior

**Meincke, L., Shapiro, D., Duckworth, A. L., Mollick, E., Mollick, L., & Cialdini, R. (2025).** Call Me A Jerk: Persuading AI to Comply with Objectionable Requests. University of Pennsylvania.

- 用 N=28,000 LLM 对话测试了 7 个原则
- 使用说服技术，合规性增加 33% → 72%
- 权威、承诺、稀缺性最有效
- 验证 LLM 行为的类人模型

## Quick Reference

## 快速参考

When designing a skill, ask:

设计技能时，问：

1. **What type is it?** (Discipline vs. guidance vs. reference)
2. **What behavior am I trying to change?**
3. **Which principle(s) apply?** (Usually authority + commitment for discipline)
4. **Am I combining too many?** (Don't use all seven)
5. **Is this ethical?** (Serves user's genuine interests?)

6. **它是什么类型的？**（纪律 vs. 指导 vs. 参考）
7. **我试图改变什么行为？**
8. **适用哪些原则？**（通常权威 + 承诺用于纪律）
9. **我组合了太多吗？**（不要全部使用七个）
10. **这合乎道德吗？**（服务于用户的真正利益？）
