# Obsidian 配置说明

本知识库已预置统一的 Obsidian 配置和 CSS 样式，开箱即用。

## 已启用的核心插件

| 插件 | 用途 |
|------|------|
| Graph | 知识图谱可视化 |
| Backlink | 反链面板 |
| Page Preview | 悬浮预览 |
| Templates | 笔记模板 |
| Note Composer | 笔记合并/拆分 |
| Outline | 文档大纲 |
| Tag Pane | 标签面板 |
| Word Count | 字数统计 |

## 推荐安装的社区插件

首次打开 Vault 后，请前往 **Settings → Community plugins → Browse** 安装以下插件：

| 类别 | 插件 | 用途 |
|------|------|------|
| 查询 | **Dataview** | 数据查询与动态列表（index.md 仪表盘依赖此插件） |
| 查询 | **Omnisearch** | 全文搜索增强 |
| 效率 | **Templater** | 高级模板引擎 |
| 效率 | **QuickAdd** | 快速捕获与笔记创建 |
| 效率 | **Commander** | 自定义命令与工具栏 |
| 组织 | **Calendar** | 日历视图 |
| 组织 | **Periodic Notes** | 周期笔记（日/周/月/年） |
| 组织 | **Tag Wrangler** | 标签批量管理 |
| 视觉 | **Style Settings** | 主题样式微调 |
| 视觉 | **Callout Manager** | Callout 自定义管理 |
| 视觉 | **Hover Editor** | 悬浮编辑窗口 |
| 导航 | **Recent Files** | 最近打开文件 |

> 已安装的插件列表保存在 `.obsidian/community-plugins.json` 中。

## CSS 样式片段

位于 `.obsidian/snippets/`，已自动启用：

| 片段 | 说明 |
|------|------|
| `wiki-reading.css` | 阅读排版优化：标题层级、段落间距、引用块、表格 |
| `wiki-callouts.css` | Callout 美化：语义化颜色 + 左侧边条 |
| `wiki-components.css` | 组件增强：双链、外部链接、标签、图片、代码块 |

如需调整样式，可在 **Settings → Appearance → CSS Snippets** 中开关单个片段。

## 标准模板

位于项目根目录 `templates/`，包含 4 种页面类型：

| 模板 | 用途 | 关键区块 |
|------|------|---------|
| `entity.md` | 人物、公司、工具、产品等实体 | 基本资料、时间线、评价与影响 |
| `concept.md` | 概念、框架、方法论 | 定义、原理、类比、误区、应用场景 |
| `source.md` | 原始资料摘要 | 元信息、核心摘要、批注、可信度评估 |
| `synthesis.md` | 综合分析报告 | 研究问题、证据表、结论、行动建议 |

### 配置 Templater 使用模板

1. 安装 **Templater** 社区插件
2. 前往 **Settings → Templater**，设置 `Template folder location` 为 `templates`
3. 可选：设置 `Trigger Templater on new file creation` 为开启

### 手动使用模板

新建笔记时，按 `Ctrl/Cmd + P` 打开命令面板，搜索 "Templater: Open Insert Template Modal" 选择对应模板。

## Dataview 动态仪表盘

`wiki/index.md` 已预置 Dataview 查询，安装 Dataview 插件后自动生效：

- **最新来源**：自动列出最近摄入的 raw 资料
- **概念库/实体库/综合分析**：按类型动态聚合
- **待处理清单**：自动找出草稿状态、无来源的页面

如需创建新的主题地图（MOC），参考 `wiki/mocs/README.md` 中的 Dataview 查询模板。

## 推荐主题

本 CSS 片段不依赖特定主题，在默认主题下效果最佳。如需更换主题，建议：

- **Minimal** — 极简高定制（配合 Style Settings 使用）
- **Things** — 清爽语义化配色
- **AnuPpuccin** — 柔和护眼

## Obsidian Markdown 高级语法

本知识库的 Agent 在创建内容时遵循统一的 Markdown 规范，支持以下 Obsidian 特有语法：

### 内部链接（Wikilinks）

```markdown
[[页面名称]]                    → 基础双链
[[页面名称|显示文本]]           → 自定义显示文本
[[页面名称#标题]]               → 链接到特定标题
[[页面名称#^块ID]]              → 链接到特定段落
```

**块 ID**：在任意段落末尾追加 `^块ID` 即可创建精确链接锚点：
```markdown
这是一个可被精确引用的段落。 ^my-block-id
```

### 嵌入（Embeds）

```markdown
![[图片.png|300]]              → 嵌入图片并限制宽度
![[文档.pdf#page=3]]           → 嵌入 PDF 第 3 页
![[页面名称#^块ID]]            → 嵌入其他页面的特定段落
```

### Callouts

```markdown
> [!note]
> 笔记型 callout

> [!warning] 自定义标题
> 带自定义标题的警告

> [!faq]- 默认折叠
> 可折叠的 callout（- 折叠，+ 展开）
```

### 注释（Comments）

```markdown
这是可见文本 %%这是隐藏的 AI 批注%%。

%%
整段隐藏内容。
可用于记录处理逻辑，不影响阅读视图。
%%
```

---

## Canvas 可视化

本知识库包含 `json-canvas` Agent Skill，可将 `wiki/` 中的知识网络自动生成为 `.canvas` 可视化文件。

### 使用方法

在 Kimi Code CLI 中执行：
```
/canvas
```

Agent 会扫描 `wiki/` 中的页面和双链关系，生成一个可视化画布文件（如 `wiki/knowledge-graph.canvas`），你可以在 Obsidian 的 Canvas 视图中打开查看和编辑。

### Canvas 中的节点类型

- **文本节点** — 概念定义、关键洞察
- **文件节点** — 链接到具体的 wiki 页面（可点击跳转）
- **链接节点** — 外部参考 URL
- **分组节点** — 按主题组织相关概念

---

## 首次使用步骤

1. 在 Obsidian 中打开本文件夹作为 Vault
2. 前往 **Settings → Community plugins**，关闭 Restricted mode
3. 按上表安装需要的社区插件（至少安装 **Dataview** 和 **Templater**）
4. 在 **Appearance → CSS Snippets** 中确认三个片段已启用
5. 配置 Templater 的模板文件夹为 `templates/`
6. 将原始资料放入 raw/ 目录
7. 执行 `/ingest` 开始编译知识
8. 执行 `/canvas` 生成知识网络可视化图谱
