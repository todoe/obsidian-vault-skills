---
name: "obsidian-tags"
description: "Manage Obsidian tags: list, count, analyze tag hierarchy and usage. Invoke when user needs to view tags, check tag distribution, or organize tags in their vault."
---

# Obsidian Tags 标签系统

通过 Obsidian CLI 管理标签系统，实现标签的列出、统计、层级分析和标签管理。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。

## 标签语法

Obsidian 支持两种标签定义方式：

### 行内标签

```markdown
这是一个 #项目 标签
这是嵌套标签 #项目/进行中
这是更深层级 #项目/进行中/前端
```

### Frontmatter 标签

```yaml
---
tags:
  - 项目
  - 进行中
---
```

### 标签规则

- 可包含字母、数字（不能作为首字符）、下划线、连字符、斜杠
- 嵌套标签用 `/` 分隔，形成层级结构
- 行内标签和 Frontmatter 标签都会被 Obsidian 索引
- Text 属性中的 `#标签` 不会创建标签

## 核心命令

### 列出仓库中的标签

```bash
obsidian tags
obsidian tags counts
obsidian tags sort=count
obsidian tags total
obsidian tags format=json
obsidian tags format=tsv
obsidian tags format=csv
```

参数：
- `sort=count` — 按数量排序（默认按名称排序）
- `counts` — 包含每个标签的使用次数
- `total` — 返回标签总数
- `format=json|tsv|csv` — 输出格式

### 查看指定文件的标签

```bash
obsidian tags file="我的笔记"
obsidian tags path="Projects/计划.md"
obsidian tags active
```

### 查看标签详情

```bash
obsidian tag name=项目
obsidian tag name=项目 total
obsidian tag name=项目 verbose
```

参数：
- `name=<tag>` — 标签名称（必填）
- `total` — 返回出现次数
- `verbose` — 包含文件列表和数量

### 查看嵌套标签

```bash
obsidian tag name=项目 verbose
obsidian tag name=项目/进行中 verbose
```

## 常见工作流

### 查看标签分布

```bash
obsidian tags counts sort=count
```

返回按使用次数排序的标签列表，快速了解知识库的主题分布。

### 发现低频标签

```bash
obsidian tags counts sort=count
```

查看使用次数最少的标签，判断是否需要合并或补充相关笔记。

### 查看某主题下的所有笔记

```bash
obsidian tag name=项目 verbose
```

返回所有带有 `#项目` 标签的文件列表。

### 分析标签层级

```bash
obsidian tag name=项目 verbose
obsidian tag name=项目/进行中 verbose
obsidian tag name=项目/已完成 verbose
```

逐级查看嵌套标签下的笔记分布。

### 检查笔记的标签覆盖

```bash
obsidian tags file="项目计划"
```

查看某篇笔记使用了哪些标签，判断是否需要补充。

### 为笔记添加标签

通过属性系统添加 Frontmatter 标签：

```bash
obsidian property:set name="tags" value="项目" type=text file="项目计划"
```

或通过追加内容添加行内标签：

```bash
obsidian append file="项目计划" content="\n#项目/进行中"
```

### 跨仓库标签分析

```bash
obsidian vault="工作笔记" tags counts sort=count
obsidian vault="个人知识库" tags counts sort=count
```

## 标签管理最佳实践

1. **层级设计**：使用嵌套标签建立分类体系，如 `#项目/进行中`、`#项目/已完成`
2. **命名规范**：统一使用小写、连字符分隔，如 `#读书笔记` 而非 `#读书笔记/未完成`
3. **适度使用**：每篇笔记 3-5 个标签为宜，过多标签反而降低检索效率
4. **定期清理**：使用 `tags counts sort=count` 检查低频标签，合并或删除
5. **与链接配合**：标签用于分类，`[[链接]]` 用于具体关联，两者互补
6. **避免重复**：同一概念不要同时用标签和链接表达，选择一种方式坚持使用

## 标签与链接的区别

| 维度 | 标签 `#tag` | 链接 `[[note]]` |
|------|------------|----------------|
| 用途 | 分类归档 | 具体关联 |
| 粒度 | 粗（主题级） | 细（笔记级） |
| 方向 | 单向标记 | 双向连接 |
| 检索 | 按主题筛选 | 按关系追踪 |
| 适合 | 分类、筛选 | 知识图谱构建 |