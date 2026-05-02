# Maps of Content (MOC)

MOC（内容地图）是主题层级的导航入口。每个 MOC 是一个中心节点，通过 Dataview 动态聚合相关笔记。

## 创建新 MOC 的方法

1. 在 `wiki/mocs/` 下新建文件，命名格式 `MOC-主题名.md`
2. 使用标签或路径规则定义聚合范围
3. 用 Dataview 查询自动列出相关页面

## Dataview 查询模板

按标签聚合：
```dataview
TABLE type, last_updated
FROM "wiki"
WHERE contains(tags, "#ai")
SORT last_updated DESC
```

按路径聚合：
```dataview
TABLE type, last_updated
FROM "wiki/concepts"
WHERE contains(file.path, "architecture")
SORT last_updated DESC
```

按链接关系聚合：
```dataview
TABLE type, last_updated
FROM "wiki"
WHERE contains(file.outlinks, [[核心概念]])
SORT last_updated DESC
```
