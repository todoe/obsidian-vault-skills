---
name: "obsidian-daily"
description: "Manage Obsidian daily notes: read, append, prepend content. Invoke when user needs to work with today's daily note, add tasks or entries to daily notes, or read daily note content."
---

# Obsidian Daily Notes 日记系统

通过 Obsidian CLI 管理日记系统，实现日记的读取、追加和前置插入。

前置条件：Obsidian 应用必须处于运行状态，且已启用 CLI。需要在 Obsidian 核心插件中启用「日记（Daily notes）」功能。

## 核心命令

### 打开今日日记

```bash
obsidian daily
```

参数：
- `paneType=tab|split|window` — 打开方式（新标签页/分栏/新窗口）

```bash
obsidian daily paneType=tab
obsidian daily paneType=split
obsidian daily paneType=window
```

### 获取日记路径

即使日记文件尚未创建也会返回预期路径：

```bash
obsidian daily:path
```

### 读取日记内容

```bash
obsidian daily:read
```

### 向日记追加内容

```bash
obsidian daily:append content="- [ ] 买 groceries"
```

参数：
- `content=<text>` — 要追加的内容（必填）
- `inline` — 追加时不换行
- `open` — 添加后打开文件
- `paneType=tab|split|window` — 打开方式

### 在日记开头插入内容

```bash
obsidian daily:prepend content="## 今日重点\n- 完成项目报告"
```

参数：
- `content=<text>` — 要插入的内容（必填）
- `inline` — 插入时不换行
- `open` — 添加后打开文件
- `paneType=tab|split|window` — 打开方式

## 常见工作流

### 晨间日记初始化

```bash
obsidian daily:prepend content="## 今日目标\n- [ ] \n- [ ] \n\n## 日程安排\n- 09:00 \n- 14:00 "
```

### 快速记录想法

```bash
obsidian daily:append content="\n💡 灵感：关于项目架构的新想法"
```

### 添加任务到日记

```bash
obsidian daily:append content="- [ ] 完成周报"
obsidian daily:append content="- [ ] 回复客户邮件"
```

### 记录会议纪要

```bash
obsidian daily:append content="\n## 14:00 项目会议\n- 讨论了 Q2 路线图\n- 下周交付原型\n- [[项目会议纪要]]"
```

### 晚间回顾

```bash
obsidian daily:append content="\n## 晚间回顾\n- 今日完成了 3 个任务\n- 明天重点：完成报告初稿"
```

### 查看今日日记内容

```bash
obsidian daily:read
```

### 跨仓库操作日记

```bash
obsidian vault="工作笔记" daily:read
obsidian vault="个人知识库" daily:append content="- [ ] 阅读 30 分钟"
```

## 与其他 Skill 配合

### 结合任务管理

```bash
obsidian tasks daily
obsidian tasks daily todo
obsidian tasks daily done
```

### 结合属性系统

```bash
obsidian property:set name="mood" value="良好" type=text file=$(obsidian daily:path)
```

### 结合模板系统

先在模板中定义日记模板，然后在日记设置中绑定该模板，新建日记时会自动应用。

```bash
obsidian templates
obsidian template:read name="Daily Template"
```

## 内容格式建议

日记中常用的内容结构：

```markdown
## 今日目标
- [ ] 目标一
- [ ] 目标二

## 日程
- 09:00 晨会
- 14:00 项目评审

## 笔记
灵感与想法记录

## 晚间回顾
- 完成了什么
- 明天重点
```

## 注意事项

- `content` 参数中的 `\n` 表示换行，`\t` 表示制表符
- 值包含空格时需要用引号包裹
- `daily:prepend` 在 frontmatter 之后插入，不会破坏属性
- 日记文件名格式由 Obsidian 日记插件设置决定（默认 YYYY-MM-DD）