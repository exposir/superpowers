# Creation Log: Systematic Debugging Skill

# 创建日志：系统化调试技能

Reference example of extracting, structuring, and bulletproofing a critical skill.

提取、构建和强化关键技能的参考示例。

## Source Material

## 源材料

Extracted debugging framework from `/Users/jesse/.claude/CLAUDE.md`:

- 4-phase systematic process (Investigation → Pattern Analysis → Hypothesis → Implementation)
- Core mandate: ALWAYS find root cause, NEVER fix symptoms
- Rules designed to resist time pressure and rationalization

从 `/Users/jesse/.claude/CLAUDE.md` 中提取的调试框架：

- 4 阶段系统化流程（调查 → 模式分析 → 假设 → 实施）
- 核心要求：始终找到根本原因，从不修复症状
- 旨在抵制时间压力和合理化的规则

## Extraction Decisions

## 提取决策

**What to include:**

**包含什么：**

- Complete 4-phase framework with all rules
- Anti-shortcuts ("NEVER fix symptom", "STOP and re-analyze")
- Pressure-resistant language ("even if faster", "even if I seem in a hurry")
- Concrete steps for each phase

- 包含所有规则的完整 4 阶段框架
- 反捷径（“从不修复症状”，“停止并重新分析”）
- 抗压语言（“即使更快”，“即使我看起来很匆忙”）
- 每个阶段的具体步骤

**What to leave out:**

- Project-specific context
- Repetitive variations of same rule
- Narrative explanations (condensed to principles)

**省略什么：**

- 特定于项目的上下文
- 同一规则的重复变体
- 叙述性解释（浓缩为原则）

## Structure Following skill-creation/SKILL.md

## 遵循 skill-creation/SKILL.md 的结构

1. **Rich when_to_use** - Included symptoms and anti-patterns
2. **Type: technique** - Concrete process with steps
3. **Keywords** - "root cause", "symptom", "workaround", "debugging", "investigation"
4. **Flowchart** - Decision point for "fix failed" → re-analyze vs add more fixes
5. **Phase-by-phase breakdown** - Scannable checklist format
6. **Anti-patterns section** - What NOT to do (critical for this skill)

7. **丰富的何时使用** - 包括症状和反模式
8. **类型：技术** - 包含步骤的具体流程
9. **关键词** - “根本原因”，“症状”，“解决方法”，“调试”，“调查”
10. **流程图** - “修复失败”的决策点 → 重新分析 vs 添加更多修复
11. **逐阶段分解** - 可扫描的清单格式
12. **反模式部分** - **不**做什么（对此技能至关重要）

## Bulletproofing Elements

## 强化要素

Framework designed to resist rationalization under pressure:

旨在抵制压力下合理化的框架：

### Language Choices

### 语言选择

- "ALWAYS" / "NEVER" (not "should" / "try to")
- "even if faster" / "even if I seem in a hurry"
- "STOP and re-analyze" (explicit pause)
- "Don't skip past" (catches the actual behavior)

- “始终” / “从不”（不是“应该” / “尝试”）
- “即使更快” / “即使我看起来很匆忙”
- “停止并重新分析”（明确暂停）
- “不要跳过”（捕获实际行为）

### Structural Defenses

### 结构防御

- **Phase 1 required** - Can't skip to implementation
- **Single hypothesis rule** - Forces thinking, prevents shotgun fixes
- **Explicit failure mode** - "IF your first fix doesn't work" with mandatory action
- **Anti-patterns section** - Shows exactly what shortcuts look like

- **第 1 阶段必需** - 不能跳到实施
- **单一假设规则** - 强制思考，防止散弹枪式修复
- **显式故障模式** - “如果你的第一个修复不起作用”带有强制行动
- **反模式部分** - 准确显示捷径的样子

### Redundancy

### 冗余

- Root cause mandate in overview + when_to_use + Phase 1 + implementation rules
- "NEVER fix symptom" appears 4 times in different contexts
- Each phase has explicit "don't skip" guidance

- 概述 + 何时使用 + 第 1 阶段 + 实施规则中的根本原因要求
- “从不修复症状”在不同上下文中出现 4 次
- 每个阶段都有明确的“不要跳过”指导

## Testing Approach

## 测试方法

Created 4 validation tests following skills/meta/testing-skills-with-subagents:

遵循 skills/meta/testing-skills-with-subagents 创建了 4 个验证测试：

### Test 1: Academic Context (No Pressure)

- Simple bug, no time pressure
- **Result:** Perfect compliance, complete investigation

### 测试 1：学术背景（无压力）

- 简单的 bug，没有时间压力
- **结果：**完全合规，完整调查

### Test 2: Time Pressure + Obvious Quick Fix

- User "in a hurry", symptom fix looks easy
- **Result:** Resisted shortcut, followed full process, found real root cause

### 测试 2：时间压力 + 明显的快速修复

- 用户“很匆忙”，症状修复看起来很容易
- **结果：**抵制捷径，遵循完整流程，找到真正的根本原因

### Test 3: Complex System + Uncertainty

- Multi-layer failure, unclear if can find root cause
- **Result:** Systematic investigation, traced through all layers, found source

### 测试 3：复杂系统 + 不确定性

- 多层故障，不清楚是否能找到根本原因
- **结果：**系统化调查，跟踪所有层，找到源头

### Test 4: Failed First Fix

- Hypothesis doesn't work, temptation to add more fixes
- **Result:** Stopped, re-analyzed, formed new hypothesis (no shotgun)

**All tests passed.** No rationalizations found.

### 测试 4：第一次修复失败

- 假设不起作用，诱惑添加更多修复
- **结果：**停止，重新分析，形成新假设（无散弹枪）

**所有测试通过。**未发现合理化。

## Iterations

## 迭代

### Initial Version

### 初始版本

- Complete 4-phase framework
- Anti-patterns section
- Flowchart for "fix failed" decision

- 完整的 4 阶段框架
- 反模式部分
- “修复失败”决策的流程图

### Enhancement 1: TDD Reference

- Added link to skills/testing/test-driven-development
- Note explaining TDD's "simplest code" ≠ debugging's "root cause"
- Prevents confusion between methodologies

### 增强 1：TDD 参考

- 添加了指向 skills/testing/test-driven-development 的链接
- 说明 TDD 的“最简单代码” ≠ 调试的“根本原因”的注释
- 防止方法论之间的混淆

## Final Outcome

## 最终结果

Bulletproof skill that:

- ✅ Clearly mandates root cause investigation
- ✅ Resists time pressure rationalization
- ✅ Provides concrete steps for each phase
- ✅ Shows anti-patterns explicitly
- ✅ Tested under multiple pressure scenarios
- ✅ Clarifies relationship to TDD
- ✅ Ready for use

防弹技能：

- ✅ 明确要求根本原因调查
- ✅ 抵制时间压力合理化
- ✅ 为每个阶段提供具体步骤
- ✅ 明确显示反模式
- ✅ 在多种压力场景下测试
- ✅ 阐明与 TDD 的关系
- ✅ 准备好使用

## Key Insight

## 关键见解

**Most important bulletproofing:** Anti-patterns section showing exact shortcuts that feel justified in the moment. When Claude thinks "I'll just add this one quick fix", seeing that exact pattern listed as wrong creates cognitive friction.

**最重要的强化：**反模式部分显示了在那一刻感觉合理的具体捷径。当 Claude 认为“我就只需添加这个快速修复”时，看到该确切模式被列为错误会产生认知摩擦。

## Usage Example

## 用法示例

When encountering a bug:

1. Load skill: skills/debugging/systematic-debugging
2. Read overview (10 sec) - reminded of mandate
3. Follow Phase 1 checklist - forced investigation
4. If tempted to skip - see anti-pattern, stop
5. Complete all phases - root cause found

**Time investment:** 5-10 minutes
**Time saved:** Hours of symptom-whack-a-mole

当遇到 bug 时：

1. 加载技能：skills/debugging/systematic-debugging
2. 阅读概述（10 秒）- 提醒任务
3. 遵循第 1 阶段检查清单 - 强制调查
4. 如果想跳过 - 查看反模式，停止
5. 完成所有阶段 - 找到根本原因

**时间投入：** 5-10 分钟
**节省时间：** 数小时的打地鼠式修复症状

---

_Created: 2025-10-03_
_Purpose: Reference example for skill extraction and bulletproofing_
