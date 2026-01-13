---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements (当完成任务、实施主要功能或在合并之前使用，以验证工作是否符合要求)
---

# Requesting Code Review

# 请求代码审查

Dispatch superpowers:code-reviewer subagent to catch issues before they cascade.

调度 superpowers:code-reviewer 子代理，在问题级联之前捕获问题。

**Core principle:** Review early, review often.

**核心原则：**尽早审查，经常审查。

## When to Request Review

## 何时请求审查

**Mandatory:**

**强制性：**

- After each task in subagent-driven development

- 在子代理驱动开发中的每个任务之后

- After completing major feature

- 在完成主要功能之后

- Before merge to main

- 在合并到 main 之前

**Optional but valuable:**

**可选但有价值：**

- When stuck (fresh perspective)

- 卡住时（全新的视角）

- Before refactoring (baseline check)

- 重构之前（基线检查）

- After fixing complex bug

- 修复复杂错误之后

## How to Request

## 如何请求

**1. Get git SHAs:**

**1. 获取 git SHAs：**

```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Dispatch code-reviewer subagent:**

**2. 调度 code-reviewer 子代理：**

Use Task tool with superpowers:code-reviewer type, fill template at `code-reviewer.md`

使用 superpowers:code-reviewer 类型的 Task 工具，填写 `code-reviewer.md` 中的模板

**Placeholders:**

**占位符：**

- `{WHAT_WAS_IMPLEMENTED}` - What you just built

- `{WHAT_WAS_IMPLEMENTED}` - 你刚刚构建了什么

- `{PLAN_OR_REQUIREMENTS}` - What it should do

- `{PLAN_OR_REQUIREMENTS}` - 它应该做什么

- `{BASE_SHA}` - Starting commit

- `{BASE_SHA}` - 起始提交

- `{HEAD_SHA}` - Ending commit

- `{HEAD_SHA}` - 结束提交

- `{DESCRIPTION}` - Brief summary

- `{DESCRIPTION}` - 简要总结

**3. Act on feedback:**

**3. 对反馈采取行动：**

- Fix Critical issues immediately

- 立即修复严重 (Critical) 问题

- Fix Important issues before proceeding

- 在继续之前修复重要 (Important) 问题

- Note Minor issues for later

- 记录次要 (Minor) 问题以备后用

- Push back if reviewer is wrong (with reasoning)

- 如果审查者错误，请反驳（并说明理由）

## Example

## 示例

```
[Just completed Task 2: Add verification function]

You: Let me request code review before proceeding.

BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
HEAD_SHA=$(git rev-parse HEAD)

[Dispatch superpowers:code-reviewer subagent]
  WHAT_WAS_IMPLEMENTED: Verification and repair functions for conversation index
  PLAN_OR_REQUIREMENTS: Task 2 from docs/plans/deployment-plan.md
  BASE_SHA: a7981ec
  HEAD_SHA: 3df7661
  DESCRIPTION: Added verifyIndex() and repairIndex() with 4 issue types

[Subagent returns]:
  Strengths: Clean architecture, real tests
  Issues:
    Important: Missing progress indicators
    Minor: Magic number (100) for reporting interval
  Assessment: Ready to proceed

You: [Fix progress indicators]
[Continue to Task 3]
```

## Integration with Workflows

## 与工作流集成

**Subagent-Driven Development:**

**子代理驱动开发：**

- Review after EACH task

- 在每个任务之后审查

- Catch issues before they compound

- 在问题复合之前捕获它们

- Fix before moving to next task

- 在移动到下一个任务之前修复

**Executing Plans:**

**执行计划：**

- Review after each batch (3 tasks)

- 在每批（3 个任务）之后审查

- Get feedback, apply, continue

- 获取反馈，应用，继续

**Ad-Hoc Development:**

**临时开发：**

- Review before merge

- 在合并之前审查

- Review when stuck

- 卡住时审查

## Red Flags

## 危险信号

**Never:**

**永远不要：**

- Skip review because "it's simple"

- 因为“很简单”而跳过审查

- Ignore Critical issues

- 忽略严重 (Critical) 问题

- Proceed with unfixed Important issues

- 继续进行未修复的重要 (Important) 问题

- Argue with valid technical feedback

- 与有效的技术反馈争论

**If reviewer wrong:**

**如果审查者错了：**

- Push back with technical reasoning

- 用技术理由反驳

- Show code/tests that prove it works

- 展示证明其有效的代码/测试

- Request clarification

- 请求澄清

See template at: requesting-code-review/code-reviewer.md

见模板：requesting-code-review/code-reviewer.md
