---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints (当你有书面的实施计划，需要在单独的会话中执行并进行检查点审查时使用)
---

# Executing Plans

# 执行计划

## Overview

## 概览

Load plan, review critically, execute tasks in batches, report for review between batches.

加载计划，批判性地审查，分批执行任务，在批次之间报告审查。

**Core principle:** Batch execution with checkpoints for architect review.

**核心原则：**带有架构师审查检查点的分批执行。

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**开始时宣布：**“我正在使用执行计划 (executing-plans) 技能来实施此计划。”

## The Process

## 流程

### Step 1: Load and Review Plan

1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Batch

### 步骤 2：执行批次

**Default: First 3 tasks**

**默认：前 3 个任务**

For each task:

对于每个任务：

1. Mark as in_progress

1. 标记为进行中

1. Follow each step exactly (plan has bite-sized steps)

1. 严格遵循每个步骤（计划有小步骤）

1. Run verifications as specified

1. 按指定运行验证

1. Mark as completed

1. 标记为完成

### Step 3: Report

### 步骤 3：报告

When batch complete:

当批次完成时：

- Show what was implemented

- 展示实施了什么

- Show verification output

- 展示验证输出

- Say: "Ready for feedback."

- 说：“准备好接受反馈。”

### Step 4: Continue

### 步骤 4：继续

Based on feedback:

根据反馈：

- Apply changes if needed

- 如果需要，应用更改

- Execute next batch

- 执行下一批次

- Repeat until complete

- 重复直到完成

### Step 5: Complete Development

### 步骤 5：完成开发

After all tasks complete and verified:

在所有任务完成并验证后：

- Announce: "I'm using the finishing-a-development-branch skill to complete this work."

- 宣布：“我正在使用完成开发分支 (finishing-a-development-branch) 技能来完成这项工作。”

- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch

- **必需的子技能：**使用 superpowers:finishing-a-development-branch

- Follow that skill to verify tests, present options, execute choice

- 跟随该技能验证测试，展示选项，执行选择

## When to Stop and Ask for Help

## 何时停止并寻求帮助

**STOP executing immediately when:**

**在以下情况立即停止执行：**

- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)

- 批次中间遇到阻碍（缺少依赖，测试失败，指令不清楚）

- Plan has critical gaps preventing starting

- 计划有阻止开始的关键漏洞

- You don't understand an instruction

- 你不理解指令

- Verification fails repeatedly

- 验证反复失败

**Ask for clarification rather than guessing.**

**寻求澄清而不是猜测。**

## When to Revisit Earlier Steps

## 何时重访早期步骤

**Return to Review (Step 1) when:**

**在以下情况返回审查（步骤 1）：**

- Partner updates the plan based on your feedback

- 合作伙伴根据你的反馈更新计划

- Fundamental approach needs rethinking

- 基本方法需要重新思考

**Don't force through blockers** - stop and ask.

**不要强行突破阻碍** - 停止并询问。

## Remember

## 记住

- Review plan critically first

- 首先批判性地审查计划

- Follow plan steps exactly

- 严格遵循计划步骤

- Don't skip verifications

- 不要跳过验证

- Reference skills when plan says to

- 当计划要求时引用技能

- Between batches: just report and wait

- 批次之间：只是报告并等待

- Stop when blocked, don't guess

- 遇到阻碍时停止，不要猜测
