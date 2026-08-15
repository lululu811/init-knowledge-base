# {vault_name}

本项目是一个基于 [Karpathy LLM Wiki 理念](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 构建的 Obsidian 知识库。

## 核心理念

将碎片化的信息编译成**结构化、高度相互链接**的知识网络，便于 AI 辅助学习和研究。

## 目录结构

```
📁 知识库项目
├── 🖼️ assets/              ← 媒体资源层：图片、PDF、附件
├── 📥 raw/                 ← 原始资料收件箱（只读）
│   ├── 01-articles/        ← 网页剪藏、文章
│   ├── 02-papers/          ← 论文、研报、PDF
│   ├── 03-transcripts/     ← 视频/播客转录
│   ├── 04-meeting_notes/   ← 会议/课堂笔记
│   └── 09-archive/         ← 已归档区
├── 🧠 wiki/                ← 知识编译输出层
│   ├── index.md            ← 全局内容字典
│   ├── log.md              ← 操作日志
│   ├── concepts/           ← 概念、框架、方法论
│   ├── entities/           ← 人物、公司、工具
│   ├── sources/            ← 原始资料摘要
│   └── syntheses/          ← 综合分析报告
└── .claude/skills/         ← Agent Skills
    ├── ingest/             # 将 raw/ 资料编译到 wiki/（支持讨论确认）
    ├── query/              # 在知识库中搜索与回答
    ├── lint/               # 检查死链、孤儿页面、概念空缺、逻辑冲突
    ├── refresh/            # 联网搜索更新陈旧知识，确保时效性
    ├── obsidian-markdown/  # Obsidian Markdown 语法规范
    └── json-canvas/        # Canvas 可视化与知识图谱
```

## 使用方式

在 Obsidian 中打开本 vault，使用 Claude Code 执行操作。

### 常用命令

- `/ingest <路径或URL>` — 将原始资料或网页编译到知识库（支持讨论确认）
- `/query <问题>` — 在知识库中搜索相关内容
- `/lint` — 检查知识库健康度（死链、孤儿页面、概念空缺）
- `/refresh` — 联网搜索更新陈旧知识，确保时效性
- `/canvas` — 将知识网络生成为可视化的 Canvas 图谱
