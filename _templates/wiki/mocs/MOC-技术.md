---
title: "MOC-技术"
type: moc
aliases: []
tags: [moc, 技术]
sources: []
created: "<% tp.date.now('YYYY-MM-DD') %>"
last_updated: "<% tp.date.now('YYYY-MM-DD') %>"
status: "finished"
---

# MOC-技术

> 技术概念、工具、框架的知识地图。

## 技术领域图谱

```mermaid
mindmap
  root((技术))
    AI/机器学习
      深度学习
      NLP
      计算机视觉
      强化学习
    前端
      框架
      CSS
      构建工具
    后端
      语言
      数据库
      中间件
    基础设施
      云计算
      容器化
      CI/CD
    开发工具
      编辑器
      版本控制
      调试工具
```

## 概念

```dataview
TABLE complexity AS "复杂度", last_updated AS "更新日期", status AS "状态"
FROM "wiki/concepts"
WHERE contains(tags, "#技术") OR contains(tags, "#tech")
SORT last_updated DESC
```

## 实体

```dataview
TABLE entity_type AS "类型", last_updated AS "更新日期", status AS "状态"
FROM "wiki/entities"
WHERE contains(tags, "#技术") OR contains(tags, "#tech")
SORT last_updated DESC
```

## 综合分析

```dataview
TABLE confidence AS "置信度", last_updated AS "更新日期"
FROM "wiki/syntheses"
WHERE contains(tags, "#技术") OR contains(tags, "#tech")
SORT last_updated DESC
```

> 💡 提示：给相关页面添加 `#技术` 或 `#tech` 标签即可自动聚合到本 MOC。
