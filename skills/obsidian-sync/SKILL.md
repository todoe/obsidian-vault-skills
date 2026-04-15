---
name: "obsidian-sync"
description: "Manage Obsidian Sync: check status, view version history, restore versions, manage sync lifecycle. Invoke when user needs to check sync status, view file history, or restore previous versions."
---

# Obsidian Sync 同步与版本管理

通过 Obsidian CLI 管理 Obsidian Sync 同步功能，实现同步状态查看、版本历史管理和版本恢复。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。需要 Obsidian Sync 订阅。

## 同步控制

### 查看同步状态

```bash
obsidian sync:status
```

返回同步状态和使用情况信息。

### 暂停/恢复同步

```bash
obsidian sync:status off
obsidian sync:status on
```

## 版本历史

### 本地文件历史

#### 列出有本地历史的文件

```bash
obsidian history:list
```

#### 列出文件的版本历史

```bash
obsidian history file="我的笔记"
obsidian history path="Projects/计划.md"
```

#### 读取本地历史版本

```bash
obsidian history:read file="我的笔记" version=1
obsidian history:read path="Projects/计划.md" version=2
```

参数：
- `file=<name>` — 文件名
- `path=<path>` — 文件路径
- `version=<n>` — 版本号（默认：1，即最新版本）

#### 恢复本地历史版本

```bash
obsidian history:restore file="我的笔记" version=3
obsidian history:restore path="Projects/计划.md" version=2
```

`version` 参数为必填。

#### 打开文件恢复界面

```bash
obsidian history:open file="我的笔记"
obsidian history:open path="Projects/计划.md"
```

### 同步版本历史

#### 列出同步版本

```bash
obsidian sync:history file="我的笔记"
obsidian sync:history path="Projects/计划.md"
obsidian sync:history file="我的笔记" total
```

#### 读取同步版本

```bash
obsidian sync:read file="我的笔记" version=1
obsidian sync:read path="Projects/计划.md" version=2
```

`version` 参数为必填。

#### 恢复同步版本

```bash
obsidian sync:restore file="我的笔记" version=2
obsidian sync:restore path="Projects/计划.md" version=1
```

`version` 参数为必填。

#### 打开同步历史界面

```bash
obsidian sync:open file="我的笔记"
obsidian sync:open path="Projects/计划.md"
```

#### 列出同步中已删除的文件

```bash
obsidian sync:deleted
obsidian sync:deleted total
```

## 文件版本对比

### 列出文件的所有版本

```bash
obsidian diff
obsidian diff file="我的笔记"
obsidian diff path="Projects/计划.md"
```

版本从新到旧编号，1 为最新版本。

### 比较版本

```bash
obsidian diff file="我的笔记" from=1
obsidian diff file="我的笔记" from=2 to=1
```

参数：
- `file=<name>` — 文件名
- `path=<path>` — 文件路径
- `from=<n>` — 对比起始版本号
- `to=<n>` — 对比目标版本号
- `filter=local|sync` — 按版本来源筛选

### 仅查看同步版本

```bash
obsidian diff filter=sync
obsidian diff file="我的笔记" filter=sync
```

## 常见工作流

### 检查同步状态

```bash
obsidian sync:status
```

### 查看文件的修改历史

```bash
obsidian diff file="重要文档"
```

### 恢复误删或误改的内容

```bash
obsidian diff file="重要文档" from=1
obsidian history:read file="重要文档" version=2
obsidian history:restore file="重要文档" version=2
```

### 对比两个版本的差异

```bash
obsidian diff file="项目计划" from=3 to=1
```

### 查看同步中已删除的文件

```bash
obsidian sync:deleted
```

### 暂停同步进行批量操作

```bash
obsidian sync:status off
obsidian create name="笔记1" content="内容" silent
obsidian create name="笔记2" content="内容" silent
obsidian create name="笔记3" content="内容" silent
obsidian sync:status on
```

### 跨仓库同步管理

```bash
obsidian vault="工作笔记" sync:status
obsidian vault="个人知识库" sync:history file="重要文档"
```

## 版本恢复策略

1. **先查看差异**：恢复前先用 `diff` 查看版本差异，确认恢复目标
2. **读取确认**：用 `history:read` 或 `sync:read` 读取目标版本内容，确认是需要的版本
3. **再执行恢复**：确认后用 `history:restore` 或 `sync:restore` 恢复
4. **本地 vs 同步**：本地历史保存在本机，同步历史保存在云端；优先使用同步版本（更可靠）
5. **版本编号**：1 为最新版本，数字越大越旧

## 注意事项

- 同步功能需要 Obsidian Sync 订阅
- 本地历史和同步历史是独立的系统
- 恢复操作会覆盖当前文件内容
- `sync:deleted` 列出的文件可能可以恢复
- 批量操作前建议暂停同步，操作完成后再恢复