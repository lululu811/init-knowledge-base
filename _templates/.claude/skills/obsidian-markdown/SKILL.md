---
name: obsidian-markdown
description: 创建和编辑符合 Obsidian 规范的 Markdown 文件。涵盖 Wikilinks、Embeds、Callouts、Properties（Frontmatter）、Tags、Comments 等 Obsidian 特有语法。在 ingest 创建 wiki 页面、query 引用来源、lint 修复内容时，必须使用本规范确保语法正确。
user-invocable: false
---

# Obsidian Markdown 规范

本 Skill 定义了知识库中所有 Markdown 文件必须遵循的 Obsidian Flavored Markdown 语法标准。标准 Markdown（标题、列表、粗体、代码块等） assumed knowledge，此处仅覆盖 Obsidian 特有扩展。

---

## 工作流：创建 Wiki 页面

1. **添加 frontmatter**（YAML Properties）在文件最顶部
2. **编写正文**使用标准 Markdown + 本规范中的 Obsidian 特有语法
3. **链接相关页面**使用 wikilinks `[[页面名]]` 建立内部连接
4. **嵌入内容**使用 `![[embed]]` 引用图片、PDF 或其他笔记片段
5. **使用 callouts**高亮关键信息
6. **验证**页面在 Obsidian 阅读视图中渲染正确

> **链接选择原则**：Vault 内部页面一律使用 `[[wikilinks]]`（Obsidian 会自动追踪重命名），外部 URL 才使用标准 Markdown 链接 `[text](url)`。

---

## 1. 内部链接（Wikilinks）

```markdown
[[页面名称]]                    → 基础链接
[[页面名称|显示文本]]           → 自定义显示文本
[[页面名称#标题]]               → 链接到目标页面的某个标题
[[页面名称#^块ID]]              → 链接到目标页面的某个段落（块）
[[#本文标题]]                   → 同一页面内的标题跳转
```

### 块 ID（Block ID）

在任意段落末尾追加 `^块ID` 即可创建可链接的块：

```markdown
这是一个可以被精确链接的段落。 ^my-block-id
```

对于列表和引用块，块 ID 放在块后面的独立行：

```markdown
> 这是一个引用块

^quote-id
```

引用时：
```markdown
[[某页面#^quote-id]]
```

---

## 2. 嵌入（Embeds）

在 wikilink 前加 `!` 即可嵌入内容：

```markdown
![[页面名称]]                  → 嵌入整页内容
![[页面名称#标题]]             → 嵌入某个标题下的内容
![[页面名称#^块ID]]            → 嵌入某个段落
![[图片.png]]                  → 嵌入图片
![[图片.png|300]]              → 嵌入图片并限制宽度为 300px
![[图片.png|640x480]]          → 嵌入图片并指定宽高
![[文档.pdf#page=3]]           → 嵌入 PDF 的第 3 页
![[文档.pdf#height=400]]       → 嵌入 PDF 并限制高度
![[音频.mp3]]                  → 嵌入音频播放器
```

外部图片：
```markdown
![Alt文本](https://example.com/image.png)
![Alt文本|300](https://example.com/image.png)
```

---

## 3. Callouts

```markdown
> [!note]
> 基础笔记 callout

> [!warning] 自定义标题
> 带自定义标题的警告 callout

> [!faq]- 默认折叠
> 默认折叠的 callout（- 折叠，+ 展开）

> [!tip]+
> 默认展开但可折叠的 callout
```

### 支持的 Callout 类型

| 类型 | 别名 | 语义 |
|------|------|------|
| `note` | — | 一般笔记 |
| `abstract` | `summary`, `tldr` | 摘要/总结 |
| `info` | — | 信息 |
| `todo` | — | 待办事项 |
| `tip` | `hint`, `important` | 提示/重要 |
| `success` | `check`, `done` | 成功/完成 |
| `question` | `help`, `faq` | 问题/帮助 |
| `warning` | `caution`, `attention` | 警告 |
| `failure` | `fail`, `missing` | 失败 |
| `danger` | `error` | 危险/错误 |
| `bug` | — | Bug |
| `example` | — | 示例 |
| `quote` | `cite` | 引用 |

### 嵌套 Callouts

```markdown
> [!question] 外层问题
> > [!note] 内层笔记
> > 嵌套内容
```

---

## 4. Properties（Frontmatter）

所有 wiki 页面必须以 YAML frontmatter 开头：

```yaml
---
title: "页面标题"
type: concept | entity | source | synthesis | moc
aliases: []
tags: []
sources: []
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
status: draft | finished | archived
---
```

### Property 类型规范

| 类型 | 示例 | 说明 |
|------|------|------|
| 文本 | `title: 页面标题` | 单行字符串 |
| 数字 | `complexity: 3` | 整数或小数 |
| 布尔 | `completed: false` | true / false |
| 日期 | `created: 2026-06-04` | ISO 日期格式 |
| 列表 | `tags: [ai, llm]` | YAML 数组 |
| 链接 | `sources: ["[[SourceName]]"]` | 可包含 wikilinks |

### 默认 Properties

- `tags` — 标签，用于搜索和图谱视图分组
- `aliases` — 别名，链接建议时会匹配
- `cssclasses` — 应用到当前页面的 CSS 类（高级用法）

---

## 5. 标签（Tags）

```markdown
#标签              → 行内标签
#嵌套/标签         → 层级标签
#tag-with-dashes   → 带连字符的标签
```

标签规则：可包含字母、数字（不能开头）、下划线 `_`、连字符 `-`、斜杠 `/`（用于嵌套）。

在 frontmatter 中定义：
```yaml
---
tags:
  - ai
  - nested/tag
---
```

---

## 6. 注释（Comments）

用于在源代码中隐藏 AI 批注或内部标记，阅读视图中不可见：

```markdown
这是可见文本 %%但这是隐藏的批注%%。

%%
整段都被隐藏。
可用于记录 AI 的处理逻辑或待办标记，
不影响阅读体验。
%%
```

---

## 7. 高亮（Highlights）

```markdown
==高亮文本==        → 阅读视图中以高亮背景显示
```

---

## 8. Mermaid 图表

Obsidian 原生支持 Mermaid 图表，在代码块中使用 `mermaid` 语言标记即可渲染。无需安装插件。

### 流程图（flowchart）

适合展示工作流、数据流、决策过程：

```markdown
```mermaid
flowchart LR
    A[原始资料] --> B[ingest 编译]
    B --> C[wiki 知识库]
    C --> D[query 查询]
```
```

### 思维导图（mindmap）

适合展示层级关系、知识体系：

```markdown
```mermaid
mindmap
  root((主题))
    分支 A
      子节点 A1
      子节点 A2
    分支 B
      子节点 B1
```
```

### 时间线（timeline）

适合展示演进历史、发展脉络：

```markdown
```mermaid
timeline
    title 发展历程
    section 起源
        2017 : 概念提出
    section 发展
        2020 : 重大突破
    section 现状
        2024 : 当前状态
```
```

### 时序图（sequenceDiagram）

适合展示交互流程、系统调用：

```markdown
```mermaid
sequenceDiagram
    participant U as 用户
    participant A as Agent
    U->>A: /ingest path
    A->>A: 编译知识
    A-->>U: 完成报告
```
```

### 使用建议

| 场景 | 推荐图表类型 |
|------|-------------|
| 知识体系/领域分类 | `mindmap` |
| 工作流/数据流 | `flowchart LR` 或 `flowchart TD` |
| 概念演进/历史 | `timeline` |
| 交互流程 | `sequenceDiagram` |
| 状态转换 | `stateDiagram-v2` |
| 类关系 | `classDiagram` |

> Agent 在创建 wiki 页面时，应在以下场景主动使用 Mermaid 图表：
> - 概念页面的「演进脉络」→ `timeline`
> - 综合分析页面的「证据关系」→ `flowchart`
> - MOC 页面的领域分类 → `mindmap`

---

## 9. 完整示例

```markdown
---
title: "Transformer 架构"
type: concept
aliases: ["Attention Is All You Need"]
tags: [ai, nlp, architecture]
sources: ["raw/02-papers/attention-is-all-you-need.md"]
created: 2026-06-04
last_updated: 2026-06-04
status: draft
---

# Transformer 架构

Transformer 是一种完全基于 [[注意力机制]] 的深度学习架构，彻底改变了 NLP 领域。

> [!important] 核心贡献
> 用自注意力（Self-Attention）取代了 RNN 和 CNN，实现了并行计算。

## 核心组件

1. **编码器（Encoder）**：读取输入序列
2. **解码器（Decoder）**：生成输出序列
3. **自注意力层**：计算序列中每个位置与其他位置的关系

## 关键公式

注意力计算：
$$Attention(Q, K, V) = softmax(\frac{QK^T}{\sqrt{d_k}})V$$

## 关联连接

- [[Attention Mechanism]] — 核心机制详解
- [[BERT]] — 仅使用编码器的变体
- [[GPT]] — 仅使用解码器的变体

## 来源

核心论文：[[attention-is-all-you-need]]
```

---

## 参考

- [Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown)
- [Internal links](https://help.obsidian.md/links)
- [Embed files](https://help.obsidian.md/embeds)
- [Callouts](https://help.obsidian.md/callouts)
- [Properties](https://help.obsidian.md/properties)
