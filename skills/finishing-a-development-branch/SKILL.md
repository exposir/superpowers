---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup (当实施完成，所有测试通过，你需要决定如何集成工作时使用 - 通过提供结构化的合并、PR 或清理选项来指导开发工作的完成)
---

# Finishing a Development Branch

# 完成开发分支

## Overview

## 概览

Guide completion of development work by presenting clear options and handling chosen workflow.

通过提供明确的选项并处理所选的工作流，指导开发工作的完成。

**Core principle:** Verify tests → Present options → Execute choice → Clean up.

**核心原则：**验证测试 → 展示选项 → 执行选择 → 清理。

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

**开始时宣布：**“我正在使用完成开发分支 (finishing-a-development-branch) 技能来完成这项工作。”

## The Process

## 流程

### Step 1: Verify Tests

### 步骤 1：验证测试

**Before presenting options, verify tests pass:**

**在展示选项之前，验证测试通过：**

```bash
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**If tests fail:**

**如果测试失败：**

```
Tests failing (<N> failures). Must fix before completing:

[Show failures]

Cannot proceed with merge/PR until tests pass.
```

Stop. Don't proceed to Step 2.

停止。不要进行步骤 2。

**If tests pass:** Continue to Step 2.

**如果测试通过：**继续步骤 2。

### Step 2: Determine Base Branch

### 步骤 2：确定基础分支

```bash
# Try common base branches
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

Or ask: "This branch split from main - is that correct?"

或者询问：“这个分支是从 main 分离出来的——对吗？”

### Step 3: Present Options

### 步骤 3：展示选项

Present exactly these 4 options:

展示确切的这 4 个选项：

```
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**Don't add explanation** - keep options concise.

**不要添加解释** - 保持选项简洁。

### Step 4: Execute Choice

### 步骤 4：执行选择

#### Option 1: Merge Locally

#### 选项 1：本地合并

```bash
# Switch to base branch
git checkout <base-branch>

# Pull latest
git pull

# Merge feature branch
git merge <feature-branch>

# Verify tests on merged result
<test command>

# If tests pass
git branch -d <feature-branch>
```

Then: Cleanup worktree (Step 5)

然后：清理工作区（步骤 5）

#### Option 2: Push and Create PR

#### 选项 2：推送并创建 PR

```bash
# Push branch
git push -u origin <feature-branch>

# Create PR
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed>

## Test Plan
- [ ] <verification steps>
EOF
)"
```

Then: Cleanup worktree (Step 5)

然后：清理工作区（步骤 5）

#### Option 3: Keep As-Is

#### 选项 3：保持原样

Report: "Keeping branch <name>. Worktree preserved at <path>."

报告：“保留分支 <name>。工作区保留在 <path>。”

**Don't cleanup worktree.**

**不要清理工作区。**

#### Option 4: Discard

#### 选项 4：放弃

**Confirm first:**

**首先确认：**

```
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

Wait for exact confirmation.

等待确切的确认。

If confirmed:

如果确认：

```bash
git checkout <base-branch>
git branch -D <feature-branch>
```

Then: Cleanup worktree (Step 5)

然后：清理工作区（步骤 5）

### Step 5: Cleanup Worktree

### 步骤 5：清理工作区

**For Options 1, 2, 4:**

**对于选项 1、2、4：**

Check if in worktree:

检查是否在工作区中：

```bash
git worktree list | grep $(git branch --show-current)
```

If yes:

如果是：

```bash
git worktree remove <worktree-path>
```

**For Option 3:** Keep worktree.

**对于选项 3：**保留工作区。

## Quick Reference

## 快速参考

| Option           | Merge | Push | Keep Worktree | Cleanup Branch |
| ---------------- | ----- | ---- | ------------- | -------------- |
| 1. Merge locally | ✓     | -    | -             | ✓              |
| 2. Create PR     | -     | ✓    | ✓             | -              |
| 3. Keep as-is    | -     | -    | ✓             | -              |
| 4. Discard       | -     | -    | -             | ✓ (force)      |

## Common Mistakes

## 常见错误

**Skipping test verification**

**跳过测试验证**

- **Problem:** Merge broken code, create failing PR

- **问题：**合并损坏的代码，创建失败的 PR

- **Fix:** Always verify tests before offering options

- **修复：**在提供选项之前总是验证测试

**Open-ended questions**

**开放式问题**

- **Problem:** "What should I do next?" → ambiguous

- **问题：**“接下来我该做什么？”→ 模棱两可

- **Fix:** Present exactly 4 structured options

- **修复：**确切展示 4 个结构化选项

**Automatic worktree cleanup**

**自动工作区清理**

- **Problem:** Remove worktree when might need it (Option 2, 3)

- **问题：**当可能需要时移除工作区（选项 2、3）

- **Fix:** Only cleanup for Options 1 and 4

- **修复：**仅对选项 1 和 4 进行清理

**No confirmation for discard**

**没有放弃的确认**

- **Problem:** Accidentally delete work

- **问题：**意外删除工作

- **Fix:** Require typed "discard" confirmation

- **修复：**要求键入 "discard" 确认

## Red Flags

## 危险信号

**Never:**

**永远不要：**

- Proceed with failing tests

- 在测试失败时继续

- Merge without verifying tests on result

- 在不验证结果测试的情况下合并

- Delete work without confirmation

- 在没有确认的情况下删除工作

- Force-push without explicit request

- 在没有明确请求的情况下强制推送

**Always:**

**总是：**

- Verify tests before offering options

- 在提供选项之前验证测试

- Present exactly 4 options

- 确切展示 4 个选项

- Get typed confirmation for Option 4

- 获取选项 4 的键入确认

- Clean up worktree for Options 1 & 4 only

- 仅对选项 1 和 4 清理工作区

## Integration

## 集成

**Called by:**

**被调用：**

- **subagent-driven-development** (Step 7) - After all tasks complete

- **subagent-driven-development** (步骤 7) - 在所有任务完成后

- **executing-plans** (Step 5) - After all batches complete

- **executing-plans** (步骤 5) - 在所有批次完成后

**Pairs with:**

**搭配使用：**

- **using-git-worktrees** - Cleans up worktree created by that skill

- **using-git-worktrees** - 清理该技能创建的工作区
