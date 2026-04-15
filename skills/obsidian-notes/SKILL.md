---
name: "obsidian-notes"
description: "Create, read, update, delete, search, and manage Obsidian notes via CLI. Invoke when user needs to write, edit, find, or organize notes in their vault."
---

# Obsidian Notes Management

通过 Obsidian CLI 实现笔记的完整生命周期管理：创建、读取、编辑、删除、搜索和文件组织。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。

## 文件指定方式

多数命令接受 `file` 或 `path` 参数指定文件，两者都不提供时默认作用于活动文件：

- `file=<name>` — 类似 Wikilink 解析，只需文件名，无需路径和扩展名
- `path=<path>` — 从仓库根目录开始的完整路径，如 `folder/note.md`

```bash
obsidian read file=Recipe
obsidian read path="Templates/Recipe.md"
```

## 笔记创建

### 创建新笔记

```bash
obsidian create name="笔记标题" content="# 标题\n\n正文内容" silent
```

参数说明：
- `name=<name>` — 文件名
- `path=<path>` — 文件路径（与 name 二选一）
- `content=<text>` — 初始内容，`\n` 换行，`\t` 制表符
- `template=<name>` — 使用的模板名
- `silent` — 创建后不打开文件
- `overwrite` — 文件存在时覆盖
- `open` — 创建后打开文件
- `newtab` — 在新标签页打开

### 基于模板创建

```bash
obsidian create name="读书笔记" template="Book Template" silent
```

## 笔记读取

### 读取文件内容

```bash
obsidian read file="我的笔记"
obsidian read path="Daily/2026-04-15.md"
```

### 读取文件信息

```bash
obsidian file file=Recipe
```

返回：path、name、extension、size、created、modified

### 读取文件大纲

```bash
obsidian outline file="我的笔记" format=tree
obsidian outline file="我的笔记" format=md
obsidian outline file="我的笔记" format=json
```

## 笔记编辑

### 追加内容

```bash
obsidian append file="我的笔记" content="\n追加的新内容"
obsidian append file="我的笔记" content="同行追加" inline
```

### 在 Frontmatter 之后插入内容

```bash
obsidian prepend file="我的笔记" content="插入到开头的内容"
obsidian prepend file="我的笔记" content="同行插入" inline
```

## 笔记移动与重命名

### 移动文件

```bash
obsidian move file="我的笔记" to="Archive/"
```

移动时如果仓库设置中启用了相关选项，将自动更新内部链接。

### 重命名文件

```bash
obsidian rename file="旧名称" name="新名称"
```

省略扩展名时自动保留原扩展名。

## 笔记删除

```bash
obsidian delete file="待删除笔记"
obsidian delete file="待删除笔记" permanent
```

不加 `permanent` 时默认移至回收站。

## 搜索

### 全文搜索

```bash
obsidian search query="关键词" limit=10
obsidian search query="项目计划" path="Projects" format=json
```

参数：
- `query=<text>` — 搜索查询（必填）
- `path=<folder>` — 限定在某个文件夹内
- `limit=<n>` — 最大返回文件数
- `format=text|json` — 输出格式
- `total` — 返回匹配数量
- `case` — 区分大小写

### 带上下文的搜索

```bash
obsidian search:context query="TODO" limit=5
```

返回 grep 风格的 `path:line: text` 输出。

## 文件与文件夹列表

### 列出文件

```bash
obsidian files
obsidian files folder="Projects" ext=md
obsidian files total
```

### 列出文件夹

```bash
obsidian folders
obsidian folders folder="Projects"
obsidian folders total
```

### 文件夹信息

```bash
obsidian folder path="Projects" info=files
obsidian folder path="Projects" info=size
```

## 打开文件

```bash
obsidian open file="我的笔记"
obsidian open file="我的笔记" newtab
```

## 常见工作流

### 快速捕获想法

```bash
obsidian create name="闪念" content="# 闪念\n\n在地铁上想到的一个点子..." silent
```

### 将笔记归档

```bash
obsidian move file="已完成项目" to="Archive/已完成项目"
```

### 批量搜索并读取

```bash
obsidian search query="架构设计" limit=5
obsidian read file="搜索结果中的笔记"
```

### 查看笔记结构

```bash
obsidian outline file="长文笔记" format=md
```

## 通用技巧

- 任何命令后加 `--copy` 可将输出复制到剪贴板
- 列表命令加 `total` 可返回数量而非完整列表
- 创建笔记时加 `silent` 防止自动打开文件
- 多行内容使用 `\n` 换行、`\t` 制表符