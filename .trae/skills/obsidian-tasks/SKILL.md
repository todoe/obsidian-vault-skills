---
name: "obsidian-tasks"
description: "Manage Obsidian tasks: list, filter, toggle status, mark done/todo. Invoke when user needs to track tasks, check todo items, or update task status in their vault."
---

# Obsidian Tasks 任务管理

通过 Obsidian CLI 管理任务，实现任务的列出、筛选、状态切换和更新。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。

## 任务语法

Obsidian 使用标准 Markdown 任务列表语法，并扩展了自定义状态：

```markdown
- [ ] 未完成任务
- [x] 已完成任务
- [-] 进行中的任务
- [?] 待确认的任务
- [/] 已取消的任务
```

## 列出任务

### 列出仓库中所有任务

```bash
obsidian tasks
```

### 按完成状态筛选

```bash
obsidian tasks todo
obsidian tasks done
```

### 按文件筛选

```bash
obsidian tasks file="项目计划"
obsidian tasks path="Projects/计划.md"
obsidian tasks file="项目计划" done
obsidian tasks file="项目计划" todo
```

### 查看日记中的任务

```bash
obsidian tasks daily
obsidian tasks daily todo
obsidian tasks daily done
obsidian tasks daily total
```

### 按自定义状态筛选

```bash
obsidian tasks 'status=-'
obsidian tasks 'status=?'
```

### 输出格式

```bash
obsidian tasks format=json
obsidian tasks format=tsv
obsidian tasks format=csv
```

### 详细输出

```bash
obsidian tasks verbose
obsidian tasks daily verbose
```

按文件分组并显示行号。

### 任务计数

```bash
obsidian tasks total
obsidian tasks daily total
obsidian tasks file="项目计划" todo total
```

## 更新任务

### 任务引用方式

任务通过 `文件:行号` 引用：

- `ref=<path:line>` — 任务引用
- `file=<name>` + `line=<n>` — 文件名 + 行号
- `path=<path>` + `line=<n>` — 文件路径 + 行号

### 切换任务状态

```bash
obsidian task ref="项目计划.md:8" toggle
obsidian task file="项目计划" line=8 toggle
```

### 标记为已完成

```bash
obsidian task ref="项目计划.md:8" done
obsidian task file="项目计划" line=8 done
obsidian task daily line=3 done
```

### 标记为未完成

```bash
obsidian task ref="项目计划.md:8" todo
obsidian task file="项目计划" line=8 todo
obsidian task daily line=3 todo
```

### 设置自定义状态

```bash
obsidian task file="项目计划" line=8 status=-
obsidian task file="项目计划" line=8 status=?
obsidian task file="项目计划" line=8 status=/
```

### 查看任务信息

```bash
obsidian task file="项目计划" line=8
obsidian task ref="项目计划.md:8"
```

## 常见工作流

### 每日任务回顾

```bash
obsidian tasks daily
obsidian tasks daily todo
obsidian tasks daily done
```

### 完成日记中的任务

```bash
obsidian tasks daily verbose
obsidian task daily line=3 done
obsidian task daily line=5 done
```

### 查看项目进度

```bash
obsidian tasks file="项目计划" total
obsidian tasks file="项目计划" done total
obsidian tasks file="项目计划" todo total
```

### 查看所有进行中的任务

```bash
obsidian tasks 'status=-'
```

### 跨仓库查看任务

```bash
obsidian vault="工作笔记" tasks todo
obsidian vault="个人知识库" tasks daily
```

### 创建任务到日记

```bash
obsidian daily:append content="- [ ] 完成周报"
obsidian daily:append content="- [ ] 回复客户邮件"
```

### 创建任务到指定笔记

```bash
obsidian append file="项目计划" content="\n- [ ] 新增任务项"
```

## 任务管理最佳实践

1. **日记任务**：日常待办放在日记中，每天自动刷新
2. **项目任务**：项目相关任务放在项目笔记中，便于集中管理
3. **状态流转**：`[ ]` → `[-]` → `[x]`，表示 未开始 → 进行中 → 已完成
4. **定期回顾**：使用 `tasks todo` 定期检查未完成任务
5. **任务计数**：使用 `total` 参数快速了解任务总量和完成率