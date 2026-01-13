# Code Quality Reviewer Prompt Template

# 代码质量审查者提示模板

Use this template when dispatching a code quality reviewer subagent.

调度代码质量审查子代理时使用此模板。

**Purpose:** Verify implementation is well-built (clean, tested, maintainable)

**目的：**验证实施是否构建良好（清洁、经过测试、可维护）

**Only dispatch after spec compliance review passes.**

**仅在规格合规性审查通过后调度。**

```
Task tool (superpowers:code-reviewer):
  Use template at requesting-code-review/code-reviewer.md

  WHAT_WAS_IMPLEMENTED: [from implementer's report]
  PLAN_OR_REQUIREMENTS: Task N from [plan-file]
  BASE_SHA: [commit before task]
  HEAD_SHA: [current commit]
  DESCRIPTION: [task summary]
```

**Code reviewer returns:** Strengths, Issues (Critical/Important/Minor), Assessment

**代码审查者返回：**优势，问题（严重/重要/次要），评估
