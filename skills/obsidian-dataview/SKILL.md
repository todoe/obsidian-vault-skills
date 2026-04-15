---
name: "obsidian-dataview"
description: "Execute Dataview queries in Obsidian via CLI eval. Requires Dataview plugin installed. Invoke when user needs advanced data queries, cross-note aggregation, or structured data extraction beyond basic search."
---

# Obsidian Dataview 数据查询

通过 Obsidian CLI 的 `eval` 命令调用 Dataview 插件 API，实现高级数据查询和跨笔记聚合。

⚠️ **前置条件**：
- Obsidian 应用必须处于运行状态，且已启用 CLI
- **必须安装并启用 Dataview 社区插件**（设置 → 社区插件 → 浏览 → 搜索 Dataview → 安装并启用）

## 检测 Dataview 是否可用

在使用任何 Dataview 查询前，先检测插件是否已安装：

```bash
obsidian eval code="app.plugins.plugins.dataview ? '已安装' : '未安装'"
```

如果返回「未安装」，需要先安装 Dataview 插件后才能使用本技能。

## 查询原理

Dataview 插件暴露了 JavaScript API，通过 `obsidian eval` 命令在 Obsidian 应用上下文中执行 JS 代码来调用：

```bash
obsidian eval code="<Dataview JS 代码>"
```

返回结果需要用 `JSON.stringify()` 序列化，否则得到的是 Obsidian 内部对象。

## 常用查询模板

### 按标签查询笔记

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('#项目').map(p=>p.file.name))"
```

### 按文件夹查询笔记

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('"Daily"').map(p=>p.file.name))"
```

### 最近修改的笔记

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').sort(p=>p.file.mtime,'desc').limit(5).map(p=>({name:p.file.name,mtime:p.file.mtime})))"
```

### 孤立笔记（无入链无出链）

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').where(p=>p.file.inlinks.length===0&&p.file.outlinks.length===0).map(p=>p.file.name))"
```

### 按属性值筛选

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').where(p=>p.status==='进行中').map(p=>({name:p.file.name,status:p.status})))"
```

### 未完成任务汇总

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').file.tasks.where(t=>!t.completed).map(t=>({text:t.text,path:t.path})))"
```

### 笔记统计

```bash
obsidian eval code="JSON.stringify({total:app.plugins.plugins.dataview.api.pages('').length})"
```

### 按标签统计笔记数量

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').groupBy(p=>p.file.tags).map(g=>({tag:g.key,count:g.rows.length})))"
```

### 随机发现笔记

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').sort(()=>Math.random()).limit(5).map(p=>p.file.name))"
```

### 查找重名笔记

```bash
obsidian eval code="JSON.stringify((()=>{const groups=app.plugins.plugins.dataview.api.pages('').groupBy(p=>p.file.name.toLowerCase());return groups.filter(g=>g.rows.length>1).map(g=>({name:g.key,paths:g.rows.map(r=>r.file.path)}))})())"
```

### 按创建时间分组

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('').groupBy(p=>p.file.ctime.toFormat('yyyy-MM')).map(g=>({month:g.key,count:g.rows.length})))"
```

## DQL 查询语法参考

Dataview 支持自己的查询语言（DQL），可在笔记中直接使用：

### TABLE 查询

```dataview
TABLE file.ctime AS "创建时间", status AS "状态"
FROM #项目
SORT file.mtime DESC
```

### LIST 查询

```dataview
LIST FROM #项目/进行中
SORT file.name ASC
```

### TASK 查询

```dataview
TASK FROM #项目
WHERE !completed
SORT file.name ASC
```

### CALENDAR 查询

```dataview
CALENDAR file.ctime
FROM "Daily"
```

## DataviewJS 查询

更强大的 JavaScript 查询方式，可在笔记中使用：

```dataviewjs
dv.table(["笔记", "状态", "修改时间"],
  dv.pages("#项目")
    .sort(p => p.file.mtime, "desc")
    .map(p => [p.file.link, p.status, p.file.mtime])
)
```

## 内联表达式

在笔记中直接嵌入动态值：

```markdown
当前笔记名：`= this.file.name`
修改时间：`= this.file.mtime`
标签数量：`= length(this.file.tags)`
```

## 常见工作流

### 项目进度总览

```bash
obsidian eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('#项目').map(p=>({name:p.file.name,status:p.status||'未设置',mtime:p.file.mtime,inlinks:p.file.inlinks.length})))"
```

### 知识库健康报告

```bash
obsidian eval code="JSON.stringify({total:app.plugins.plugins.dataview.api.pages('').length,orphans:app.plugins.plugins.dataview.api.pages('').where(p=>p.file.inlinks.length===0).length,deadends:app.plugins.plugins.dataview.api.pages('').where(p=>p.file.outlinks.length===0).length,tags:app.plugins.plugins.dataview.api.pages('').flatMap(p=>p.file.tags).distinct().length})"
```

### 跨仓库查询

```bash
obsidian vault="工作笔记" eval code="JSON.stringify(app.plugins.plugins.dataview.api.pages('#项目').map(p=>p.file.name))"
```

## 与 CLI 原生命令的对比

| 需求 | CLI 原生命令 | Dataview 查询 |
|------|-------------|---------------|
| 搜索笔记 | `search query=` | 按属性/标签/文件夹精确筛选 |
| 孤立笔记 | `orphans` | 更灵活（可加额外条件） |
| 标签统计 | `tags counts` | 按标签分组聚合 |
| 任务列表 | `tasks todo` | 按属性/标签筛选任务 |
| 属性查询 | `properties` | 按属性值筛选笔记 |
| 笔记统计 | `files total` | 按时间/标签/属性分组统计 |

**原则**：能用 CLI 原生命令完成的优先用原生命令（更快更稳定），Dataview 用于原生命令无法满足的复杂查询场景。

## 注意事项

1. **插件依赖**：Dataview 是社区插件，版本更新可能导致 API 变化
2. **性能**：大型仓库的复杂查询可能较慢，建议加 `limit` 限制结果数量
3. **序列化**：返回结果必须用 `JSON.stringify()` 包裹，否则得到内部对象
4. **引号转义**：`eval code=` 中的引号需要正确转义
5. **只读安全**：DQL 查询是沙盒化的，不会修改文件；DataviewJS 可以修改文件，需谨慎
6. **中文属性**：查询中文属性名时确保编码正确