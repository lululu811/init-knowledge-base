---
title: "MOC-待整理"
type: moc
aliases: []
tags: [moc, 待整理]
sources: []
created: "<% tp.date.now('YYYY-MM-DD') %>"
last_updated: "<% tp.date.now('YYYY-MM-DD') %>"
status: "finished"
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

## 长期未更新

```dataview
LIST
FROM "wiki"
WHERE type != null
  AND last_updated != null
  AND date(last_updated) < date(today) - dur(90 days)
SORT last_updated ASC
```
