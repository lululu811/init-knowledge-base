---
title: "MOC-待整理"
type: moc
tags: [moc]
last_updated: "<% tp.date.now('YYYY-MM-DD') %>"
---

# MOC-待整理

> 待分类的知识碎片与草稿。

## 草稿页面

```dataview
LIST
FROM "wiki"
WHERE status = "draft"
SORT last_updated DESC
```

## 无来源页面

```dataview
LIST
FROM "wiki"
WHERE type != null
  AND (sources = null OR length(sources) = 0)
SORT file.name ASC
```
