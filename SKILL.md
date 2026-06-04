---
name: init-knowledge-base
description: 基于 Karpathy LLM Wiki 架构快速初始化新的 Obsidian 知识库项目。创建标准目录结构、核心配置文件、Obsidian 统一配置（插件+样式）、Templater 模板集、Dataview MOC 仪表盘、三个 Agent Skills（ingest/query/lint）。当用户提到"新建知识库"、"初始化知识库"、"创建 vault"、"init vault"、"建立知识库"时使用。
user-invocable: true
---

# init-knowledge-base 技能

## 核心目标
基于 Karpathy LLM Wiki 理念，一键生成标准化的 Obsidian 知识库项目骨架，包含统一的插件配置、阅读优化样式、标准化模板和动态知识仪表盘。

## 触发场景
- 用户输入 `/init-vault <项目名>`
- 用户说"新建一个知识库"、"创建知识库"、"初始化 vault"
- 用户需要在当前目录下建立新的知识库项目

## 参数
- `vault_name`（可选）：知识库名称

## 模板位置

所有模板文件位于 `_templates/` 目录下（相对于本 skill 目录）：

```
_templates/
├── README.md
├── CLAUDE.md
├── OBSIDIAN_SETUP.md
├── .gitignore
├── .obsidian/
│   ├── app.json
│   ├── appearance.json
│   ├── core-plugins.json
│   ├── community-plugins.json
│   └── snippets/
│       ├── wiki-reading.css
│       ├── wiki-callouts.css
│       └── wiki-components.css
├── templates/
│   ├── entity.md
│   ├── concept.md
│   ├── source.md
│   └── synthesis.md
├── wiki/
│   ├── index.md
│   ├── log.md
│   └── mocs/
│       ├── README.md
│       ├── MOC-技术.md
│       ├── MOC-商业.md
│       ├── MOC-人物.md
│       └── MOC-待整理.md
└── .claude/
    └── skills/
        ├── ingest/SKILL.md
        ├── query/SKILL.md
        ├── lint/SKILL.md
        ├── obsidian-markdown/SKILL.md
        └── json-canvas/SKILL.md
```

## 生成流水线

### 步骤 1：确认项目名称和位置

如果用户未指定名称，询问：
> 请告诉我新知识库的名称（如"physics-notes"、"study-vault"）和存放路径。

### 步骤 2：创建目录结构

在项目根目录下创建以下结构：

```
{project_root}/
├── README.md
├── CLAUDE.md
├── OBSIDIAN_SETUP.md
├── .gitignore
├── assets/
├── raw/
│   ├── 01-articles/
│   ├── 02-papers/
│   ├── 03-transcripts/
│   ├── 04-meeting_notes/
│   └── 09-archive/
├── templates/
│   ├── entity.md
│   ├── concept.md
│   ├── source.md
│   └── synthesis.md
├── wiki/
│   ├── index.md
│   ├── log.md
│   ├── mocs/
│   │   └── README.md
│   ├── concepts/
│   ├── entities/
│   ├── sources/
│   └── syntheses/
├── .obsidian/
│   ├── app.json
│   ├── appearance.json
│   ├── core-plugins.json
│   ├── community-plugins.json
│   └── snippets/
│       ├── wiki-reading.css
│       ├── wiki-callouts.css
│       └── wiki-components.css
└── .claude/
    └── skills/
        ├── ingest/
        ├── query/
        └── lint/
```

### 步骤 3：复制模板文件

从 `_templates/` 目录复制文件到项目根目录：

- `README.md` → 将其中的 `{vault_name}` 替换为用户指定的名称
- `CLAUDE.md` → 原样复制
- `OBSIDIAN_SETUP.md` → 原样复制
- `.gitignore` → 原样复制
- `wiki/index.md` → 原样复制（Dataview 动态仪表盘）
- `wiki/log.md` → 原样复制
- `wiki/mocs/README.md` → 原样复制
- `wiki/mocs/MOC-技术.md` → 原样复制
- `wiki/mocs/MOC-商业.md` → 原样复制
- `wiki/mocs/MOC-人物.md` → 原样复制
- `wiki/mocs/MOC-待整理.md` → 原样复制

### 步骤 4：安装标准化模板

从 `_templates/templates/` 复制到项目根目录：

- `entity.md` → `templates/entity.md`
- `concept.md` → `templates/concept.md`
- `source.md` → `templates/source.md`
- `synthesis.md` → `templates/synthesis.md`

> 这些模板使用 Templater 语法（如 `<% tp.file.title %>`），供 Templater 插件解析。安装 Templater 后，前往 **Settings → Templater**，将 `Template folder location` 设为 `templates/`。

### 步骤 5：安装 Obsidian 配置

从 `_templates/.obsidian/` 复制：

- `app.json` → `.obsidian/app.json`（编辑器设置）
- `appearance.json` → `.obsidian/appearance.json`（外观+CSS片段启用）
- `core-plugins.json` → `.obsidian/core-plugins.json`（核心插件列表）
- `community-plugins.json` → `.obsidian/community-plugins.json`（推荐社区插件列表）
- `snippets/*.css` → `.obsidian/snippets/*.css`（阅读优化样式）

### 步骤 6：安装 Agent Skills

从 `_templates/.claude/skills/` 复制：

- `ingest/SKILL.md` → `.claude/skills/ingest/SKILL.md`
- `query/SKILL.md` → `.claude/skills/query/SKILL.md`
- `lint/SKILL.md` → `.claude/skills/lint/SKILL.md`
- `obsidian-markdown/SKILL.md` → `.claude/skills/obsidian-markdown/SKILL.md`
- `json-canvas/SKILL.md` → `.claude/skills/json-canvas/SKILL.md`

### 步骤 7：输出完成报告

```markdown
## ✅ 知识库初始化完成 — {vault_name}

### 已创建
- 📁 目录结构：raw/, wiki/, assets/, templates/, .claude/skills/
- 📄 核心文件：README.md, CLAUDE.md, wiki/index.md, wiki/log.md
- 📝 标准模板：entity / concept / source / synthesis（共 4 个）
- ⚙️ Obsidian 配置：统一插件清单 + 3 个 CSS 阅读样式
- 🤖 Agent Skills: ingest（增量）, query, lint, obsidian-markdown（语法规范）, json-canvas（可视化）

### 首次使用 Obsidian
1. 在 Obsidian 中打开此文件夹作为 Vault
2. 参考 `OBSIDIAN_SETUP.md` 安装推荐社区插件
3. 确认 **Settings → Appearance → CSS Snippets** 中三个片段已启用
4. 设置 **Core Plugins → Templates** 的模板文件夹为 `templates/`
5. 将原始资料放入 raw/ 目录
6. 执行 `/ingest` 开始编译知识
```

## 注意事项

- 如果目标目录已存在同名文件，询问用户是否覆盖
- 所有文件使用 UTF-8 编码
- Skill 文件保持通用性，不绑定特定主题领域
- `.obsidian/` 中的 `workspace*.json` 和插件二进制文件已加入 `.gitignore`，但配置文件和 CSS 片段会被版本控制保留
- **增量 ingest**：通过 `.claude/ingest-state.json` 追踪处理状态，避免重复编译
