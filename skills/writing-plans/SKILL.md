---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code (当你有规格或多步骤任务的需求时，在接触代码之前使用)
---

# Writing Plans

# 编写计划

## Overview

## 概览

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

编写全面的实施计划，假设工程师对我们的代码库零上下文，并且品味有问题。记录他们需要知道的一切：每个任务涉及哪些文件、代码、测试、他们可能需要检查的文档、如何测试它。以一口大小的任务形式给他们整个计划。DRY. YAGNI. TDD. 频繁提交。

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

假设他们是熟练的开发人员，但对我们的工具集或问题领域几乎一无所知。假设他们不太了解好的测试设计。

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**开始时宣布：**“我正在使用 writing-plans 技能来创建实施计划。”

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**上下文：**这也应该在专用工作树（由 brainstorming 技能创建）中运行。

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

**将计划保存到：**`docs/plans/YYYY-MM-DD-<feature-name>.md`

## Bite-Sized Task Granularity

## 一口大小的任务粒度

**Each step is one action (2-5 minutes):**

**每一步都是一个动作（2-5 分钟）：**

- "Write the failing test" - step

- “编写失败的测试” - 步骤

- "Run it to make sure it fails" - step

- “运行它以确保它失败” - 步骤

- "Implement the minimal code to make the test pass" - step

- “实施最小代码以使测试通过” - 步骤

- "Run the tests and make sure they pass" - step

- “运行测试并确其通过” - 步骤

- "Commit" - step

- “提交” - 步骤

## Plan Document Header

## 计划文档标题

**Every plan MUST start with this header:**

**每个计划必须以此标题开始：**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

（

```markdown
# [功能名称] 实施计划

> **对于 Claude：**必需的子技能：使用 superpowers:executing-plans 逐个任务实施此计划。

**目标：**[一句话描述构建的内容]

**架构：**[关于方法的 2-3 句话]

**技术栈：**[关键技术/库]

---
```

）

## Task Structure

## 任务结构

````markdown
### Task N: [Component Name]

**Files:**

- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```
````

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```

````
（
```markdown
### 任务 N: [组件名称]

**文件：**
- 创建：`exact/path/to/file.py`
- 修改：`exact/path/to/existing.py:123-145`
- 测试：`tests/exact/path/to/test.py`

**步骤 1：编写失败的测试**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
````

**步骤 2：运行测试以验证其失败**

运行：`pytest tests/path/test.py::test_name -v`
预期：失败，提示 "function not defined"

**步骤 3：编写最小实施**

```python
def function(input):
    return expected
```

**步骤 4：运行测试以验证其通过**

运行：`pytest tests/path/test.py::test_name -v`
预期：通过

**步骤 5：提交**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```

```
）

## Remember

## 记住

- Exact file paths always

- 始终是准确的文件路径

- Complete code in plan (not "add validation")

- 计划中的完整代码（不是“添加验证”）

- Exact commands with expected output

- 带有预期输出的确切命令

- Reference relevant skills with @ syntax

- 使用 @ 语法引用相关技能

- DRY, YAGNI, TDD, frequent commits

- DRY, YAGNI, TDD, 频繁提交

## Execution Handoff

## 执行移交

After saving the plan, offer execution choice:

保存计划后，提供执行选择：

**"Plan complete and saved to `docs/plans/<filename>.md`. Two execution options:**

**“计划已完成并保存到 `docs/plans/<filename>.md`。两个执行选项：**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**1. 子代理驱动（本次会话）** - 我为每个任务调度新的子代理，在任务之间进行审查，快速迭代

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints

**2. 并行会话（单独）** - 使用 executing-plans 打开新会话，带检查点的批量执行

**Which approach?"**

**哪种方法？”**

**If Subagent-Driven chosen:**

**如果选择子代理驱动：**

- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development

- **必需的子技能：**使用 superpowers:subagent-driven-development

- Stay in this session

- 留在此会话中

- Fresh subagent per task + code review

- 每个任务新的子代理 + 代码审查

**If Parallel Session chosen:**

**如果选择并行会话：**

- Guide them to open new session in worktree

- 引导他们在工作树中打开新会话

- **REQUIRED SUB-SKILL:** New session uses superpowers:executing-plans

- **必需的子技能：**新会话使用 superpowers:executing-plans
```
