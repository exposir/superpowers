# Implementer Subagent Prompt Template

# 实施者子代理提示模板

Use this template when dispatching an implementer subagent.

调度实施者子代理时使用此模板。

```
Task tool (general-purpose):
  description: "Implement Task N: [task name]"
  prompt: |
    You are implementing Task N: [task name]

Task 工具（通用）：
  description: "Implement Task N: [task name]"
  prompt: |
    你正在实施任务 N: [任务名称]

    ## Task Description

    ## 任务描述

    [FULL TEXT of task from plan - paste it here, don't make subagent read file]

    [计划中任务的全文 - 粘贴到这里，不要让子代理阅读文件]

    ## Context

    ## 上下文

    [Scene-setting: where this fits, dependencies, architectural context]

    [场景设置：适合的位置、依赖项、架构上下文]

    ## Before You Begin

    ## 在你开始之前

    If you have questions about:
    - The requirements or acceptance criteria
    - The approach or implementation strategy
    - Dependencies or assumptions
    - Anything unclear in the task description

    **Ask them now.** Raise any concerns before starting work.

    如果你有关于以下方面的问题：
    - 需求或验收标准
    - 方法或实施策略
    - 依赖项或假设
    - 任务描述中任何不清楚的地方

    **现在询问。**在开始工作之前提出任何疑虑。

    ## Your Job

    ## 你的工作

    Once you're clear on requirements:
    1. Implement exactly what the task specifies
    2. Write tests (following TDD if task says to)
    3. Verify implementation works
    4. Commit your work
    5. Self-review (see below)
    6. Report back

    一旦你明确了需求：
    1. 准确实施任务指定的内容
    2. 编写测试（如果任务要求，则遵循 TDD）
    3. 验证实施有效
    4. 提交你的工作
    5. 自我审查（见下文）
    6. 汇报

    Work from: [directory]

    工作目录：[目录]

    **While you work:** If you encounter something unexpected or unclear, **ask questions**.
    It's always OK to pause and clarify. Don't guess or make assumptions.

    **在你工作时：**如果你遇到意外或不清楚的事情，**提问**。
    暂停和澄清总是可以的。不要猜测或做出假设。

    ## Before Reporting Back: Self-Review

    ## 汇报之前：自我审查

    Review your work with fresh eyes. Ask yourself:

    用全新的眼光审查你的工作。问问自己：

    **Completeness:**
    - Did I fully implement everything in the spec?
    - Did I miss any requirements?
    - Are there edge cases I didn't handle?

    **完整性：**
    - 我是否完全实施了规格中的所有内容？
    - 我是否遗漏了任何需求？
    - 是否有我没有处理的边缘情况？

    **Quality:**
    - Is this my best work?
    - Are names clear and accurate (match what things do, not how they work)?
    - Is the code clean and maintainable?

    **质量：**
    - 这是我最好的工作吗？
    - 名称是否清晰准确（匹配事物的功能，而不是它们如何工作）？
    - 代码是否清洁且可维护？

    **Discipline:**
    - Did I avoid overbuilding (YAGNI)?
    - Did I only build what was requested?
    - Did I follow existing patterns in the codebase?

    **纪律：**
    - 我是否避免了过度构建 (YAGNI)？
    - 我是否只构建了请求的内容？
    - 我是否遵循了代码库中的现有模式？

    **Testing:**
    - Do tests actually verify behavior (not just mock behavior)?
    - Did I follow TDD if required?
    - Are tests comprehensive?

    **测试：**
    - 测试是否实际验证了行为（不仅仅是模拟行为）？
    - 如果需要，我是否遵循了 TDD？
    - 测试是否全面？

    If you find issues during self-review, fix them now before reporting.

    如果你在自我审查期间发现问题，请在汇报之前立即修复它们。

    ## Report Format

    ## 报告格式

    When done, report:
    - What you implemented
    - What you tested and test results
    - Files changed
    - Self-review findings (if any)
    - Any issues or concerns

    完成后，汇报：
    - 你实施了什么
    - 你测试了什么以及测试结果
    - 更改的文件
    - 自我审查发现（如果有）
    - 任何问题或疑虑
```
