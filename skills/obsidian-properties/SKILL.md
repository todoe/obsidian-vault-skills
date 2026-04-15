---
name: "obsidian-properties"
description: "Manage Obsidian note properties and metadata: read, set, remove frontmatter fields and aliases. Invoke when user needs to view or edit YAML frontmatter, manage aliases, or work with note metadata."
---

# Obsidian Properties 属性与元数据管理

通过 Obsidian CLI 管理笔记的属性（Frontmatter），实现属性的读取、设置、删除和别名管理。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。

## 属性概述

属性存储在笔记顶部的 YAML 格式中：

```yaml
---
title: 我的笔记
date: 2026-04-15
tags:
  - 项目
  - 进行中
aliases:
  - 备用名称
status: 进行中
priority: 1
favorite: true
---
```

### 属性类型

| 类型 | 格式 | 示例 |
|------|------|------|
| Text | 单行文本 | `title: 笔记标题` |
| List | 多值列表 | `tags: [项目, 进行中]` |
| Number | 数字 | `priority: 1` |
| Checkbox | 布尔值 | `favorite: true` |
| Date | 日期 | `date: 2026-04-15` |
| Date & time | 日期时间 | `created: 2026-04-15T10:30:00` |
| Tags | 标签列表 | `tags: [项目/进行中]` |

### 默认属性

- `tags` — 可搜索的标签
- `aliases` — 笔记的备用名称，用于链接建议
- `cssclasses` — CSS 类名，用于自定义样式

## 读取属性

### 列出仓库中的所有属性

```bash
obsidian properties
obsidian properties sort=count
obsidian properties format=yaml
obsidian properties format=json
obsidian properties total
obsidian properties counts
```

### 查看指定文件的属性

```bash
obsidian properties file="我的笔记"
obsidian properties path="Projects/计划.md"
obsidian properties active
```

### 查看特定属性的使用情况

```bash
obsidian properties name=status
obsidian properties name=tags sort=count
```

### 读取单个属性值

```bash
obsidian property:read name="status" file="我的笔记"
obsidian property:read name="tags" path="Projects/计划.md"
obsidian property:read name="title" active
```

## 设置属性

```bash
obsidian property:set name="status" value="已完成" type=text file="我的笔记"
```

参数：
- `name=<name>` — 属性名称（必填）
- `value=<value>` — 属性值（必填）
- `type=text|list|number|checkbox|date|datetime` — 属性类型
- `file=<name>` — 文件名
- `path=<path>` — 文件路径

### 按类型设置属性

```bash
obsidian property:set name="title" value="项目计划" type=text file="项目"
obsidian property:set name="priority" value=1 type=number file="项目"
obsidian property:set name="favorite" value=true type=checkbox file="项目"
obsidian property:set name="date" value=2026-04-15 type=date file="项目"
obsidian property:set name="status" value="进行中" type=text file="项目"
```

### 设置列表属性

列表属性需要逐个添加，或通过创建内容时在 frontmatter 中定义：

```bash
obsidian property:set name="tags" value="项目" type=text file="项目"
```

## 删除属性

```bash
obsidian property:remove name="status" file="我的笔记"
obsidian property:remove name="priority" path="Projects/计划.md"
```

## 别名管理

### 列出仓库中的别名

```bash
obsidian aliases
obsidian aliases total
obsidian aliases verbose
```

### 查看指定文件的别名

```bash
obsidian aliases file="我的笔记"
obsidian aliases active
```

## 常见工作流

### 为新笔记添加元数据

```bash
obsidian property:set name="title" value="会议纪要" type=text file="会议纪要"
obsidian property:set name="date" value=2026-04-15 type=date file="会议纪要"
obsidian property:set name="status" value="草稿" type=text file="会议纪要"
```

### 批量查看笔记状态

```bash
obsidian properties name=status sort=count
```

### 更新笔记状态

```bash
obsidian property:read name="status" file="项目计划"
obsidian property:set name="status" value="已完成" type=text file="项目计划"
```

### 查看所有标签属性

```bash
obsidian properties name=tags sort=count counts
```

### 跨仓库属性管理

```bash
obsidian vault="工作笔记" properties name=status sort=count
obsidian vault="个人知识库" property:set name="status" value="归档" type=text file="旧笔记"
```

## 注意事项

- 属性名在同一笔记中必须唯一
- Text 属性中的 `[[链接]]` 必须用引号包裹：`link: "[[相关笔记]]"`
- 属性不支持 Markdown 格式渲染
- 设置属性时指定 `type` 可确保 Obsidian 正确识别属性类型
- 不指定 `file` 或 `path` 时默认作用于活动文件