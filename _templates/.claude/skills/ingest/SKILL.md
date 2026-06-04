---
name: ingest
description: 将 raw/ 目录下的原始资料编译到 wiki/ 中。处理完成后将源文件移动到 raw/09-archive/ 归档。支持 /ingest（扫描所有未归档文件）或 /ingest <path>（处理指定文件）。当用户提到"摄取"、"导入"、"收入"资料时触发。绝对忽略 raw/09-archive/ 目录。
user-invocable: true
---

# ingest 技能

## 核心工作流：Inbox & Archive

你正在维护一个 **LLM Wiki**（Obsidian 知识库）。`raw/` 目录是"待处理收件箱"，`wiki/` 是"编译输出层"。

**目录结构约定：**
- `raw/01-articles/` — 网页剪藏的 Markdown 文章
- `raw/02-papers/` — 论文和 PDF 文献
- `raw/03-transcripts/` — 视频转录文案
- `raw/04-meeting_notes/` — 会议/课堂笔记
- `raw/09-archive/` — **已处理文件的归档目录，禁止读取**
- `wiki/sources/` — 资料摘要
- `wiki/entities/` — 实体（人物、公司、工具、产品）
- `wiki/concepts/` — 概念（框架、方法论、理论）
- `wiki/syntheses/` — 综合分析报告

## 状态追踪机制（增量 Ingest）

系统通过 `.claude/ingest-state.json` 追踪处理状态，避免重复编译：

```json
{
  "version": 1,
  "last_full_scan": "2026-05-01T10:00:00",
  "files": {
    "raw/01-articles/xxx.md": {
      "hash": "a3f2c1d4",
      "status": "archived",
      "ingested_at": "2026-05-01T10:30:00",
      "outputs": [
        "wiki/sources/摘要-xxx.md",
        "wiki/concepts/YYY.md",
        "wiki/entities/ZZZ.md"
      ]
    }
  }
}
```

### 状态规则

| 状态 | 含义 | 处理方式 |
|------|------|---------|
| `pending` | 从未处理过 | 完整编译 |
| `modified` | 文件内容有变更（哈希不同）| 重新编译，更新关联页面 |
| `archived` | 已处理且源文件未变更 | **跳过** |
| `failed` | 上次处理失败 | 重新尝试 |

### 哈希计算
对文件内容计算简单哈希（如 MD5 或 SHA-256），用于检测变更。

## 触发逻辑

1. **用户执行 `/ingest`**：扫描 `raw/` 所有子目录（排除 `09-archive/`），找出待处理文件。
2. **用户执行 `/ingest <path>`**：仅处理指定文件。
3. **隐式触发**：用户说"把这个资料摄入知识库"、"导入这篇文章"时，自动执行 ingest。

## 编译流水线

对每个待处理源文件，严格按以下步骤执行：

### 步骤 1：检查状态

- 读取 `.claude/ingest-state.json`
- 计算文件当前哈希
- 对比历史记录：
  - 状态为 `archived` 且哈希一致 → **跳过，报告"已是最新"**
  - 状态为 `modified` 或无记录 → **继续编译**

### 步骤 2：读取源文件

- **如果是 `.md` 文件**：使用读取工具完整读取内容。
- **如果是 `.pdf` 文件**：使用读取工具尝试提取文本。如果无法提取或内容为空，改为记录文件元信息（文件名、页数）在 sources 页面中。

### 步骤 3：提炼核心

从源文件中提取：
- **核心主旨**：这段资料讲什么（1-2句话）
- **实体**：人物、公司、工具、产品等具体名词
- **概念**：框架、方法论、理论等抽象名词

如果是非中文内容，则翻译成中文。

### 步骤 4：创建来源摘要

在 `wiki/sources/` 创建 Markdown 文件：

```markdown
---
title: "摘要-文件slug"
type: source
tags: [来源, 原始文件]
sources: [raw/01-articles/xxx.md]
last_updated: YYYY-MM-DD
---

## 核心摘要
[3-5句话的核心总结]

## 关联连接
- [[EntityName]] — 关联实体
- [[ConceptName]] — 关联概念
```

文件名使用 kebab-case：`摘要-{文件slug}.md`

### 步骤 5：知识网络化（实体/概念页面）

对于步骤 3 提取的每个实体和概念：

**目标目录：**
- 实体 → `wiki/entities/`
- 概念 → `wiki/concepts/`

**处理逻辑：**
1. 页面不存在 → 按照 CLAUDE.md 的 Frontmatter 规范创建新页面
2. 页面已存在 → 读取现有内容，**增量合并**新信息
3. **发现冲突** → 在页面中记录冲突，继续处理其他文件，全部完成后统一向用户报告所有冲突

**页面模板：**

创建页面时必须遵循 `obsidian-markdown` 技能规范，正确使用 wikilinks、callouts、frontmatter 等 Obsidian 特有语法。

```markdown
---
title: "页面名称"
type: entity | concept
tags: [标签]
sources: [关联的源文件]
last_updated: YYYY-MM-DD
---

## 定义
[对该实体/概念的定义]

## 关键信息
[从源文件中提取的详细信息]

## 关联连接
- [[摘要-source-slug]] — 来源
- [[RelatedEntity]] — 相关实体
```

### 步骤 6：更新全局注册表

**更新 `wiki/index.md`：**
按照 CLAUDE.md 规定的格式，将新增页面添加到对应分类下。

**更新 `wiki/log.md`：**
追加操作日志（Append-only）：
```markdown
## [YYYY-MM-DD] ingest | 操作简述
- **变更**: 新增 [[PageName]]; 更新 [[index.md]]
- **冲突**: 无 (或: 冲突 [[ConflictingPage]], 已暂停等待决策)
```

### 步骤 7：归档源文件

在确认以下全部完成后，将源文件移动到 `raw/09-archive/`：
- sources 页面已创建
- 实体/概念页面已创建或更新
- index.md 已更新
- log.md 已更新

**绝对禁止修改源文件内部的文字。**

### 步骤 8：更新状态记录

在 `.claude/ingest-state.json` 中更新该文件记录：

```json
{
  "raw/01-articles/xxx.md": {
    "hash": "新哈希",
    "status": "archived",
    "ingested_at": "当前时间",
    "outputs": ["wiki/sources/摘要-xxx.md", "wiki/concepts/YYY.md"]
  }
}
```

## 增量更新规则

当处理 **已存在** 的实体/概念页面时：

1. **读取现有内容**
2. **对比新旧信息**：
   - 新信息是现有信息的补充 → 在对应区块追加
   - 新信息与现有信息矛盾 → 触发冲突处理流程
   - 新信息已存在 → 跳过
3. **更新 `last_updated` 字段**
4. **保留原有双向链接**，只添加新的

## 冲突处理流程

当发现新旧知识冲突时：

1. **记录**：在当前页面的 `## 知识冲突` 区块追加冲突记录，并在 `wiki/log.md` 中标记
2. **继续**：不中断流水线，继续处理其他文件
3. **批量报告**：全部文件处理完毕后，统一向用户报告所有发现的冲突及所在页面
4. **修复**：用户确认后，再对标记的冲突页面执行具体修复（覆盖、合并或保留两者）

## 注意事项

- 绝对不读取 `raw/09-archive/` 下的任何文件
- 所有 wiki 页面必须包含 `## 关联连接` 区域，不能产生孤岛页面
- 使用简体中文编写所有内容
- 实体命名使用 TitleCase，概念和来源使用 kebab-case
- **增量机制**：通过状态文件避免重复处理，显著提高性能
