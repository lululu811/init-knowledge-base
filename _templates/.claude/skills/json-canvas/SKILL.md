---
name: json-canvas
description: 创建和编辑 Obsidian Canvas 文件（.canvas），用于可视化知识网络、思维导图、流程图。当用户要求"可视化知识图谱"、"生成思维导图"、"创建 canvas"时使用。可将 wiki/ 中的页面关系自动绘制成可视化画布。
user-invocable: true
---

# JSON Canvas 技能

本技能用于创建和编辑符合 JSON Canvas 1.0 规范的 `.canvas` 文件，实现知识网络的可视化呈现。

---

## 文件结构

Canvas 文件是标准 JSON，包含两个顶级数组：

```json
{
  "nodes": [],
  "edges": []
}
```

- `nodes`：节点数组（可选）
- `edges`：连接节点的边数组（可选）

---

## 节点（Nodes）

四种节点类型：`text`、`file`、`link`、`group`。

### 通用属性

| 属性 | 必需 | 类型 | 说明 |
|------|------|------|------|
| `id` | 是 | string | 唯一标识符（16位小写十六进制） |
| `type` | 是 | string | 节点类型 |
| `x` | 是 | integer | X 坐标（像素） |
| `y` | 是 | integer | Y 坐标（像素） |
| `width` | 是 | integer | 宽度（像素） |
| `height` | 是 | integer | 高度（像素） |
| `color` | 否 | string | 颜色（预设 `"1"`-`"6"` 或 HEX） |

### 文本节点（text）

```json
{
  "id": "6f0ad84f44ce9c17",
  "type": "text",
  "x": 0,
  "y": 0,
  "width": 400,
  "height": 200,
  "text": "# 核心概念\n\n这是 **Markdown** 内容。"
}
```

> **换行转义**：JSON 字符串中的换行必须写为 `\n`，不能写字面量 `\\n`。

### 文件节点（file）

引用 Vault 内的文件：

```json
{
  "id": "a1b2c3d4e5f67890",
  "type": "file",
  "x": 500,
  "y": 0,
  "width": 400,
  "height": 300,
  "file": "assets/diagram.png"
}
```

引用笔记中的特定标题：

```json
{
  "id": "b2c3d4e5f6789012",
  "type": "file",
  "x": 500,
  "y": 400,
  "width": 400,
  "height": 300,
  "file": "wiki/concepts/Transformer.md",
  "subpath": "#核心组件"
}
```

### 链接节点（link）

外部 URL：

```json
{
  "id": "c3d4e5f678901234",
  "type": "link",
  "x": 1000,
  "y": 0,
  "width": 400,
  "height": 200,
  "url": "https://obsidian.md"
}
```

### 分组节点（group）

视觉容器，用于组织其他节点：

```json
{
  "id": "d4e5f6789012345a",
  "type": "group",
  "x": -50,
  "y": -50,
  "width": 1000,
  "height": 600,
  "label": "项目概览",
  "color": "4"
}
```

带背景图：

```json
{
  "id": "e5f67890123456ab",
  "type": "group",
  "x": 0,
  "y": 700,
  "width": 800,
  "height": 500,
  "label": "资源",
  "background": "assets/background.png",
  "backgroundStyle": "cover"
}
```

背景样式：`cover`（填充）、`ratio`（保持比例）、`repeat`（平铺）。

---

## 边（Edges）

连接节点的线条：

```json
{
  "id": "f67890123456789a",
  "fromNode": "6f0ad84f44ce9c17",
  "toNode": "a1b2c3d4e5f67890"
}
```

完整属性：

```json
{
  "id": "0123456789abcdef",
  "fromNode": "节点A-ID",
  "fromSide": "right",
  "fromEnd": "none",
  "toNode": "节点B-ID",
  "toSide": "left",
  "toEnd": "arrow",
  "color": "1",
  "label": "导致"
}
```

| 属性 | 必需 | 默认值 | 说明 |
|------|------|--------|------|
| `fromNode` | 是 | — | 起点节点 ID |
| `toNode` | 是 | — | 终点节点 ID |
| `fromSide` | 否 | — | 起点边：`top`、`right`、`bottom`、`left` |
| `toSide` | 否 | — | 终点边 |
| `fromEnd` | 否 | `none` | 起点端点形状 |
| `toEnd` | 否 | `arrow` | 终点端点形状 |
| `color` | 否 | — | 线条颜色 |
| `label` | 否 | — | 边标签文字 |

---

## 颜色

### 预设颜色

| 预设 | 颜色 |
|------|------|
| `"1"` | 红色 |
| `"2"` | 橙色 |
| `"3"` | 黄色 |
| `"4"` | 绿色 |
| `"5"` | 青色 |
| `"6"` | 紫色 |

### 自定义 HEX

```json
{
  "color": "#5e81ac"
}
```

---

## 布局指南

### 推荐尺寸

| 节点类型 | 建议宽度 | 建议高度 |
|----------|----------|----------|
| 小文本 | 200-300 | 80-150 |
| 中文本 | 300-450 | 150-300 |
| 大文本 | 400-600 | 300-500 |
| 文件预览 | 300-500 | 200-400 |
| 链接预览 | 250-400 | 100-200 |

### 间距规范

- 分组内边距：20-50px
- 节点间距：50-100px
- 对齐网格：坐标取 10 或 20 的倍数

---

## 完整示例：知识网络可视化

```json
{
  "nodes": [
    {
      "id": "8a9b0c1d2e3f4a5b",
      "type": "text",
      "x": 400,
      "y": 200,
      "width": 300,
      "height": 150,
      "text": "# Transformer\n\n基于 [[自注意力机制]] 的深度学习架构。",
      "color": "4"
    },
    {
      "id": "1a2b3c4d5e6f7a8b",
      "type": "file",
      "x": 0,
      "y": 0,
      "width": 300,
      "height": 200,
      "file": "wiki/concepts/Attention Mechanism.md"
    },
    {
      "id": "2b3c4d5e6f7a8b9c",
      "type": "file",
      "x": 0,
      "y": 300,
      "width": 300,
      "height": 200,
      "file": "wiki/entities/OpenAI.md"
    },
    {
      "id": "3c4d5e6f7a8b9c0d",
      "type": "text",
      "x": 800,
      "y": 100,
      "width": 250,
      "height": 120,
      "text": "## BERT\n\n仅编码器架构"
    },
    {
      "id": "4d5e6f7a8b9c0d1e",
      "type": "text",
      "x": 800,
      "y": 300,
      "width": 250,
      "height": 120,
      "text": "## GPT\n\n仅解码器架构"
    }
  ],
  "edges": [
    {
      "id": "5e6f7a8b9c0d1e2f",
      "fromNode": "1a2b3c4d5e6f7a8b",
      "fromSide": "right",
      "toNode": "8a9b0c1d2e3f4a5b",
      "toSide": "left",
      "label": "核心机制"
    },
    {
      "id": "6f7a8b9c0d1e2f3a",
      "fromNode": "8a9b0c1d2e3f4a5b",
      "fromSide": "right",
      "toNode": "3c4d5e6f7a8b9c0d",
      "toSide": "left",
      "label": "变体"
    },
    {
      "id": "7a8b9c0d1e2f3a4b",
      "fromNode": "8a9b0c1d2e3f4a5b",
      "fromSide": "right",
      "toNode": "4d5e6f7a8b9c0d1e",
      "toSide": "left",
      "label": "变体"
    }
  ]
}
```

---

## 验证规则

1. 所有 `id` 必须在 nodes 和 edges 间全局唯一
2. `fromNode` 和 `toNode` 必须引用存在的 node ID
3. `type` 必须是 `text`、`file`、`link`、`group` 之一
4. `backgroundStyle` 必须是 `cover`、`ratio`、`repeat` 之一
5. `fromSide`/`toSide` 必须是 `top`、`right`、`bottom`、`left` 之一
6. `fromEnd`/`toEnd` 必须是 `none`、`arrow` 之一
7. 预设颜色必须是 `"1"`-`"6"` 或有效 HEX

---

## 参考

- [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/)
- [JSON Canvas GitHub](https://github.com/obsidianmd/jsoncanvas)
