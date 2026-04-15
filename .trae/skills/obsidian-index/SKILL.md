---
name: "obsidian-index"
description: "Master index for Obsidian skills suite. Routes to the right skill based on user intent. Invoke FIRST when user mentions Obsidian, vault, notes, links, tags, tasks, daily notes, templates, properties, bookmarks, bases, or sync."
---

# Obsidian Skills 技能索引

本索引帮助你快速定位到正确的子技能。根据用户意图选择对应的技能，无需加载全部内容。

前置条件：Obsidian 1.12+ 应用运行中，CLI 已启用（设置 → 常规 → 启用命令行界面）。

## 技能路由表

根据用户意图，选择对应的技能：

| 用户意图 | 技能 | 说明 |
|----------|------|------|
| 管理多个仓库、切换仓库 | `obsidian-vault` | 多仓库管理、仓库切换、仓库信息 |
| 创建/读取/编辑/删除/搜索笔记 | `obsidian-notes` | 笔记 CRUD、搜索、大纲、文件管理 |
| 双向链接、反向链接、孤立笔记、断链 | `obsidian-links` | 链接分析、知识图谱健康度 |
| 生成 Obsidian 风格的笔记内容 | `obsidian-markdown` | Wikilinks、Callouts、Embeds、Properties、标签等语法 |
| 日记、今日笔记 | `obsidian-daily` | 日记读取、追加、前置插入 |
| 任务管理、待办事项 | `obsidian-tasks` | 任务列表、状态切换、筛选 |
| 属性/元数据/Frontmatter | `obsidian-properties` | 属性读取/设置/删除、别名管理 |
| 模板、从模板创建 | `obsidian-templates` | 模板列表/读取、基于模板创建笔记 |
| 标签、标签统计、嵌套标签 | `obsidian-tags` | 标签列表、统计、层级分析 |
| 书签、收藏 | `obsidian-bookmarks` | 书签列表/添加 |
| 数据库、Bases、.base 文件 | `obsidian-bases` | 数据库查询、视图、创建项目 |
| 同步、版本历史、恢复 | `obsidian-sync` | 同步状态、版本对比、版本恢复 |

## 快速判断流程

```
用户提到 Obsidian
  │
  ├─ 涉及多个仓库？ → obsidian-vault
  │
  ├─ 要写/读/改/删笔记？ → obsidian-notes
  │
  ├─ 要生成笔记内容？ → obsidian-markdown
  │
  ├─ 要查链接关系？ → obsidian-links
  │
  ├─ 提到日记/今天？ → obsidian-daily
  │
  ├─ 提到任务/待办？ → obsidian-tasks
  │
  ├─ 提到属性/元数据？ → obsidian-properties
  │
  ├─ 提到模板？ → obsidian-templates
  │
  ├─ 提到标签？ → obsidian-tags
  │
  ├─ 提到书签/收藏？ → obsidian-bookmarks
  │
  ├─ 提到数据库/Bases？ → obsidian-bases
  │
  └─ 提到同步/版本/恢复？ → obsidian-sync
```

## 跨技能常见工作流

### 工作流 1：新建一篇完整笔记

1. `obsidian-templates` — 检查是否有合适模板
2. `obsidian-markdown` — 按 Obsidian 规范生成内容
3. `obsidian-notes` — 创建笔记文件
4. `obsidian-properties` — 设置属性
5. `obsidian-links` — 搜索相关笔记，建议建立双向链接

### 工作流 2：每日工作流

1. `obsidian-daily` — 读取今日日记
2. `obsidian-tasks` — 查看今日待办
3. `obsidian-daily` — 追加新任务或记录
4. `obsidian-tasks` — 完成任务后切换状态

### 工作流 3：知识库健康检查

1. `obsidian-links` — 检测孤立笔记、死端、断链
2. `obsidian-tags` — 检查标签分布和低频标签
3. `obsidian-properties` — 检查属性一致性
4. `obsidian-notes` — 对问题笔记进行修复

### 工作流 4：项目笔记初始化

1. `obsidian-templates` — 选择项目模板
2. `obsidian-markdown` — 生成项目笔记内容
3. `obsidian-notes` — 创建笔记
4. `obsidian-properties` — 设置项目属性（状态、优先级等）
5. `obsidian-tags` — 添加项目标签
6. `obsidian-links` — 关联到已有项目笔记

### 工作流 5：版本恢复

1. `obsidian-sync` — 查看文件版本历史
2. `obsidian-sync` — 对比版本差异
3. `obsidian-sync` — 恢复目标版本

## 通用 CLI 语法

所有技能都基于 Obsidian CLI，通用语法：

```bash
# 基本格式
obsidian <command> parameter=value flag

# 指定仓库
obsidian vault="仓库名" <command>

# 指定文件
obsidian <command> file="文件名"      # Wikilink 解析
obsidian <command> path="路径/文件.md"  # 完整路径

# 复制输出到剪贴板
obsidian <command> --copy

# 多行内容
obsidian <command> content="第一行\n第二行\n第三行"
```

## 优先级说明

| 优先级 | 技能 | 状态 |
|--------|------|------|
| P0 | obsidian-vault, obsidian-notes, obsidian-links, obsidian-markdown | ✅ 已完成 |
| P1 | obsidian-daily, obsidian-tasks, obsidian-properties, obsidian-templates | ✅ 已完成 |
| P2 | obsidian-tags, obsidian-bookmarks, obsidian-bases, obsidian-sync | ✅ 已完成 |
| P3 | obsidian-dataview | 📋 计划中 |