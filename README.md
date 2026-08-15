# init-knowledge-base

基于 [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 架构，一键初始化标准化的 Obsidian 知识库项目骨架。

## 简介

这是一个 [Kimi Code CLI](https://github.com/moonshot-ai/Kimi-CLI) 的 **Project Skill**，用于将碎片化的信息编译成**结构化、高度相互链接**的知识网络，并预置统一的 Obsidian 配置、阅读优化样式、标准化模板和动态知识仪表盘。

## 特性

- 📁 **标准目录结构**：`raw/`（原始资料）、`wiki/`（知识编译输出）、`assets/`（媒体资源）
- 📝 **4 种标准模板**：Entity（实体）、Concept（概念）、Source（来源摘要）、Synthesis（综合分析）
- ⚙️ **统一 Obsidian 配置**：预置核心插件清单、社区插件推荐、编辑器设置、模板路径预设、图谱颜色分组
- 🎨 **3 个 CSS 阅读样式**：`wiki-reading.css`、`wiki-callouts.css`、`wiki-components.css`
- 📊 **Dataview 动态仪表盘**：`wiki/index.md` 自动聚合概念库、实体库、待处理清单、时效性监控
- 🤖 **6 个 Agent Skills**：`ingest`（增量编译+讨论确认+URL摄入）、`query`（智能查询）、`lint`（健康检查+概念空缺检测）、`refresh`（联网时效性更新）、`obsidian-markdown`（语法规范）、`json-canvas`（知识可视化）
- 🗺️ **Markmap 思维导图**：预置知识库全景思维导图，安装插件后可交互浏览项目架构

## 快速开始

在 Kimi Code CLI 中输入：

```
/init-vault <你的知识库名称>
```

或自然语言：

> "新建一个知识库" / "创建 vault" / "初始化知识库"

Skill 将自动生成完整的项目结构，你可以直接在 Obsidian 中打开使用。

## 生成的知识库结构

```
{project_root}/
├── README.md                 # 项目说明
├── CLAUDE.md                 # Agent 行为契约与规范
├── OBSIDIAN_SETUP.md         # Obsidian 配置指南
├── .gitignore                # 忽略工作区与插件二进制文件
├── assets/                   # 媒体资源层：图片、PDF、附件
├── raw/                      # 原始资料收件箱（只读，不可变层）
│   ├── 01-articles/          # 网页剪藏、文章
│   ├── 02-papers/            # 论文、研报、PDF
│   ├── 03-transcripts/       # 视频/播客转录
│   ├── 04-meeting_notes/     # 会议/课堂笔记
│   └── 09-archive/           # 已归档区
├── templates/                # Templater 模板文件夹
│   ├── entity.md             # 实体模板（人物、公司、工具）
│   ├── concept.md            # 概念模板（框架、方法论）
│   ├── source.md             # 来源摘要模板
│   └── synthesis.md          # 综合分析模板
├── wiki/                     # 知识编译输出层
│   ├── index.md              # Dataview 动态仪表盘（全局内容字典）
│   ├── log.md                # 操作日志（Append-only）
│   ├── mocs/                 # 主题地图（Map of Contents）
│   │   ├── Mindmap-知识库全景.md # Markmap 思维导图（安装插件后可交互）
│   ├── concepts/             # 概念、框架、方法论
│   ├── entities/             # 人物、公司、工具
│   ├── sources/              # 原始资料摘要
│   └── syntheses/            # 综合分析报告
├── .obsidian/                # Obsidian 配置与样式
│   ├── app.json              # 编辑器设置（默认阅读模式）
│   ├── appearance.json       # 外观 + CSS 片段启用清单
│   ├── core-plugins.json     # 核心插件开关
│   ├── community-plugins.json # 推荐社区插件
│   ├── templates.json        # 模板文件夹路径预设
│   ├── graph.json            # 图谱视图颜色分组预设
│   └── snippets/             # CSS 样式片段
│       ├── wiki-reading.css
│       ├── wiki-callouts.css
│       └── wiki-components.css
└── .claude/
    └── skills/               # Agent Skills
        ├── ingest/           # 将 raw/ 资料编译到 wiki/（支持讨论确认）
        ├── query/            # 在知识库中搜索与回答
        ├── lint/             # 检查死链、孤儿页面、概念空缺、逻辑冲突
        ├── refresh/          # 联网搜索更新陈旧知识，确保时效性
        ├── obsidian-markdown/ # Obsidian Markdown 语法规范
        └── json-canvas/      # Canvas 可视化与知识图谱
```

## 核心设计原则

1. **raw/ 不可变**：原始资料只读，是事实的唯一真相来源。
2. **wiki/ 双向链接**：每个页面必须包含 `## 关联连接` 区域，不能产生孤岛页面。
3. **增量 ingest**：通过状态追踪避免重复编译，只处理新增或变更的原始资料。
4. **矛盾显式化**：新旧知识冲突时，在页面中新建 `## 知识冲突` 区块保留对比。
5. **联网时效性**：通过 `/refresh` 主动联网搜索，验证和更新陈旧知识，确保时效性。

## 内置 Agent Skills

| Skill | 触发命令 | 功能 |
|-------|---------|------|
| **ingest** | `/ingest <路径或URL>` | 读取 raw/ 文件或抓取 URL 网页，提炼到 wiki/，支持讨论确认，自动更新 index 和 log |
| **query** | `/query <问题>` | 通过 wiki/index.md 查找相关文件，深度阅读后用 `[[wikilink]]` 标注来源回答 |
| **lint** | `/lint` | 全局扫描 wiki/，找出孤儿页面、死链、概念空缺、逻辑冲突，输出研究方向建议 |
| **refresh** | `/refresh` | 联网搜索最新信息，验证和更新陈旧页面，标记冲突，按领域设置刷新周期 |
| **obsidian-markdown** | 隐式调用 | 所有写入操作遵循的 Obsidian Markdown 语法规范 |
| **json-canvas** | `/canvas` | 将 wiki/ 中的知识网络自动生成为 `.canvas` 可视化图谱 |

## 文件说明

- `SKILL.md` — Skill 定义与生成流水线（供 Kimi Code CLI 读取）
- `_templates/` — 所有模板文件，初始化时按流水线复制到目标项目

## 依赖

- [Obsidian](https://obsidian.md/)（桌面端）
- 推荐社区插件：[Dataview](https://github.com/blacksmithgu/obsidian-dataview)、[Templater](https://github.com/SilentVoid13/Templater)、[Omnisearch](https://github.com/scambier/obsidian-omnisearch)、[Mindmap](https://github.com/lynchjames/obsidian-mindmap)

## 致谢

- 灵感来源于 Andrej Karpathy 的 [LLM Wiki 规范](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

## License

MIT
