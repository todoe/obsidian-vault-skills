---
name: "obsidian-bookmarks"
description: "Manage Obsidian bookmarks: list, add bookmarks for files, folders, searches, and URLs. Invoke when user needs to view or manage bookmarks in their vault."
---

# Obsidian Bookmarks 书签管理

通过 Obsidian CLI 管理书签系统，实现书签的列出和添加。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。需要在 Obsidian 核心插件中启用「书签（Bookmarks）」功能。

## 核心命令

### 列出书签

```bash
obsidian bookmarks
obsidian bookmarks total
obsidian bookmarks verbose
obsidian bookmarks format=json
obsidian bookmarks format=tsv
obsidian bookmarks format=csv
```

参数：
- `total` — 返回书签数量
- `verbose` — 包含书签类型
- `format=json|tsv|csv` — 输出格式

### 添加书签

支持为多种内容类型添加书签：

#### 为文件添加书签

```bash
obsidian bookmark file="Projects/计划.md"
```

#### 为文件夹添加书签

```bash
obsidian bookmark folder="Projects"
```

#### 为搜索添加书签

```bash
obsidian bookmark search="项目计划"
```

#### 为外部 URL 添加书签

```bash
obsidian bookmark url="https://obsidian.md" title="Obsidian 官网"
```

#### 为文件内标题/块添加书签

```bash
obsidian bookmark file="Projects/计划.md" subpath="#里程碑"
```

参数：
- `file=<path>` — 要添加书签的文件
- `subpath=<subpath>` — 文件内的子路径（标题或块）
- `folder=<path>` — 要添加书签的文件夹
- `search=<query>` — 要添加书签的搜索查询
- `url=<url>` — 要添加书签的 URL
- `title=<title>` — 书签标题

## 常见工作流

### 查看当前书签

```bash
obsidian bookmarks
obsidian bookmarks verbose
```

### 快速收藏重要笔记

```bash
obsidian bookmark file="Projects/核心计划.md"
obsidian bookmark file="参考资料/常用链接.md"
```

### 收藏常用搜索

```bash
obsidian bookmark search="TODO"
obsidian bookmark search="项目/进行中"
```

### 收藏常用文件夹

```bash
obsidian bookmark folder="Daily"
obsidian bookmark folder="Projects"
```

### 收藏外部资源

```bash
obsidian bookmark url="https://docs.obsidian.md" title="Obsidian 文档"
obsidian bookmark url="https://github.com" title="GitHub"
```

### 收藏笔记中的关键章节

```bash
obsidian bookmark file="项目总览.md" subpath="#里程碑"
obsidian bookmark file="会议纪要.md" subpath="#行动项"
```

### 跨仓库书签管理

```bash
obsidian vault="工作笔记" bookmarks
obsidian vault="工作笔记" bookmark file="重要文档.md"
```

## 书签使用建议

1. **常用笔记**：将频繁访问的笔记添加为书签，快速跳转
2. **搜索书签**：将常用搜索条件保存为书签，一键执行
3. **文件夹书签**：将常用文件夹添加为书签，快速导航
4. **外部链接**：将与知识库相关的外部资源收藏为书签
5. **定期整理**：定期检查书签列表，移除不再需要的书签
6. **与日记配合**：将日记文件夹添加为书签，每天快速打开