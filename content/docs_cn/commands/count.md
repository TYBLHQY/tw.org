---
title: "Taskwarrior - 计数命令"
---

# count

`count` 命令仅简单地计新任务数量：

```
$ task count
249
```

默认情况下，所有任务都被计新，包括已完成、已删除和待处理的任务。
如果您只想计新待处理的任务，请添加一个过滤器：

```
$ task status:pending count
32
```

如果[上下文](../../context/)是活跃的，`count` 命令会遵尊它。

## 另请参阅

其他计新任务的方式包括：

- [`_unique`](_unique/) 命令
