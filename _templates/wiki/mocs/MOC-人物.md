---
title: "MOC-人物"
type: moc
aliases: []
tags: [moc, 人物]
sources: []
created: "<% tp.date.now('YYYY-MM-DD') %>"
last_updated: "<% tp.date.now('YYYY-MM-DD') %>"
status: "finished"
---

# MOC-人物

> 关键人物与机构的知识地图。

## 人物与机构图谱

```mermaid
mindmap
  root((人物与机构))
    科技领袖
      AI研究者
      创业者
      开源贡献者
    学者
      计算机科学
      物理学
      数学
      经济学
    企业
      科技公司
      投资机构
      研究院所
    社区
      开源项目
      技术社区
      标准组织
```

## 人物

```dataview
TABLE entity_type AS "类型", last_updated AS "更新日期", status AS "状态"
FROM "wiki/entities"
WHERE entity_type = "人物" OR contains(tags, "#人物")
SORT last_updated DESC
```

## 公司与机构

```dataview
TABLE entity_type AS "类型", last_updated AS "更新日期", status AS "状态"
FROM "wiki/entities"
WHERE entity_type = "公司" OR entity_type = "机构" OR contains(tags, "#公司")
SORT last_updated DESC
```
