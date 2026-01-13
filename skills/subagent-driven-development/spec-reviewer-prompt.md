# Spec Compliance Reviewer Prompt Template

# 规格合规性审查者提示模板

Use this template when dispatching a spec compliance reviewer subagent.

调度规格合规性审查子代理时使用此模板。

**Purpose:** Verify implementer built what was requested (nothing more, nothing less)

**目的：**验证实施者构建了请求的内容（不多也不少）

```
Task tool (general-purpose):
  description: "Review spec compliance for Task N"
  prompt: |
    You are reviewing whether an implementation matches its specification.

Task 工具（通用）：
  description: "Review spec compliance for Task N"
  prompt: |
    你正在审查实施是否符合其规格。

    ## What Was Requested

    ## 请求了什么

    [FULL TEXT of task requirements]

    [任务需求的全文]

    ## What Implementer Claims They Built

    ## 实施者声称他们构建了什么

    [From implementer's report]

    [来自实施者的报告]

    ## CRITICAL: Do Not Trust the Report

    ## 关键：不要相信报告

    The implementer finished suspiciously quickly. Their report may be incomplete,
    inaccurate, or optimistic. You MUST verify everything independently.

    实施者完成得可疑地快。他们的报告可能不完整、不准确或过于乐观。你必须独立验证所有内容。

    **DO NOT:**
    - Take their word for what they implemented
    - Trust their claims about completeness
    - Accept their interpretation of requirements

    **不要：**
    - 轻信他们实施了什么的说辞
    - 相信他们关于完整性的声明
    - 接受他们对需求的解释

    **DO:**
    - Read the actual code they wrote
    - Compare actual implementation to requirements line by line
    - Check for missing pieces they claimed to implement
    - Look for extra features they didn't mention

    **要做：**
    - 阅读他们编写的实际代码
    - 逐行将实际实施与需求进行比较
    - 检查他们声称已实施但在缺失的部分
    - 寻找他们没有提到的额外功能

    ## Your Job

    ## 你的工作

    Read the implementation code and verify:

    阅读实施代码并验证：

    **Missing requirements:**
    - Did they implement everything that was requested?
    - Are there requirements they skipped or missed?
    - Did they claim something works but didn't actually implement it?

    **缺失的需求：**
    - 他们是否实施了请求的所有内容？
    - 是否有他们跳过或遗漏的需求？
    - 他们是否声称某些东西有效但实际上没有实施？

    **Extra/unneeded work:**
    - Did they build things that weren't requested?
    - Did they over-engineer or add unnecessary features?
    - Did they add "nice to haves" that weren't in spec?

    **额外/不需要的工作：**
    - 他们是否构建了未请求的东西？
    - 他们是否过度工程化或添加了不必要的功能？
    - 他们是否添加了规格中没有的“最好有”的功能？

    **Misunderstandings:**
    - Did they interpret requirements differently than intended?
    - Did they solve the wrong problem?
    - Did they implement the right feature but wrong way?

    **误解：**
    - 他们对需求的解释是否与预期不同？
    - 他们是否解决了错误的问题？
    - 他们是否实施了正确的功能但方式错误？

    **Verify by reading code, not by trusting report.**

    **通过阅读代码进行验证，而不是通过信任报告。**

    Report:
    - ✅ Spec compliant (if everything matches after code inspection)
    - ❌ Issues found: [list specifically what's missing or extra, with file:line references]

    汇报：
    - ✅ 规格合规（如果检查代码后一切匹配）
    - ❌ 发现问题：[列出具体缺失或额外的内容，带有文件:行参考]
```
