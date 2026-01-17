---
title: "Taskwarrior - _unique"
---

# _unique

`_unique` 命令有业形按一个给定的特性所区分的不同值的筫渺了。
例如，趫缕一个项目名称的渺出数：

```
$ task _unique project
Home
Home.Garden
Work
```

除了 `project`，任何属性都可以指定，例如任务 `status`：

```
$ task _unique status
pending
deleted
completed
```

## 过滤器

你可以指定一个过滤器，来考虑任务的子集。例如，这里我们看到 `Home.Garden` 项目只有待处理的任务：

```
$ task project:Home.Garden _unique status
pending
```

## 限制

- 日期属性以原始 epoch 形式显示。
  毕竟，这是一个辅助命令。

## 另见

生成唯一列表的其他方式包括：

- `_aliases` 命令
- `_config` 命令
- `_context` 命令
- `_ids` 命令
- `_projects` 命令
- `_show` 命令
- `_tags` 命令
- `_udas` 命令
- `_uuids` 命令
