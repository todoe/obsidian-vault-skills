---
name: "obsidian-bases"
description: "Query and manage Obsidian Bases: list, query views, create items. Invoke when user needs to work with Obsidian database files (.base), query structured data, or create items in bases."
---

# Obsidian Bases 数据库系统

通过 Obsidian CLI 管理 Obsidian Bases（数据库），实现数据库的列出、查询和创建操作。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。Bases 是 Obsidian 1.7+ 的核心功能。

## 核心概念

Obsidian Bases 是结构化数据管理功能，以 `.base` 文件存储，每个数据库可包含多个视图（View），视图支持筛选、排序、公式和汇总。

## 核心命令

### 列出数据库

```bash
obsidian bases
```

返回仓库中所有 `.base` 文件列表。

### 列出数据库视图

```bash
obsidian base:views
obsidian base:views file="项目追踪"
obsidian base:views path="Databases/项目追踪.base"
```

返回当前/指定数据库文件中的视图列表。

### 查询数据库

```bash
obsidian base:query
obsidian base:query file="项目追踪"
obsidian base:query path="Databases/项目追踪.base"
obsidian base:query file="项目追踪" view="进行中"
```

参数：
- `file=<name>` — 数据库文件名
- `path=<path>` — 数据库文件路径
- `view=<name>` — 要查询的视图名称
- `format=json|csv|tsv|md|paths` — 输出格式（默认：json）

### 创建数据库项

```bash
obsidian base:create
obsidian base:create file="项目追踪" name="新项目" content="项目描述"
obsidian base:create path="Databases/项目追踪.base" view="进行中" name="新项目"
```

参数：
- `file=<name>` — 数据库文件名
- `path=<path>` — 数据库文件路径
- `view=<name>` — 视图名称
- `name=<name>` — 新项目名称
- `content=<text>` — 初始内容
- `open` — 创建后打开文件
- `newtab` — 在新标签页打开

如果未指定文件，默认使用活动的数据库视图。

## 常见工作流

### 查看所有数据库

```bash
obsidian bases
```

### 查询数据库数据

```bash
obsidian base:query file="项目追踪" format=json
obsidian base:query file="项目追踪" format=md
obsidian base:query file="项目追踪" format=csv
```

### 按视图筛选数据

```bash
obsidian base:views file="项目追踪"
obsidian base:query file="项目追踪" view="进行中"
obsidian base:query file="项目追踪" view="已完成"
```

### 向数据库添加新项目

```bash
obsidian base:create file="项目追踪" name="新项目 Alpha" content="项目描述内容"
```

### 导出数据库数据

```bash
obsidian base:query file="项目追踪" format=csv
obsidian base:query file="项目追踪" format=md
```

### 跨仓库数据库操作

```bash
obsidian vault="工作笔记" bases
obsidian vault="工作笔记" base:query file="项目追踪" format=json
```

## 输出格式说明

| 格式 | 说明 | 适用场景 |
|------|------|----------|
| `json` | JSON 结构化数据 | 程序处理、API 集成 |
| `csv` | 逗号分隔值 | Excel 导入、数据分析 |
| `tsv` | 制表符分隔值 | 终端展示、粘贴到表格 |
| `md` | Markdown 表格 | 笔记引用、文档生成 |
| `paths` | 文件路径列表 | 批量文件操作 |

## 注意事项

- Bases 是 Obsidian 较新的功能，CLI 支持可能随版本更新变化
- 查询大型数据库时建议指定 `view` 参数缩小范围
- `base:create` 创建的是数据库中的项目，不是独立的 `.base` 文件
- 数据库文件路径需要包含 `.base` 扩展名