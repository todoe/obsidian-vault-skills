---
name: "obsidian-links"
description: "Analyze Obsidian bidirectional links: backlinks, outgoing links, orphans, deadends, unresolved links. Invoke when user needs to understand note relationships, find isolated notes, or check link health."
---

# Obsidian Links & Knowledge Graph Analysis

通过 Obsidian CLI 分析笔记间的双向链接关系，检测知识孤岛、死端笔记和断链，维护知识图谱的健康度。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。

## 核心概念

Obsidian 的双向链接系统是知识管理的灵魂：
- **出链（Outgoing Links）**：当前笔记中 `[[链接]]` 指向的其他笔记
- **反向链接（Backlinks）**：其他笔记中 `[[当前笔记]]` 指向当前笔记的链接
- **孤立笔记（Orphans）**：没有任何入链的笔记，知识孤岛
- **死端笔记（Deadends）**：没有任何出链的笔记，缺乏延伸
- **未解析链接（Unresolved）**：指向不存在笔记的链接，断链

## 反向链接

### 查看反向链接

```bash
obsidian backlinks file="目标笔记"
```

返回所有链接到该笔记的文件列表。

### 反向链接统计

```bash
obsidian backlinks file="目标笔记" counts
obsidian backlinks file="目标笔记" total
```

### 指定路径查询

```bash
obsidian backlinks path="Projects/架构设计.md"
```

### 输出格式

```bash
obsidian backlinks file="目标笔记" format=json
obsidian backlinks file="目标笔记" format=tsv
obsidian backlinks file="目标笔记" format=csv
```

## 出链

### 查看出链

```bash
obsidian links file="源笔记"
```

返回该笔记中所有 `[[链接]]` 指向的目标。

### 出链数量

```bash
obsidian links file="源笔记" total
```

## 知识图谱健康分析

### 孤立笔记检测

找出没有任何入链的笔记——这些笔记在知识图谱中是孤岛，无法被其他笔记发现：

```bash
obsidian orphans
obsidian orphans total
```

**建议操作**：为孤立笔记建立双向链接，将其接入知识网络。

### 死端笔记检测

找出没有任何出链的笔记——这些笔记缺乏延伸阅读，是知识图谱的末端：

```bash
obsidian deadends
obsidian deadends total
```

**建议操作**：在死端笔记中添加 `[[相关笔记]]` 链接，延伸知识路径。

### 断链检测

找出指向不存在笔记的链接——这些链接会显示为灰色，点击后无法跳转：

```bash
obsidian unresolved
obsidian unresolved total
obsidian unresolved counts
obsidian unresolved verbose
```

参数：
- `total` — 返回未解析链接数量
- `counts` — 包含每个断链的出现次数
- `verbose` — 包含源文件信息
- `format=json|tsv|csv` — 输出格式

**建议操作**：创建断链指向的笔记，或修正链接名称。

## 链接分析工作流

### 全面健康检查

```bash
obsidian orphans total
obsidian deadends total
obsidian unresolved total
```

### 深度分析某篇笔记的链接关系

```bash
obsidian backlinks file="核心笔记" counts
obsidian links file="核心笔记" total
```

### 发现知识孤岛并修复

```bash
obsidian orphans
obsidian append file="孤立笔记" content="\n相关：[[已有笔记]]"
```

### 发现断链并创建目标笔记

```bash
obsidian unresolved verbose
obsidian create name="断链指向的笔记" content="# 笔记内容" silent
```

### 分析笔记的链接深度

```bash
obsidian links file="起点笔记"
obsidian backlinks file="起点笔记"
```

## 链接优化建议

当发现以下情况时，应主动建议用户优化：

1. **孤立笔记过多** → 建议建立 MOC（内容地图）笔记，用 `[[链接]]` 将相关笔记串联
2. **死端笔记过多** → 建议在笔记末尾添加「相关笔记」章节
3. **断链过多** → 建议批量创建缺失笔记或修正链接名称
4. **某笔记入链极多** → 该笔记是枢纽节点，应确保内容质量
5. **某笔记出链极多** → 该笔记可能是 MOC 或索引笔记

## 跨仓库链接分析

```bash
obsidian vault="工作笔记" orphans total
obsidian vault="个人知识库" orphans total
obsidian vault="工作笔记" unresolved total
```