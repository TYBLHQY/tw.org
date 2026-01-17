---
title: "Taskwarrior - 30秒教程"
---

# 30秒教程

让我们开始吧。
这是一个快速演示，展示如何执行基本的任务管理。

下面是对正在发生什么的说明。
首先添加两个任务。

```
$ task add Read Taskwarrior documents later
Created task 1.

$ task add priority:H Pay bills
Created task 2.
```

容易。
你看到第二个有高优先级吗？现在使用 `next` 报告查看这些任务。
注意这两个任务按紧迫性排序，紧迫性受优先级等因素影响。

```
$ task next

ID Age P Description                      Urg
-- --- - -------------------------------- ----
 2 10s H Pay bills                           6
 1 20s   Read Taskwarrior documents later    0
```

假设账单已支付，我们希望将任务 2 标记为已完成。

```
$ task 2 done
Completed task 2 'Pay bills'.
Completed 1 task.
```

现在我们可以省略 `next` 命令，因为它是默认命令。

```
$ task

ID Age Description                      Urg
-- --- -------------------------------- ----
 1 5m  Read Taskwarrior documents later    0
```

任务 2 现在消失了。
注意没有可见任务设置优先级，因此不显示优先级列。
现在我们可以删除剩余的任务，因为我们已经在使用教程。

```
$ task 1 delete
Permanently delete task 1 'Read Taskwarrior documents later'? (yes/no) y
Deleting task 1 'Read Taskwarrior documents later'.
Deleted 1 task.

$ task
No matches.
```

这就是你*需要*知道的全部内容。
这四个命令（add、done、delete、next）将让你有效地使用 Taskwarrior。
如果你是 Taskwarrior 的新手，建议你在这里停下来，开始管理你的任务列表一段时间。
我们不希望你在只需要组织和完成工作的时候感到不知所措。

当你对基本的 Taskwarrior 使用感到满意时，你可以了解许多其他功能。
虽然你不会被期望学习所有功能，甚至找不到它们有用，但你可能会找到你正好需要的功能。
