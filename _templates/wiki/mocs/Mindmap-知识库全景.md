---
title: "知识库全景思维导图"
type: moc
aliases: ["知识库思维导图", "vault mindmap"]
tags: [思维导图, MOC, mermaid]
sources: []
created: 2026-08-15
last_updated: 2026-08-15
status: finished
---

# 知识库全景

> [!tip] 关于本页面
> 以下思维导图使用 **Mermaid** 语法，Obsidian 原生支持，无需安装任何插件。
> 如同时安装了 Mindmap 插件，本页面的层级标题也可渲染为交互式思维导图。

## 架构总览

```mermaid
mindmap
  root((LLM Wiki))
    原始资料 raw
      文章 01-articles
      论文 02-papers
      转录 03-transcripts
      笔记 04-meeting_notes
      归档 09-archive
    知识编译 wiki
      概念 concepts
        框架
        方法论
        理论
      实体 entities
        人物
        公司
        工具
        产品
      来源摘要 sources
        可信度评估
      综合分析 syntheses
        跨源对比
      主题地图 mocs
        MOC技术
        MOC商业
        MOC人物
        MOC待整理
    媒体资源 assets
      图片
      PDF
      附件
    Agent技能
      ingest 摄入
        增量编译
        讨论确认
        URL摄入
      query 查询
      lint 健康检查
        死链检测
        概念空缺
        研究建议
      refresh 联网刷新
        按领域周期
        冲突标记
      canvas 可视化
    配置体系
      Obsidian配置
        app.json
        appearance.json
        templates.json
        graph.json
      CSS样式
        wiki-reading
        wiki-callouts
        wiki-components
      模板系统
        entity
        concept
        source
        synthesis
```

## 知识编译流程

```mermaid
flowchart LR
    subgraph Raw["📥 raw/ 不可变层"]
        R1[文章]
        R2[论文]
        R3[转录]
        R4[URL网页]
    end

    subgraph Ingest["🤖 ingest"]
        I1[提取实体/概念]
        I2[讨论确认]
        I3[创建wiki页面]
    end

    subgraph Wiki["🧠 wiki/ 编译层"]
        W1[concepts/]
        W2[entities/]
        W3[sources/]
        W4[syntheses/]
        W5[index.md]
    end

    subgraph Maintain["🔧 维护"]
        M1[lint<br/>健康检查]
        M2[refresh<br/>联网更新]
        M3[canvas<br/>可视化]
    end

    R1 & R2 & R3 --> I1
    R4 -->|自动抓取| I1
    I1 --> I2 --> I3
    I3 --> W1 & W2 & W3 & W4
    I3 --> W5

    W1 & W2 & W3 & W4 --> M1
    W1 & W2 & W3 & W4 --> M2
    W1 & W2 & W3 & W4 --> M3

    style Raw fill:#2e3440,stroke:#5e81ac,color:#e5e9f0
    style Ingest fill:#2e3440,stroke:#a3be8c,color:#e5e9f0
    style Wiki fill:#2e3440,stroke:#88c0d0,color:#e5e9f0
    style Maintain fill:#2e3440,stroke:#b48ead,color:#e5e9f0
```

## Agent 技能交互

```mermaid
flowchart TD
    User([用户])

    User -->|"/ingest path"| IG[ingest<br/>增量编译]
    User -->|"/ingest url"| IG
    User -->|"/query 问题"| QY[query<br/>智能查询]
    User -->|"/lint"| LT[lint<br/>健康检查]
    User -->|"/refresh"| RF[refresh<br/>联网更新]
    User -->|"/canvas"| CV[canvas<br/>图谱生成]

    IG -->|"写入"| Wiki[(wiki/)]
    QY -->|"读取"| Wiki
    LT -->|"扫描"| Wiki
    RF -->|"更新"| Wiki
    CV -->|"读取"| Wiki

    IG -.->|"遵循规范"| OM[obsidian-markdown<br/>语法规范]
    QY -.-> OM
    LT -.-> OM
    RF -.-> OM

    IG -->|"状态追踪"| IS[(ingest-state.json)]
    RF -->|"状态追踪"| RS[(refresh-state.json)]

    LT -->|"输出"| Report[健康报告<br/>死链/孤儿/空缺/建议]
    QY -->|"输出"| Answer[回答<br/>带wikilink来源]
    RF -->|"输出"| Refresh[刷新报告<br/>更新/冲突]
    CV -->|"输出"| Canvas[.canvas文件]

    style User fill:#5e81ac,stroke:#5e81ac,color:#fff
    style Wiki fill:#88c0d0,stroke:#88c0d0,color:#2e3440
    style OM fill:#4c566a,stroke:#4c566a,color:#e5e9f0
    style IS fill:#d08770,stroke:#d08770,color:#2e3440
    style RS fill:#d08770,stroke:#d08770,color:#2e3440
```

## 目录权限模型

```mermaid
flowchart TB
    subgraph Immutable["⛔ 不可变层"]
        direction LR
        RAW["raw/<br/>原始资料<br/>只读"]
        ARC["raw/09-archive/<br/>归档区<br/>禁止读取"]
    end

    subgraph Media["🖼️ 媒体层"]
        ASSETS["assets/<br/>图片/PDF/附件<br/>引用: ![[file]]"]
    end

    subgraph Workspace["✏️ 编译层 Agent 工作区"]
        direction LR
        WIKI["wiki/<br/>概念/实体/来源/综合<br/>可创建/更新/提炼"]
        TPL["templates/<br/>Templater模板<br/>只读引用"]
    end

    subgraph Config["⚙️ 配置层"]
        direction LR
        OBS[".obsidian/<br/>Obsidian配置"]
        CLD[".claude/<br/>Agent Skills + 状态文件"]
    end

    RAW -->|"ingest 编译"| WIKI
    WIKI -->|"![[嵌入]]"| ASSETS
    TPL -.->|"模板引用"| WIKI

    style Immutable fill:#bf616a,stroke:#bf616a,color:#fff
    style Media fill:#d08770,stroke:#d08770,color:#fff
    style Workspace fill:#a3be8c,stroke:#a3be8c,color:#fff
    style Config fill:#5e81ac,stroke:#5e81ac,color:#fff
```

## 关联连接

- [[index]] — 全局内容字典
- [[log]] — 操作日志
- [[MOC-技术]] — 技术领域主题地图
- [[MOC-商业]] — 商业领域主题地图
- [[MOC-人物]] — 人物与机构主题地图
- [[MOC-待整理]] — 待分类与草稿
