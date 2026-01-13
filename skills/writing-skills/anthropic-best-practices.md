# Skill authoring best practices

# 技能创作最佳实践

> Learn how to write effective Skills that Claude can discover and use successfully.
>
> 了解如何编写 Claude 可以发现并成功使用的有效技能。

Good Skills are concise, well-structured, and tested with real usage. This guide provides practical authoring decisions to help you write Skills that Claude can discover and use effectively.

好的技能简洁、结构良好，并经过实际使用测试。本指南提供了实用的创作决策，以帮助你编写 Claude 可以发现并有效使用的技能。

For conceptual background on how Skills work, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview).

有关技能如何工作的概念背景，请参阅 [技能概述](/en/docs/agents-and-tools/agent-skills/overview)。

## Core principles

## 核心原则

### Concise is key

### 简洁是关键

The [context window](https://platform.claude.com/docs/en/build-with-claude/context-windows) is a public good. Your Skill shares the context window with everything else Claude needs to know, including:

[Context window](https://platform.claude.com/docs/en/build-with-claude/context-windows) 是一种公共物品。你的技能与 Claude 需要知道的所有其他内容共享上下文窗口，包括：

- The system prompt

- 系统提示

- Conversation history

- 对话历史

- Other Skills' metadata

- 其他技能的元数据

- Your actual request

- 你的实际请求

Not every token in your Skill has an immediate cost. At startup, only the metadata (name and description) from all Skills is pre-loaded. Claude reads SKILL.md only when the Skill becomes relevant, and reads additional files only as needed. However, being concise in SKILL.md still matters: once Claude loads it, every token competes with conversation history and other context.

并非你的技能中的每个 token 都有直接成本。启动时，仅预加载所有技能的元数据（名称和描述）。Claude 仅在技能变得相关时才读取 SKILL.md，并仅在需要时读取其他文件。但是，在 SKILL.md 中保持简洁仍然很重要：一旦 Claude 加载它，每个 token 都会与对话历史和其他上下文竞争。

**Default assumption**: Claude is already very smart

**默认假设**：Claude 已经非常聪明了

Only add context Claude doesn't already have. Challenge each piece of information:

仅添加 Claude 尚未拥有的上下文。质疑每一则信息：

- "Does Claude really need this explanation?"

- “Claude 真的需要这个解释吗？”

- "Can I assume Claude knows this?"

- “我可以假设 Claude 知道这个吗？”

- "Does this paragraph justify its token cost?"

- “这一段值得它的 token 成本吗？”

**Good example: Concise** (approximately 50 tokens):

**好的例子：简洁**（大约 50 个 token）：

````markdown theme={null}
## Extract PDF text

## 提取 PDF 文本

Use pdfplumber for text extraction:

使用 pdfplumber 进行文本提取：

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
````

**Bad example: Too verbose** (approximately 150 tokens):

**坏的例子：太啰嗦**（大约 150 个 token）：

```markdown theme={null}
## Extract PDF text

## 提取 PDF 文本

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available for PDF processing, but we
recommend pdfplumber because it's easy to use and handles most cases well.
First, you'll need to install it using pip. Then you can use the code below...

PDF（便携式文档格式）文件是一种包含文本、图像和其他内容的常见文件格式。要从 PDF 中提取文本，你需要使用一个库。有许多可用于 PDF 处理的库，但我们推荐 pdfplumber，因为它易于使用且能很好地处理大多数情况。首先，你需要使用 pip 安装它。然后你可以使用下面的代码......
```

The concise version assumes Claude knows what PDFs are and how libraries work.

简洁版本假设 Claude 知道 PDF 是什么以及库是如何工作的。

### Set appropriate degrees of freedom

### 设置适当的自由度

Match the level of specificity to the task's fragility and variability.

使具体程度与任务的脆弱性和可变性相匹配。

**High freedom** (text-based instructions):

**高自由度**（基于文本的指令）：

Use when:

何时使用：

- Multiple approaches are valid

- 多种方法均有效

- Decisions depend on context

- 决定取决于背景

- Heuristics guide the approach

- 启发式指导方法

Example:

示例：

```markdown theme={null}
## Code review process

## 代码审查流程

1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions

5. 分析代码结构和组织
6. 检查潜在的错误或边缘情况
7. 建议改进可读性和可维护性
8. 验证是否遵守项目约定
```

**Medium freedom** (pseudocode or scripts with parameters):

**中等自由度**（伪代码或带参数的脚本）：

Use when:

何时使用：

- A preferred pattern exists

- 存在首选模式

- Some variation is acceptable

- 一些变化是可以接受的

- Configuration affects behavior

- 配置影响行为

Example:

示例：

````markdown theme={null}
## Generate report

## 生成报告

Use this template and customize as needed:

使用此模板并按需自定义：

```python
def generate_report(data, format="markdown", include_charts=True):
    # Process data
    # Generate output in specified format
    # Optionally include visualizations
```
````

**Low freedom** (specific scripts, few or no parameters):

**低自由度**（特定脚本，很少或没有参数）：

Use when:

何时使用：

- Operations are fragile and error-prone

- 操作脆弱且容易出错

- Consistency is critical

- 一致性至关重要

- A specific sequence must be followed

- 必须遵循特定的顺序

Example:

示例：

````markdown theme={null}
## Database migration

## 数据库迁移

Run exactly this script:

严格运行此脚本：

```bash
python scripts/migrate.py --verify --backup
```

Do not modify the command or add additional flags.

不要修改命令或添加额外的标志。
````

**Analogy**: Think of Claude as a robot exploring a path:

**类比**：把 Claude 想象成一个在探索路径的机器人：

- **Narrow bridge with cliffs on both sides**: There's only one safe way forward. Provide specific guardrails and exact instructions (low freedom). Example: database migrations that must run in exact sequence.

- **两侧悬崖的狭窄桥梁**：只有一条安全的前进道路。提供特定的护栏和确切的指示（低自由度）。示例：必须按确切顺序运行的数据库迁移。

- **Open field with no hazards**: Many paths lead to success. Give general direction and trust Claude to find the best route (high freedom). Example: code reviews where context determines the best approach.

- **没有危险的开阔场地**：许多路径都能通向成功。给出大方向并相信 Claude 会找到最佳路线（高自由度）。示例：上下文决定最佳方法的代码审查。

### Test with all models you plan to use

### 使用你计划使用的所有模型进行测试

Skills act as additions to models, so effectiveness depends on the underlying model. Test your Skill with all the models you plan to use it with.

技能充当模型的补充，因此有效性取决于底层模型。使用你计划使用的所有模型测试你的技能。

**Testing considerations by model**:

**按模型划分的测试注意事项**：

- **Claude Haiku** (fast, economical): Does the Skill provide enough guidance?

- **Claude Haiku**（快速，经济）：技能是否提供了足够的指导？

- **Claude Sonnet** (balanced): Is the Skill clear and efficient?

- **Claude Sonnet**（平衡）：技能是否清晰高效？

- **Claude Opus** (powerful reasoning): Does the Skill avoid over-explaining?

- **Claude Opus**（强大的推理）：技能是否避免了过度解释？

What works perfectly for Opus might need more detail for Haiku. If you plan to use your Skill across multiple models, aim for instructions that work well with all of them.

对 Opus 完美奏效的方法可能需要为 Haiku 提供更多细节。如果你计划在多个模型中使用你的技能，请确指令对它们都有效。

## Skill structure

## 技能结构

<Note>
  **YAML Frontmatter**: The SKILL.md frontmatter supports two fields:

**YAML Frontmatter**：SKILL.md frontmatter 支持两个字段：

- `name` - Human-readable name of the Skill (64 characters maximum)
- `description` - One-line description of what the Skill does and when to use it (1024 characters maximum)

For complete Skill structure details, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#skill-structure).
</Note>

### Naming conventions

### 命名约定

Use consistent naming patterns to make Skills easier to reference and discuss. We recommend using **gerund form** (verb + -ing) for Skill names, as this clearly describes the activity or capability the Skill provides.

使用一致的命名模式，以便更轻松地引用和讨论技能。我们建议使用**动名词形式**（动词 + -ing）作为技能名称，因为这清楚地描述了技能提供的活动或能力。

**Good naming examples (gerund form)**:

**好的命名示例（动名词形式）**：

- "Processing PDFs"
- "Analyzing spreadsheets"
- "Managing databases"
- "Testing code"
- "Writing documentation"

**Acceptable alternatives**:

**可接受的替代方案**：

- Noun phrases: "PDF Processing", "Spreadsheet Analysis"
- Action-oriented: "Process PDFs", "Analyze Spreadsheets"

**Avoid**:

- Vague names: "Helper", "Utils", "Tools"
- Overly generic: "Documents", "Data", "Files"
- Inconsistent patterns within your skill collection

Consistent naming makes it easier to:

一致的命名有助于：

- Reference Skills in documentation and conversations
- Understand what a Skill does at a glance
- Organize and search through multiple Skills
- Maintain a professional, cohesive skill library

### Writing effective descriptions

### 编写有效的描述

The `description` field enables Skill discovery and should include both what the Skill does and when to use it.

`description` 字段支持技能发现，并且应包括技能做什么以及何时使用它。

<Warning>
  **Always write in third person**. The description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems.

**始终使用第三人称编写**。描述被注入到系统提示中，不一致的视角可能会导致发现问题。

- **Good:** "Processes Excel files and generates reports"
- **Avoid:** "I can help you process Excel files"
- **Avoid:** "You can use this to process Excel files"
  </Warning>

**Be specific and include key terms**. Include both what the Skill does and specific triggers/contexts for when to use it.

**具体并包含关键术语**。包括技能做什么以及何时使用它的特定触发器/上下文。

Each Skill has exactly one description field. The description is critical for skill selection: Claude uses it to choose the right Skill from potentially 100+ available Skills. Your description must provide enough detail for Claude to know when to select this Skill, while the rest of SKILL.md provides the implementation details.

每个技能只有一个描述字段。描述对于技能选择至关重要：Claude 使用它从可能 100 多个可用技能中选择正确的技能。你的描述必须提供足够的细节，以便 Claude 知道何时选择此技能，而 SKILL.md 的其余部分则提供实施细节。

Effective examples:

有效示例：

**PDF Processing skill:**

**PDF Processing skill:**

```yaml theme={null}
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**Excel Analysis skill:**

**Excel Analysis skill:**

```yaml theme={null}
description: Analyze Excel spreadsheets, create pivot tables, generate charts. Use when analyzing Excel files, spreadsheets, tabular data, or .xlsx files.
```

**Git Commit Helper skill:**

**Git Commit Helper skill:**

```yaml theme={null}
description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.
```

Avoid vague descriptions like these:

避免像这样的模糊描述：

```yaml theme={null}
description: Helps with documents
```

```yaml theme={null}
description: Processes data
```

```yaml theme={null}
description: Does stuff with files
```

### Progressive disclosure patterns

### 渐进式披露模式

SKILL.md serves as an overview that points Claude to detailed materials as needed, like a table of contents in an onboarding guide. For an explanation of how progressive disclosure works, see [How Skills work](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work) in the overview.

**Practical guidance:**

- Keep SKILL.md body under 500 lines for optimal performance
- Split content into separate files when approaching this limit
- Use the patterns below to organize instructions, code, and resources effectively

#### Visual overview: From simple to complex

#### 视觉概览：从简单到复杂

A basic Skill starts with just a SKILL.md file containing metadata and instructions:

基本的技能仅以包含元数据和指令的 SKILL.md 文件开始：

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=87782ff239b297d9a9e8e1b72ed72db9" alt="Simple SKILL.md file showing YAML frontmatter and markdown body" data-og-width="2048" width="2048" data-og-height="1153" height="1153" data-path="images/agent-skills-simple-file.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=c61cc33b6f5855809907f7fda94cd80e 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=90d2c0c1c76b36e8d485f49e0810dbfd 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=ad17d231ac7b0bea7e5b4d58fb4aeabb 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=f5d0a7a3c668435bb0aee9a3a8f8c329 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0e927c1af9de5799cfe557d12249f6e6 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=46bbb1a51dd4c8202a470ac8c80a893d 2500w" />

As your Skill grows, you can bundle additional content that Claude loads only when needed:

随着你的技能增长，你可以捆绑 Claude 仅在需要时加载的额外内容：

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=a5e0aa41e3d53985a7e3e43668a33ea3" alt="Bundling additional reference files like reference.md and forms.md." data-og-width="2048" width="2048" data-og-height="1327" height="1327" data-path="images/agent-skills-bundling-content.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=f8a0e73783e99b4a643d79eac86b70a2 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=dc510a2a9d3f14359416b706f067904a 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=82cd6286c966303f7dd914c28170e385 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=56f3be36c77e4fe4b523df209a6824c6 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=d22b5161b2075656417d56f41a74f3dd 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=3dd4bdd6850ffcc96c6c45fcb0acd6eb 2500w" />

The complete Skill directory structure might look like this:

完整的技能目录结构可能如下所示：

```
pdf/
├── SKILL.md              # Main instructions (loaded when triggered)
├── FORMS.md              # Form-filling guide (loaded as needed)
├── reference.md          # API reference (loaded as needed)
├── examples.md           # Usage examples (loaded as needed)
└── scripts/
    ├── analyze_form.py   # Utility script (executed, not loaded)
    ├── fill_form.py      # Form filling script
    └── validate.py       # Validation script
```

```
pdf/
├── SKILL.md              # 主要指令（触发时加载）
├── FORMS.md              # 表单填写指南（按需加载）
├── reference.md          # API 参考（按需加载）
├── examples.md           # 使用示例（按需加载）
└── scripts/
    ├── analyze_form.py   # 实用脚本（执行，不加载）
    ├── fill_form.py      # 表单填写脚本
    └── validate.py       # 验证脚本
```

#### Pattern 1: High-level guide with references

#### 模式 1：带有引用的高级指南

````markdown theme={null}
---
name: PDF Processing
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---

## description: 提取 PDF 文件中的文本和表格，填写表单并合并文档。在处理 PDF 文件或用户提到 PDF、表单或文档提取时使用。

# PDF Processing

## Quick start

## 快速开始

Extract text with pdfplumber:

使用 pdfplumber 提取文本：

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## Advanced features

## 高级功能

**Form filling**: See [FORMS.md](FORMS.md) for complete guide
**API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
**Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns

**表单填写**：有关完整指南，请参阅 [FORMS.md](FORMS.md)
**API 参考**：有关所有方法，请参阅 [REFERENCE.md](REFERENCE.md)
**示例**：有关常见模式，请参阅 [EXAMPLES.md](EXAMPLES.md)
````

Claude loads FORMS.md, REFERENCE.md, or EXAMPLES.md only when needed.

Claude 仅在需要时加载 FORMS.md、REFERENCE.md 或 EXAMPLES.md。

#### Pattern 2: Domain-specific organization

#### 模式 2：特定领域的组织

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context. When a user asks about sales metrics, Claude only needs to read sales-related schemas, not finance or marketing data. This keeps token usage low and context focused.

对于具有多个领域的技能，按领域组织内容以避免加载不相关的上下文。当用户询问销售指标时，Claude 只需要阅读与销售相关的架构，而不需要阅读财务或营销数据。这可以保持较低的 token 使用率并使上下文集中。

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

```
bigquery-skill/
├── SKILL.md (概览和导航)
└── reference/
    ├── finance.md (收入，账单指标)
    ├── sales.md (机会，管道)
    ├── product.md (API 使用情况，功能)
    └── marketing.md (活动，归因)
```

````markdown SKILL.md theme={null}
# BigQuery Data Analysis

# BigQuery 数据分析

## Available datasets

## 可用数据集

**Finance**: Revenue, ARR, billing → See [reference/finance.md](reference/finance.md)
**Sales**: Opportunities, pipeline, accounts → See [reference/sales.md](reference/sales.md)
**Product**: API usage, features, adoption → See [reference/product.md](reference/product.md)
**Marketing**: Campaigns, attribution, email → See [reference/marketing.md](reference/marketing.md)

**财务**：收入、ARR、账单 → 见 [reference/finance.md](reference/finance.md)
**销售**：机会、管道、账户 → 见 [reference/sales.md](reference/sales.md)
**产品**：API 使用情况、功能、采用 → 见 [reference/product.md](reference/product.md)
**营销**：活动、归因、电子邮件 → 见 [reference/marketing.md](reference/marketing.md)

## Quick search

## 快速搜索

Find specific metrics using grep:

使用 grep 查找特定指标：

```bash
grep -i "revenue" reference/finance.md
grep -i "pipeline" reference/sales.md
grep -i "api usage" reference/product.md
```
````

#### Pattern 3: Conditional details

#### 模式 3：条件细节

Show basic content, link to advanced content:

显示基本内容，链接到高级内容：

```markdown theme={null}
# DOCX Processing

# DOCX 处理

## Creating documents

## 创建文档

Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

使用 docx-js 创建新文档。请参阅 [DOCX-JS.md](DOCX-JS.md)。

## Editing documents

## 编辑文档

For simple edits, modify the XML directly.

对于简单的编辑，直接修改 XML。

**For tracked changes**: See [REDLINING.md](REDLINING.md)
**For OOXML details**: See [OOXML.md](OOXML.md)

**对于修订更改**：请参阅 [REDLINING.md](REDLINING.md)
**对于 OOXML 详细信息**：请参阅 [OOXML.md](OOXML.md)
```

Claude reads REDLINING.md or OOXML.md only when the user needs those features.

Claude 仅在用户需要这些功能时才读取 REDLINING.md 或 OOXML.md。

### Avoid deeply nested references

### 避免深度嵌套的引用

Claude may partially read files when they're referenced from other referenced files. When encountering nested references, Claude might use commands like `head -100` to preview content rather than reading entire files, resulting in incomplete information.

当文件被其他引用的文件引用时，Claude 可能会部分读取文件。当遇到嵌套的引用时，Claude 可能会使用像 `head -100` 这样的命令来预览内容，而不是读取整个文件，导致信息不完整。

**Keep references one level deep from SKILL.md**. All reference files should link directly from SKILL.md to ensure Claude reads complete files when needed.

**保持引用在 SKILL.md 的一层深度**。所有参考文件都应直接从 SKILL.md 链接，以确保 Claude 在需要时读取完整的文件。

**Bad example: Too deep**:

**坏的例子：太深了**：

```markdown theme={null}
# SKILL.md

See [advanced.md](advanced.md)...

# advanced.md

See [details.md](details.md)...

# details.md

Here's the actual information...
```

**Good example: One level deep**:

**好的例子：一层深度**：

```markdown theme={null}
# SKILL.md

**Basic usage**: [instructions in SKILL.md]
**Advanced features**: See [advanced.md](advanced.md)
**API reference**: See [reference.md](reference.md)
**Examples**: See [examples.md](examples.md)
```

### Structure longer reference files with table of contents

### 使用目录构建更长的参考文件

For reference files longer than 100 lines, include a table of contents at the top. This ensures Claude can see the full scope of available information even when previewing with partial reads.

对于超过 100 行的参考文件，请在顶部包含目录。这确保 Claude 即使在通过部分读取进行预览时也能看到可用信息的完整范围。

**Example**:

**示例**：

```markdown theme={null}
# API Reference

## Contents

- Authentication and setup
- Core methods (create, read, update, delete)
- Advanced features (batch operations, webhooks)
- Error handling patterns
- Code examples

- 身份验证和设置
- 核心方法（创建、读取、更新、删除）
- 高级功能（批处理操作、webhook）
- 错误处理模式
- 代码示例

## Authentication and setup

## 身份验证和设置

...

## Core methods

## 核心方法

...
```

Claude can then read the complete file or jump to specific sections as needed.

然后 Claude 可以读取完整的文件或根据需要跳转到特定部分。

For details on how this filesystem-based architecture enables progressive disclosure, see the [Runtime environment](#runtime-environment) section in the Advanced section below.

有关此基于文件系统的架构如何实现渐进式披露的详细信息，请参阅下面的高级部分中的 [运行时环境](#runtime-environment) 部分。

## Workflows and feedback loops

## 工作流和反馈循环

### Use workflows for complex tasks

### 对复杂任务使用工作流

Break complex operations into clear, sequential steps. For particularly complex workflows, provide a checklist that Claude can copy into its response and check off as it progresses.

将复杂的操作分解为清晰的连续步骤。对于特别复杂的工作流，提供一个清单，Claude 可以将其复制到其响应中并在进行时勾选。

**Example 1: Research synthesis workflow** (for Skills without code):

**示例 1：研究综合工作流**（适用于无代码的技能）：

````markdown theme={null}
## Research synthesis workflow

## 研究综合工作流

Copy this checklist and track your progress:

复制此清单并跟踪您的进度：

```
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations

研究进度：
- [ ] 步骤 1：阅读所有源文档
- [ ] 步骤 2：确定关键主题
- [ ] 步骤 3：交叉引用主张
- [ ] 步骤 4：创建结构化摘要
- [ ] 步骤 5：验证引文
```

**Step 1: Read all source documents**

**步骤 1：阅读所有源文档**

Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

查看 `sources/` 目录中的每个文档。记下主要论点和支持证据。

**Step 2: Identify key themes**

**步骤 2：确定关键主题**

Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

在源之间寻找模式。什么主题反复出现？源在哪里一致或不一致？

**Step 3: Cross-reference claims**

**步骤 3：交叉引用主张**

For each major claim, verify it appears in the source material. Note which source supports each point.

对于每个主要主张，验证它是否出现在源材料中。记下哪个源支持每个点。

**Step 4: Create structured summary**

**步骤 4：创建结构化摘要**

Organize findings by theme. Include:

- Main claim
- Supporting evidence from sources
- Conflicting viewpoints (if any)

- 主要主张
- 来自源的支持证据
- 相互冲突的观点（如果有）

**Step 5: Verify citations**

**步骤 5：验证引文**

Check that every claim references the correct source document. If citations are incomplete, return to Step 3.

检查每个主张是否引用了正确的源文档。如果引文不完整，请返回步骤 3。
````

This example shows how workflows apply to analysis tasks that don't require code. The checklist pattern works for any complex, multi-step process.

此示例展示了工作流如何应用于不需要代码的分析任务。清单模式适用于任何复杂的多步骤过程。

**Example 2: PDF form filling workflow** (for Skills with code):

**示例 2：PDF 表单填写工作流**（适用于有代码的技能）：

````markdown theme={null}
## PDF form filling workflow

## PDF 表单填写工作流

Copy this checklist and check off items as you complete them:

复制此清单并在完成时勾选项目：

```
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)

任务进度：
- [ ] 步骤 1：分析表单（运行 analyze_form.py）
- [ ] 步骤 2：创建字段映射（编辑 fields.json）
- [ ] 步骤 3：验证映射（运行 validate_fields.py）
- [ ] 步骤 4：填写表单（运行 fill_form.py）
- [ ] 步骤 5：验证输出（运行 verify_output.py）
```

**Step 1: Analyze the form**

**步骤 1：分析表单**

Run: `python scripts/analyze_form.py input.pdf`

运行：`python scripts/analyze_form.py input.pdf`

This extracts form fields and their locations, saving to `fields.json`.

这提取了表单字段及其位置，保存到 `fields.json`。

**Step 2: Create field mapping**

**步骤 2：创建字段映射**

Edit `fields.json` to add values for each field.

编辑 `fields.json` 以添加每个字段的值。

**Step 3: Validate mapping**

**步骤 3：验证映射**

Run: `python scripts/validate_fields.py fields.json`

运行：`python scripts/validate_fields.py fields.json`

Fix any validation errors before continuing.

在继续之前修复任何验证错误。

**Step 4: Fill the form**

**步骤 4：填写表单**

Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`

运行：`python scripts/fill_form.py input.pdf fields.json output.pdf`

**Step 5: Verify output**

**步骤 5：验证输出**

Run: `python scripts/verify_output.py output.pdf`

运行：`python scripts/verify_output.py output.pdf`

If verification fails, return to Step 2.

如果验证失败，请返回步骤 2。
````

Clear steps prevent Claude from skipping critical validation. The checklist helps both Claude and you track progress through multi-step workflows.

清晰的步骤可以防止 Claude 跳过关键验证。清单有助于 Claude 和你通过多步工作流程跟踪进度。

### Implement feedback loops

### 实现反馈循环

**Common pattern**: Run validator → fix errors → repeat

**常见模式**：运行验证器 → 修复错误 → 重复

This pattern greatly improves output quality.

这种模式极大地提高了输出质量。

**Example 1: Style guide compliance** (for Skills without code):

**示例 1：风格指南合规性**（适用于无代码的技能）：

```markdown theme={null}
## Content review process

## 内容审查流程

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document

6. 按照 STYLE_GUIDE.md 中的指南起草你的内容
7. 根据清单进行审查：
   - 检查术语一致性
   - 验证示例遵循标准格式
   - 确认所有必需的部分都已存在
8. 如果发现问题：
   - 注明每个问题的具体章节参考
   - 修改内容
   - 再次审查清单
9. 仅在满足所有要求时才继续
10. 定稿并保存文档
```

This shows the validation loop pattern using reference documents instead of scripts. The "validator" is STYLE_GUIDE.md, and Claude performs the check by reading and comparing.

这显示了使用参考文档而不是脚本的验证循环模式。“验证器”是 STYLE_GUIDE.md，Claude 通过阅读和比较来执行检查。

**Example 2: Document editing process** (for Skills with code):

**示例 2：文档编辑过程**（适用于有代码的技能）：

```markdown theme={null}
## Document editing process

## 文档编辑过程

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document

7. 对 `word/document.xml` 进行编辑
8. **立即验证**：`python ooxml/scripts/validate.py unpacked_dir/`
9. 如果验证失败：
   - 仔细查看错误消息
   - 修复 XML 中的问题
   - 再次运行验证
10. **仅在验证通过时继续**
11. 重建：`python ooxml/scripts/pack.py unpacked_dir/ output.docx`
12. 测试输出文档
```

The validation loop catches errors early.

验证循环提早捕获错误。

## Content guidelines

## 内容指南

### Avoid time-sensitive information

### 避免时间敏感信息

Don't include information that will become outdated:

不要包含会过时的信息：

**Bad example: Time-sensitive** (will become wrong):

**坏的例子：时间敏感**（将会变错）：

```markdown theme={null}
If you're doing this before August 2025, use the old API.
After August 2025, use the new API.

如果你在 2025 年 8 月之前这样做，请使用旧 API。
在 2025 年 8 月之后，请使用新 API。
```

**Good example** (use "old patterns" section):

**好的例子**（使用“旧模式”部分）：

```markdown theme={null}
## Current method

## 当前方法

Use the v2 API endpoint: `api.example.com/v2/messages`

使用 v2 API 端点：`api.example.com/v2/messages`

## Old patterns

## 旧模式

<details>
<summary>Legacy v1 API (deprecated 2025-08)</summary>

The v1 API used: `api.example.com/v1/messages`

This endpoint is no longer supported.

<summary>旧版 v1 API（2025-08 弃用）</summary>

v1 API 使用：`api.example.com/v1/messages`

此端点不再受支持。

</details>
```

The old patterns section provides historical context without cluttering the main content.

旧模式部分提供了历史背景，而不会使主要内容混乱。

### Use consistent terminology

### 使用一致的术语

Choose one term and use it throughout the Skill:

选择一个术语并在整个技能中使用它：

**Good - Consistent**:

**好 - 一致**：

- Always "API endpoint"
- Always "field"
- Always "extract"

- 始终使用 "API endpoint"
- 始终使用 "field"
- 始终使用 "extract"

**Bad - Inconsistent**:

**坏 - 不一致**：

- Mix "API endpoint", "URL", "API route", "path"
- Mix "field", "box", "element", "control"
- Mix "extract", "pull", "get", "retrieve"

- 混合使用 "API endpoint", "URL", "API route", "path"
- 混合使用 "field", "box", "element", "control"
- 混合使用 "extract", "pull", "get", "retrieve"

Consistency helps Claude understand and follow instructions.

一致性有助于 Claude 理解并遵循指令。

## Common patterns

## 常见模式

### Template pattern

### 模板模式

Provide templates for output format. Match the level of strictness to your needs.

提供输出格式的模板。使严格程度与你的需求相匹配。

**For strict requirements** (like API responses or data formats):

**对于严格要求**（如 API 响应或数据格式）：

````markdown theme={null}
## Report structure

## 报告结构

ALWAYS use this exact template structure:

始终使用此确切的模板结构：

```markdown
# [Analysis Title]

## Executive summary

## 执行摘要

[One-paragraph overview of key findings]

[关键发现的一段概述]

## Key findings

## 关键发现

- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations

## 建议

1. Specific actionable recommendation
2. Specific actionable recommendation
```
````

**For flexible guidance** (when adaptation is useful):

**对于灵活的指导**（当适应性有用时）：

````markdown theme={null}
## Report structure

## 报告结构

Here is a sensible default format, but use your best judgment based on the analysis:

这是一个合理的默认格式，但请根据分析运用你的最佳判断：

```markdown
# [Analysis Title]

## Executive summary

## 执行摘要

[Overview]

[概览]

## Key findings

## 关键发现

[Adapt sections based on what you discover]

[根据你的发现调整章节]

## Recommendations

## 建议

[Tailor to the specific context]

[根据具体情况进行调整]
```

Adjust sections as needed for the specific analysis type.

根据特定的分析类型按需调整章节。
````

### Examples pattern

### 示例模式

For Skills where output quality depends on seeing examples, provide input/output pairs just like in regular prompting:

对于输出质量取决于查看示例的技能，请像常规提示一样提供输入/输出对：

````markdown theme={null}
## Commit message format

## 提交消息格式

Generate commit messages following these examples:

按照这些示例生成提交消息：

**Example 1:**

**示例 1：**
Input: Added user authentication with JWT tokens
Output:

输入：添加了使用 JWT 令牌的用户身份验证
输出：

```
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
```

**Example 2:**

**示例 2：**
Input: Fixed bug where dates displayed incorrectly in reports
Output:

输入：修复了报告中日期显示不正确的错误
输出：

```
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
```

**Example 3:**

**示例 3：**
Input: Updated dependencies and refactored error handling
Output:

输入：更新了依赖项并重构了错误处理
输出：

```
chore: update dependencies and refactor error handling

- Upgrade lodash to 4.17.21
- Standardize error response format across endpoints
```

Follow this style: type(scope): brief description, then detailed explanation.

遵循这种风格：类型（范围）：简短描述，然后是详细说明。
````

Examples help Claude understand the desired style and level of detail more clearly than descriptions alone.

示例比单纯的描述更能帮助 Claude 清楚地理解所需的风格和细节水平。

### Conditional workflow pattern

### 条件工作流模式

Guide Claude through decision points:

引导 Claude 做出决策：

```markdown theme={null}
## Document modification workflow

## 文档修改工作流

1. Determine the modification type:

1. 确定修改类型：

   **Creating new content?** → Follow "Creation workflow" below
   **Editing existing content?** → Follow "Editing workflow" below

   **创建新内容？** → 遵循下面的“创建工作流”
   **编辑现有内容？** → 遵循下面的“编辑工作流”

1. Creation workflow:

1. 创建工作流：

   - Use docx-js library
   - Build document from scratch
   - Export to .docx format

   - 使用 docx-js 库
   - 从头开始构建文档
   - 导出为 .docx 格式

1. Editing workflow:

1. 编辑工作流：

   - Unpack existing document
   - Modify XML directly
   - Validate after each change
   - Repack when complete

   - 解压现有文档
   - 直接修改 XML
   - 每次更改后验证
   - 完成后重新打包
```

<Tip>
  If workflows become large or complicated with many steps, consider pushing them into separate files and tell Claude to read the appropriate file based on the task at hand.
  如果工作流变得很大或步骤繁琐，请考虑将它们放入单独的文件中，并告诉 Claude 根据手头的任务读取适当的文件。
</Tip>

## Evaluation and iteration

## 评估和迭代

### Build evaluations first

### 首先构建评估

**Create evaluations BEFORE writing extensive documentation.** This ensures your Skill solves real problems rather than documenting imagined ones.

**在编写大量文档之前创建评估。**这将确保你的技能解决实际问题，而不是记录想象中的问题。

**Evaluation-driven development:**

**评估驱动开发：**

1. **Identify gaps**: Run Claude on representative tasks without a Skill. Document specific failures or missing context
2. **Create evaluations**: Build three scenarios that test these gaps
3. **Establish baseline**: Measure Claude's performance without the Skill
4. **Write minimal instructions**: Create just enough content to address the gaps and pass evaluations
5. **Iterate**: Execute evaluations, compare against baseline, and refine

6. **确定差距**：在没有技能的情况下通过代表性任务运行 Claude。记录具体的失败或缺失的上下文
7. **创建评估**：构建三个测试这些差距的场景
8. **建立基线**：在没有技能的情况下测量 Claude 的表现
9. **编写最少指令**：创建足以解决差距并通过评估的内容
10. **迭代**：执行评估，与基线比较，并进行完善

This approach ensures you're solving actual problems rather than anticipating requirements that may never materialize.

这种方法确保你正在解决实际问题，而不是预期可能永远不会实现的需求。

**Evaluation structure**:

**评估结构**：

```json theme={null}
{
  "skills": ["pdf-processing"],
  "query": "Extract all text from this PDF file and save it to output.txt",
  "files": ["test-files/document.pdf"],
  "expected_behavior": [
    "Successfully reads the PDF file using an appropriate PDF processing library or command-line tool",
    "Extracts text content from all pages in the document without missing any pages",
    "Saves the extracted text to a file named output.txt in a clear, readable format"
  ]
}
```

<Note>
  This example demonstrates a data-driven evaluation with a simple testing rubric. We do not currently provide a built-in way to run these evaluations. Users can create their own evaluation system. Evaluations are your source of truth for measuring Skill effectiveness.
  此示例演示了具有简单测试标准的以数据为导向的评估。我们目前不提供运行这些评估的内置方法。用户可以创建自己的评估系统。评估是你衡量技能有效性的事实来源。
</Note>

### Develop Skills iteratively with Claude

### 与 Claude 一起迭代开发技能

The most effective Skill development process involves Claude itself. Work with one instance of Claude ("Claude A") to create a Skill that will be used by other instances ("Claude B"). Claude A helps you design and refine instructions, while Claude B tests them in real tasks. This works because Claude models understand both how to write effective agent instructions and what information agents need.

最有效的技能开发过程涉及 Claude 本身。与一个 Claude 实例（“Claude A”）合作创建一个将由其他实例（“Claude B”）使用的技能。Claude A 帮助你设计和完善指令，而 Claude B 在实际任务中测试它们。之所以有效，是因为 Claude 模型既了解如何编写有效的代理指令，也了解代理需要什么信息。

**Creating a new Skill:**

**创建新技能：**

1. **Complete a task without a Skill**: Work through a problem with Claude A using normal prompting. As you work, you'll naturally provide context, explain preferences, and share procedural knowledge. Notice what information you repeatedly provide.

1. **在没有技能的情况下完成一项任务**：使用常规提示与 Claude A 一起解决问题。当你工作时，你会自然地提供背景、解释偏好并分享程序性知识。注意你反复提供什么信息。

1. **Identify the reusable pattern**: After completing the task, identify what context you provided that would be useful for similar future tasks.

1. **识别可重用模式**：完成任务后，确定你提供的哪些上下文对未来的类似任务有用。

   **Example**: If you worked through a BigQuery analysis, you might have provided table names, field definitions, filtering rules (like "always exclude test accounts"), and common query patterns.

   **示例**：如果你完成了一个 BigQuery 分析，你可能已经提供了表名、字段定义、过滤规则（如“始终排除测试账户”）和常见查询模式。

1. **Ask Claude A to create a Skill**: "Create a Skill that captures this BigQuery analysis pattern we just used. Include the table schemas, naming conventions, and the rule about filtering test accounts."

1. **让 Claude A 创建一个技能**：“创建一个技能来捕获我们刚刚使用的这种 BigQuery 分析模式。包括表模式、命名约定和关于过滤测试账户的规则。”

   <Tip>
     Claude models understand the Skill format and structure natively. You don't need special system prompts or a "writing skills" skill to get Claude to help create Skills. Simply ask Claude to create a Skill and it will generate properly structured SKILL.md content with appropriate frontmatter and body content.
     Claude 模型原生理解技能格式和结构。你不需要特殊的系统提示或“编写技能”技能来让 Claude 帮助创建技能。只需让 Claude 创建一个技能，它就会生成结构合理的 SKILL.md 内容，包括适当的 frontmatter 和正文内容。
   </Tip>

1. **Review for conciseness**: Check that Claude A hasn't added unnecessary explanations. Ask: "Remove the explanation about what win rate means - Claude already knows that."

1. **审查简洁性**：检查 Claude A 是否添加了不必要的解释。问：“删除关于赢率意味着什么的解释 - Claude 已经知道了。”

1. **Improve information architecture**: Ask Claude A to organize the content more effectively. For example: "Organize this so the table schema is in a separate reference file. We might add more tables later."

1. **改进信息架构**：让 Claude A 更有效地组织内容。例如：“组织这个以便表模式在一个单独的参考文件中。我们稍后可能会添加更多表。”

1. **Test on similar tasks**: Use the Skill with Claude B (a fresh instance with the Skill loaded) on related use cases. Observe whether Claude B finds the right information, applies rules correctly, and handles the task successfully.

1. **在类似任务上测试**：在相关用例上使用具有 Claude B（已加载技能的新实例）的技能。观察 Claude B 是否找到正确的信息，正确应用规则，并成功处理任务。

1. **Iterate based on observation**: If Claude B struggles or misses something, return to Claude A with specifics: "When Claude used this Skill, it forgot to filter by date for Q4. Should we add a section about date filtering patterns?"

1. **根据观察进行迭代**：如果 Claude B 挣扎或错过某些内容，请带着细节返回 Claude A：“当 Claude 使用此技能时，它忘记确按 Q4 的日期进行筛选。我们应该添加一个关于日期筛选模式的部分吗？”

**Iterating on existing Skills:**

**在现有技能上迭代：**

The same hierarchical pattern continues when improving Skills. You alternate between:

改进技能时，同样的层次结构模式仍在继续。你在以下各项之间交替：

- **Working with Claude A** (the expert who helps refine the Skill)
- **Testing with Claude B** (the agent using the Skill to perform real work)
- **Observing Claude B's behavior** and bringing insights back to Claude A

- **与 Claude A 一起工作**（帮助完善技能的专家）
- **与 Claude B 一起测试**（使用技能执行实际工作的代理）
- **观察 Claude B 的行为**并将见解带回给 Claude A

1. **Use the Skill in real workflows**: Give Claude B (with the Skill loaded) actual tasks, not test scenarios

1. **在实际工作流中使用技能**：给 Claude B（加载了技能）实际任务，而不是测试场景

1. **Observe Claude B's behavior**: Note where it struggles, succeeds, or makes unexpected choices

1. **观察 Claude B 的行为**：记下它挣扎、成功或做出意外选择的地方

   **Example observation**: "When I asked Claude B for a regional sales report, it wrote the query but forgot to filter out test accounts, even though the Skill mentions this rule."

   **示例观察**：“当我向 Claude B 索要区域销售报告时，它编写了查询，但忘记了过滤掉测试账户，即使技能提到了此规则。”

1. **Return to Claude A for improvements**: Share the current SKILL.md and describe what you observed. Ask: "I noticed Claude B forgot to filter test accounts when I asked for a regional report. The Skill mentions filtering, but maybe it's not prominent enough?"

1. **返回 Claude A 进行改进**：分享当前的 SKILL.md 并描述你观察到的情况。问：“我注意到 Claude B 在我索要区域报告时忘记了过滤测试账户。技能提到了过滤，但也许它不够突出？”

1. **Review Claude A's suggestions**: Claude A might suggest reorganizing to make rules more prominent, using stronger language like "MUST filter" instead of "always filter", or restructuring the workflow section.

1. **审查 Claude A 的建议**：Claude A 可能会建议重组以使规则更突出，使用“必须过滤”而不是“始终过滤”等更强烈的语言，或重组工作流部分。

1. **Apply and test changes**: Update the Skill with Claude A's refinements, then test again with Claude B on similar requests

1. **应用和测试更改**：用 Claude A 的改进更新技能，然后在类似请求上再次用 Claude B 测试

1. **Repeat based on usage**: Continue this observe-refine-test cycle as you encounter new scenarios. Each iteration improves the Skill based on real agent behavior, not assumptions.

1. **根据使用情况重复**：当你遇到新场景时，继续这个观察-完善-测试循环。每次迭代都会根据实际代理行为（而不是假设）改进技能。

**Gathering team feedback:**

**收集团队反馈：**

1. Share Skills with teammates and observe their usage
2. Ask: Does the Skill activate when expected? Are instructions clear? What's missing?
3. Incorporate feedback to address blind spots in your own usage patterns

4. 与队友分享技能并观察他们的使用情况
5. 问：技能是否在预期时激活？指令是否清晰？缺少什么？
6. 结合反馈以解决你自己的使用模式中的盲点

**Why this approach works**: Claude A understands agent needs, you provide domain expertise, Claude B reveals gaps through real usage, and iterative refinement improves Skills based on observed behavior rather than assumptions.

**为什么这种方法有效**：Claude A 了解代理需求，你提供领域专业知识，Claude B 通过实际使用揭示差距，并且迭代完善改进基于观察到的行为而不是假设的技能。

### Observe how Claude navigates Skills

### 观察 Claude 如何导航技能

As you iterate on Skills, pay attention to how Claude actually uses them in practice. Watch for:

当你迭代技能时，请注意 Claude 在实践中实际如何使用它们。注意：

- **Unexpected exploration paths**: Does Claude read files in an order you didn't anticipate? This might indicate your structure isn't as intuitive as you thought
- **Missed connections**: Does Claude fail to follow references to important files? Your links might need to be more explicit or prominent
- **Overreliance on certain sections**: If Claude repeatedly reads the same file, consider whether that content should be in the main SKILL.md instead
- **Ignored content**: If Claude never accesses a bundled file, it might be unnecessary or poorly signaled in the main instructions

- **意外的探索路径**：Claude 是否按你没想到的顺序读取文件？这可能表明你的结构不如你想象的直观
- **错过的连接**：Claude 是否未能遵循对重要文件的引用？你的链接可能需要更明确或更突出
- **过度依赖某些部分**：如果 Claude 反复读取通过一个文件，请考虑该内容是否应在主 SKILL.md 中
- **忽略的内容**：如果 Claude 从未访问捆绑文件，则可能不需要它，或者在主指令中信号不佳

Iterate based on these observations rather than assumptions. The 'name' and 'description' in your Skill's metadata are particularly critical. Claude uses these when deciding whether to trigger the Skill in response to the current task. Make sure they clearly describe what the Skill does and when it should be used.

根据这些观察进行迭代，而不是假设。你的技能元数据中的 'name' 和 'description' 特别关键。Claude 在决定是否响应当前任务触发技能时使用这些。确保它们清楚地描述了技能的作用以及何时应该使用它。

## Anti-patterns to avoid

## 要避免的反模式

### Avoid Windows-style paths

### 避免 Windows 风格的路径

Always use forward slashes in file paths, even on Windows:

在文件路径中始终使用正斜杠，即使在 Windows 上也是如此：

- ✓ **Good**: `scripts/helper.py`, `reference/guide.md`
- ✗ **Avoid**: `scripts\helper.py`, `reference\guide.md`

- ✓ **Good**: `scripts/helper.py`, `reference/guide.md`
- ✗ **Avoid**: `scripts\helper.py`, `reference\guide.md`

Unix-style paths work across all platforms, while Windows-style paths cause errors on Unix systems.

Unix 风格的路径在所有平台上都能工作，而 Windows 风格的路径在 Unix 系统上会导致错误。

### Avoid offering too many options

### 避免提供太多选项

Don't present multiple approaches unless necessary:

除非必要，否则不要提出多种方法：

````markdown theme={null}
**Bad example: Too many choices** (confusing):
"You can use pypdf, or pdfplumber, or PyMuPDF, or pdf2image, or..."

**坏的例子：太多选择**（令人困惑）：
"你可以使用 pypdf，或 pdfplumber，或 PyMuPDF，或 pdf2image，或 ……"

**Good example: Provide a default** (with escape hatch):
"Use pdfplumber for text extraction:

**好的例子：提供默认值**（带有逃生舱）：
"使用 pdfplumber 进行文本提取：

```python
import pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead."

对于需要 OCR 的扫描 PDF，请改用带有 pytesseract 的 pdf2image。"
````

## Advanced: Skills with executable code

## 高级：带有可执行代码的技能

The sections below focus on Skills that include executable scripts. If your Skill uses only markdown instructions, skip to [Checklist for effective Skills](#checklist-for-effective-skills).

下面的部分重点介绍包含可执行脚本的技能。如果你的技能仅使用 markdown 指令，请跳至 [有效技能清单](#checklist-for-effective-skills)。

### Solve, don't punt

### 解决，不要踢皮球

When writing scripts for Skills, handle error conditions rather than punting to Claude.

为技能编写脚本时，处理错误情况，而不是将其踢给 Claude。

**Good example: Handle errors explicitly**:

**好的例子：显式处理错误**：

```python theme={null}
def process_file(path):
    """Process a file, creating it if it doesn't exist."""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        # Create file with default content instead of failing
        print(f"File {path} not found, creating default")
        with open(path, 'w') as f:
            f.write('')
        return ''
    except PermissionError:
        # Provide alternative instead of failing
        print(f"Cannot access {path}, using default")
        return ''
```

**Bad example: Punt to Claude**:

**坏的例子：踢给 Claude**：

```python theme={null}
def process_file(path):
    # Just fail and let Claude figure it out
    return open(path).read()
```

Configuration parameters should also be justified and documented to avoid "voodoo constants" (Ousterhout's law). If you don't know the right value, how will Claude determine it?

配置参数也应合理且有据可查，以避免“巫毒常量”（Ousterhout 定律）。如果你不知道正确的值，Claude 将如何确定它？

**Good example: Self-documenting**:

**好的例子：自文档化**：

```python theme={null}
# HTTP requests typically complete within 30 seconds
# Longer timeout accounts for slow connections
REQUEST_TIMEOUT = 30

# Three retries balances reliability vs speed
# Most intermittent failures resolve by the second retry
MAX_RETRIES = 3
```

**Bad example: Magic numbers**:

**坏的例子：魔术数字**：

```python theme={null}
TIMEOUT = 47  # Why 47?
RETRIES = 5   # Why 5?
```

### Provide utility scripts

### 提供实用脚本

Even if Claude could write a script, pre-made scripts offer advantages:

即使 Claude 可以编写脚本，预制脚本也具有优势：

**Benefits of utility scripts**:

**实用脚本的好处**：

- More reliable than generated code
- Save tokens (no need to include code in context)
- Save time (no code generation required)
- Ensure consistency across uses

- 比生成的代码更可靠
- 节省 token（无需在上下文中包含代码）
- 节省时间（无需生成代码）
- 确保使用的一致性

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=4bbc45f2c2e0bee9f2f0d5da669bad00" alt="Bundling executable scripts alongside instruction files" data-og-width="2048" width="2048" data-og-height="1154" height="1154" data-path="images/agent-skills-executable-scripts.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=9a04e6535a8467bfeea492e517de389f 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=e49333ad90141af17c0d7651cca7216b 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=954265a5df52223d6572b6214168c428 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=2ff7a2d8f2a83ee8af132b29f10150fd 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=48ab96245e04077f4d15e9170e081cfb 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0301a6c8b3ee879497cc5b5483177c90 2500w" />

The diagram above shows how executable scripts work alongside instruction files. The instruction file (forms.md) references the script, and Claude can execute it without loading its contents into context.

上图显示了可执行脚本如何与指令文件一起工作。指令文件 (forms.md) 引用脚本，Claude 可以在不将其内容加载到上下文中的情况下执行它。

**Important distinction**: Make clear in your instructions whether Claude should:

**重要区别**：在你的指令中明确 Claude 是否应该：

- **Execute the script** (most common): "Run `analyze_form.py` to extract fields"
- **Read it as reference** (for complex logic): "See `analyze_form.py` for the field extraction algorithm"

- **执行脚本**（最常见）：“运行 `analyze_form.py` 以提取字段”
- **作为参考阅读**（对于复杂逻辑）：“有关字段提取算法，请参阅 `analyze_form.py`”

For most utility scripts, execution is preferred because it's more reliable and efficient. See the [Runtime environment](#runtime-environment) section below for details on how script execution works.

对于大多数实用脚本，首选执行，因为它更可靠、更高效。有关脚本执行如何工作的详细信息，请参阅下面的 [运行时环境](#runtime-environment) 部分。

**Example**:

**示例**：

````markdown theme={null}
## Utility scripts

## 实用脚本

## 实用脚本

**analyze_form.py**: Extract all form fields from PDF

**analyze_form.py**：从 PDF 中提取所有表单字段

```bash
python scripts/analyze_form.py input.pdf > fields.json
```

Output format:

输出格式：

```json
{
  "field_name": { "type": "text", "x": 100, "y": 200 },
  "signature": { "type": "sig", "x": 150, "y": 500 }
}
```

**validate_boxes.py**: Check for overlapping bounding boxes

**validate_boxes.py**：检查重叠的边界框

```bash
python scripts/validate_boxes.py fields.json
# Returns: "OK" or lists conflicts
```

**fill_form.py**: Apply field values to PDF

**fill_form.py**：将字段值应用于 PDF

```bash
python scripts/fill_form.py input.pdf fields.json output.pdf
```
````

### Use visual analysis

### 使用视觉分析

When inputs can be rendered as images, have Claude analyze them:

当输入可以呈现为图像时，让 Claude 分析它们：

````markdown theme={null}
## Form layout analysis

## 表单布局分析

1. Convert PDF to images:

1. 将 PDF 转换为图像：

   ```bash
   python scripts/pdf_to_images.py form.pdf
   ```

1. Analyze each page image to identify form fields
1. Claude can see field locations and types visually

1. 分析每个页面图像以识别表单字段
1. Claude 可以直观地看到字段位置和类型
````

<Note>
  In this example, you'd need to write the `pdf_to_images.py` script.
  在此示例中，你需要编写 `pdf_to_images.py` 脚本。
</Note>

Claude's vision capabilities help understand layouts and structures.

Claude 的视觉能力有助于理解布局和结构。

### Create verifiable intermediate outputs

### 创建可验证的中间输出

When Claude performs complex, open-ended tasks, it can make mistakes. The "plan-validate-execute" pattern catches errors early by having Claude first create a plan in a structured format, then validate that plan with a script before executing it.

当 Claude 执行复杂的开放式任务时，它可能会犯错误。“计划-验证-执行”模式通过让 Claude 首先以结构化格式创建计划，然后在执行之前使用脚本验证该计划，从而尽早发现错误。

**Example**: Imagine asking Claude to update 50 form fields in a PDF based on a spreadsheet. Without validation, Claude might reference non-existent fields, create conflicting values, miss required fields, or apply updates incorrectly.

**示例**：想象一下要求 Claude 根据电子表格更新 PDF 中的 50 个表单字段。如果没有验证，Claude 可能会引用不存在的字段，创建冲突的值，遗漏必填字段，或错误地应用更新。

**Solution**: Use the workflow pattern shown above (PDF form filling), but add an intermediate `changes.json` file that gets validated before applying changes. The workflow becomes: analyze → **create plan file** → **validate plan** → execute → verify.

**解决方案**：使用上面显示的流程模式（PDF 表单填写），但添加一个在应用更改之前进行验证的中间 `changes.json` 文件。工作流变为：分析 → **创建计划文件** → **验证计划** → 执行 → 验证。

**Why this pattern works:**

**为什么这种模式有效：**

- **Catches errors early**: Validation finds problems before changes are applied
- **Machine-verifiable**: Scripts provide objective verification
- **Reversible planning**: Claude can iterate on the plan without touching originals
- **Clear debugging**: Error messages point to specific problems

- **及早发现错误**：验证在应用更改之前发现问题
- **机器可验证**：脚本提供客观验证
- **可逆计划**：Claude 可以在不触及原件的情况下迭代计划
- **清晰调试**：错误消息指向具体问题

**When to use**: Batch operations, destructive changes, complex validation rules, high-stakes operations.

**何时使用**：批处理操作，破坏性更改，复杂验证规则，高风险操作。

**Implementation tip**: Make validation scripts verbose with specific error messages like "Field 'signature_date' not found. Available fields: customer_name, order_total, signature_date_signed" to help Claude fix issues.

**实施提示**：使验证脚本详细说明具体的错误消息，如“未找到字段 'signature_date'。可用字段：customer_name, order_total, signature_date_signed”，以帮助 Claude 修复问题。

### Package dependencies

### 包依赖项

Skills run in the code execution environment with platform-specific limitations:

技能在具有特定于平台的限制的代码执行环境中运行：

- **claude.ai**: Can install packages from npm and PyPI and pull from GitHub repositories
- **Anthropic API**: Has no network access and no runtime package installation

- **claude.ai**：可以从 npm 和 PyPI 安装包，并从 GitHub 存储库拉取
- **Anthropic API**：没有网络访问权限，也没有运行时包安装

List required packages in your SKILL.md and verify they're available in the [code execution tool documentation](/en/docs/agents-and-tools/tool-use/code-execution-tool).

在你的 SKILL.md 中列出所需的包，并确认它们在 [代码执行工具文档](/en/docs/agents-and-tools/tool-use/code-execution-tool) 中可用。

### Runtime environment

### 运行时环境

Skills run in a code execution environment with filesystem access, bash commands, and code execution capabilities. For the conceptual explanation of this architecture, see [The Skills architecture](/en/docs/agents-and-tools/agent-skills/overview#the-skills-architecture) in the overview.

技能在具有文件系统访问权限、bash 命令和代码执行能力的代码执行环境中运行。有关此架构的概念说明，请参阅概述中的 [技能架构](/en/docs/agents-and-tools/agent-skills/overview#the-skills-architecture)。

**How this affects your authoring:**

**这如何影响你的创作：**

**How Claude accesses Skills:**

**Claude 如何访问技能：**

1. **Metadata pre-loaded**: At startup, the name and description from all Skills' YAML frontmatter are loaded into the system prompt
2. **Files read on-demand**: Claude uses bash Read tools to access SKILL.md and other files from the filesystem when needed
3. **Scripts executed efficiently**: Utility scripts can be executed via bash without loading their full contents into context. Only the script's output consumes tokens
4. **No context penalty for large files**: Reference files, data, or documentation don't consume context tokens until actually read

5. **预加载元数据**：启动时，所有技能的 YAML frontmatter 中的名称和描述都会加载到系统提示中
6. **按需读取文件**：Claude 在需要时使用 bash 读取工具从文件系统访问 SKILL.md 和其他文件
7. **高效执行脚本**：实用脚本可以通过 bash 执行，而无需将其全部内容加载到上下文中。只有脚本的输出消耗 token
8. **大文件没有上下文惩罚**：参考文件、数据或文档在实际读取之前不消耗上下文 token

- **File paths matter**: Claude navigates your skill directory like a filesystem. Use forward slashes (`reference/guide.md`), not backslashes
- **Name files descriptively**: Use names that indicate content: `form_validation_rules.md`, not `doc2.md`
- **Organize for discovery**: Structure directories by domain or feature
  - Good: `reference/finance.md`, `reference/sales.md`
  - Bad: `docs/file1.md`, `docs/file2.md`
- **Bundle comprehensive resources**: Include complete API docs, extensive examples, large datasets; no context penalty until accessed
- **Prefer scripts for deterministic operations**: Write `validate_form.py` rather than asking Claude to generate validation code
- **Make execution intent clear**:
  - "Run `analyze_form.py` to extract fields" (execute)
  - "See `analyze_form.py` for the extraction algorithm" (read as reference)
- **Test file access patterns**: Verify Claude can navigate your directory structure by testing with real requests

- **文件路径很重要**：Claude 像文件系统一样浏览你的技能目录。使用正斜杠（`reference/guide.md`），而不是反斜杠
- **描述性地命名文件**：使用指示内容的名称：`form_validation_rules.md`，而不是 `doc2.md`
- **为发现而组织**：按域或功能构建目录
  - 好：`reference/finance.md`, `reference/sales.md`
  - 坏：`docs/file1.md`, `docs/file2.md`
- **捆绑综合资源**：包括完整的 API 文档、大量示例、大数据集；访问前无上下文惩罚
- **优先使用脚本进行确定性操作**：编写 `validate_form.py` 而不是让 Claude 生成验证代码
- **明确执行意图**：
  - “运行 `analyze_form.py` 以提取字段”（执行）
  - “有关提取算法，请参阅 `analyze_form.py`”（作为参考阅读）
- **测试文件访问模式**：通过使用真实请求进行测试，验证 Claude 是否可以浏览你的目录结构

**Example:**

**示例：**

```
bigquery-skill/
├── SKILL.md (overview, points to reference files)
└── reference/
    ├── finance.md (revenue metrics)
    ├── sales.md (pipeline data)
    └── product.md (usage analytics)
```

When the user asks about revenue, Claude reads SKILL.md, sees the reference to `reference/finance.md`, and invokes bash to read just that file. The sales.md and product.md files remain on the filesystem, consuming zero context tokens until needed. This filesystem-based model is what enables progressive disclosure. Claude can navigate and selectively load exactly what each task requires.

当用户询问收入时，Claude 读取 SKILL.md，看到对 `reference/finance.md` 的引用，并调用 bash 仅读取该文件。sales.md 和 product.md 文件保留在文件系统中，直到需要时才消耗上下文 token。这种基于文件系统的模型是实现渐进式披露的原因。Claude 可以导航并有选择地加载每个任务所需的内容。

For complete details on the technical architecture, see [How Skills work](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work) in the Skills overview.

有关技术架构的完整详细信息，请参阅技能概述中的 [技能如何工作](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work)。

### MCP tool references

### MCP 工具参考

If your Skill uses MCP (Model Context Protocol) tools, always use fully qualified tool names to avoid "tool not found" errors.

如果你的技能使用 MCP（模型上下文协议）工具，请始终使用完全限定的工具名称以避免“未找到工具”错误。

**Format**: `ServerName:tool_name`

**格式**：`ServerName:tool_name`

**Example**:

**示例**：

```markdown theme={null}
Use the BigQuery:bigquery_schema tool to retrieve table schemas.
Use the GitHub:create_issue tool to create issues.
```

Where:

其中：

- `BigQuery` and `GitHub` are MCP server names
- `bigquery_schema` and `create_issue` are the tool names within those servers

- `BigQuery` 和 `GitHub` 是 MCP 服务器名称
- `bigquery_schema` 和 `create_issue` 是这些服务器中的工具名称

Without the server prefix, Claude may fail to locate the tool, especially when multiple MCP servers are available.

如果没有服务器前缀，Claude 可能无法定位工具，尤其是在有多个 MCP 服务器可用时。

### Avoid assuming tools are installed

### 避免假设已安装工具

Don't assume packages are available:

不要假设包可用：

`````markdown theme={null}
**Bad example: Assumes installation**:
"Use the pdf library to process the file."

**坏的例子：假设已安装**：
"使用 pdf 库处理文件。"

**Good example: Explicit about dependencies**:
"Install required package: `pip install pypdf`

**好的例子：明确依赖关系**：
"安装所需的包：`pip install pypdf`

Then use it:

然后使用它：

````python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
```"
````
`````

```

## Technical notes

## 技术说明

### YAML frontmatter requirements

### YAML frontmatter 要求

The SKILL.md frontmatter includes only `name` (64 characters max) and `description` (1024 characters max) fields. See the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#skill-structure) for complete structure details.

SKILL.md frontmatter 仅包含 `name`（最多 64 个字符）和 `description`（最多 1024 个字符）字段。有关完整的结构详细信息，请参阅 [技能概述](/en/docs/agents-and-tools/agent-skills/overview#skill-structure)。

### Token budgets

### Token 预算

Keep SKILL.md body under 500 lines for optimal performance. If your content exceeds this, split it into separate files using the progressive disclosure patterns described earlier. For architectural details, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work).

将 SKILL.md 正文保持在 500 行以下以获得最佳性能。如果你的内容超过此值，请使用前面描述的渐进式披露模式将其拆分为单独的文件。有关架构详细信息，请参阅 [技能概述](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work)。

## Checklist for effective Skills

## 有效技能清单

Before sharing a Skill, verify:

在分享技能之前，请验证：

### Core quality

### 核心质量

- [ ] Description is specific and includes key terms
- [ ] Description includes both what the Skill does and when to use it
- [ ] SKILL.md body is under 500 lines
- [ ] Additional details are in separate files (if needed)
- [ ] No time-sensitive information (or in "old patterns" section)
- [ ] Consistent terminology throughout
- [ ] Examples are concrete, not abstract
- [ ] File references are one level deep
- [ ] Progressive disclosure used appropriately
- [ ] Workflows have clear steps

- [ ] 描述具体且包含关键术语
- [ ] 描述包括技能的作用以及何时使用它
- [ ] SKILL.md 正文在 500 行以下
- [ ] 更多细节在单独的文件中（如果需要）
- [ ] 没有时间敏感信息（或在“旧模式”部分）
- [ ] 全文术语一致
- [ ] 示例具体，不抽象
- [ ] 文件引用为一层深度
- [ ] 适当地使用了渐进式披露
- [ ] 工作流有清晰的步骤

### Code and scripts

### 代码和脚本

- [ ] Scripts solve problems rather than punt to Claude
- [ ] Error handling is explicit and helpful
- [ ] No "voodoo constants" (all values justified)
- [ ] Required packages listed in instructions and verified as available
- [ ] Scripts have clear documentation
- [ ] No Windows-style paths (all forward slashes)
- [ ] Validation/verification steps for critical operations
- [ ] Feedback loops included for quality-critical tasks

- [ ] 脚本解决问题而不是推给 Claude
- [ ] 错误处理明确且有帮助
- [ ] 没有“巫毒常量”（所有值都有正当理由）
- [ ] 所需包在指令中列出并已验证可用
- [ ] 脚本有清晰的文档
- [ ] 没有 Windows 风格的路径（所有正斜杠）
- [ ] 关键操作的验证/确认步骤
- [ ] 包含质量关键任务的反馈循环

### Testing

### 测试

- [ ] At least three evaluations created
- [ ] Tested with Haiku, Sonnet, and Opus
- [ ] Tested with real usage scenarios
- [ ] Team feedback incorporated (if applicable)

- [ ] 至少创建了三个评估
- [ ] 使用 Haiku, Sonnet, and Opus 进行了测试
- [ ] 使用真实使用场景进行了测试
- [ ] 纳入了团队反馈（如适用）

## Next steps

## 下一步

<CardGroup cols={2}>
  <Card title="Get started with Agent Skills" icon="rocket" href="/en/docs/agents-and-tools/agent-skills/quickstart">
    Create your first Skill
    创建你的第一个技能
  </Card>

  <Card title="Use Skills in Claude Code" icon="terminal" href="/en/docs/claude-code/skills">
    Create and manage Skills in Claude Code
    在 Claude Code 中创建和管理技能
  </Card>

  <Card title="Use Skills with the API" icon="code" href="/en/api/skills-guide">
    Upload and use Skills programmatically
    以编程方式上传和使用技能
  </Card>
</CardGroup>
```
