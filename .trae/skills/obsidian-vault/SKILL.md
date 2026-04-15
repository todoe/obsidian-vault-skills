---
name: "obsidian-vault"
description: "Manage multiple Obsidian vaults: list, switch, query vault info. Invoke when user needs to work with different vaults, check vault status, or target a specific vault for operations."
---

# Obsidian Vault Management

通过 Obsidian CLI 管理多个仓库，实现仓库切换、信息查询和默认仓库推断。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI（设置 → 常规 → 启用命令行界面）。

## 仓库指定方式

所有 CLI 命令都支持 `vault=<name>` 或 `vault=<id>` 作为第一个参数来指定目标仓库：

```bash
obsidian vault="我的知识库" search query="会议记录"
obsidian vault=Notes daily
```

如果终端当前工作目录是仓库文件夹，则默认使用该仓库。否则使用当前活动仓库。

## 核心命令

### 列出所有已知仓库

```bash
obsidian vaults
```

返回所有已注册的仓库名称和路径。

### 查看当前仓库信息

```bash
obsidian vault
```

返回当前活动仓库的名称、路径等详细信息。

## 多仓库操作模式

### 模式一：命令前缀指定

每条命令前加 `vault=<name>` 参数：

```bash
obsidian vault="工作笔记" search query="项目"
obsidian vault="个人知识库" tags counts
obsidian vault=Research tasks todo
```

### 模式二：工作目录推断

如果终端当前目录是某个仓库的根目录，CLI 自动使用该仓库：

```bash
cd /path/to/my-vault
obsidian search query="关键词"
```

### 模式三：TUI 模式切换

在 TUI 模式中使用 `vault:open` 切换：

```bash
obsidian
vault:open "我的知识库"
```

## 常见工作流

### 跨仓库搜索

```bash
obsidian vault="项目A" search query="架构设计" limit=5
obsidian vault="项目B" search query="架构设计" limit=5
```

### 查看各仓库概览

```bash
obsidian vault="工作笔记" files total
obsidian vault="个人知识库" files total
obsidian vault="学习笔记" tags counts
```

### 在指定仓库创建笔记

```bash
obsidian vault="工作笔记" create name="会议纪要" content="# 会议纪要\n\n日期：2026-04-15" silent
```

## 仓库健康检查

```bash
obsidian vault="我的知识库" orphans total
obsidian vault="我的知识库" deadends total
obsidian vault="我的知识库" unresolved total
```

## 注意事项

- `vault=<name>` 必须是命令的第一个参数
- 仓库名称包含空格时需要用引号包裹
- CLI 需要 Obsidian 应用处于运行状态
- 仓库必须已在 Obsidian 中注册（通过打开过一次来注册）