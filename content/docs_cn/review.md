---
title: "Taskwarrior - Tasksh 评审"
---

# Tasksh 评审

从发布版本 {{< label >}}1.1.0{{< /label >}} 开始，Taskwarrior shell (`tasksh`) 有一个 `review` 命令，让您可以评审您的任务。

评审您的任务列表很重要，因为您需要确保首先处理更紧急的任务，并且还要确保您的列表是最新的。
只有准确的元数据（截止日期、优先级等），您的任务列表才能反映现实世界的需求。
定期评审将帮助您维护正确的截止日期、优先级、依赖关系、标签、项目分配等，同时删除不再需要的任务。

这是评审功能的快速演示：

## 工作原理

理想情况下，您应该定期评审任务列表，每周一次。
如果您发现自己没有对任务进行任何更改，也许您应该更少地评审。
目标是使评审过程有效地清理列表，但不是负担或浪费时间。

Tasksh 通过创建一个专门为此目的的 Taskwarrior 报告来实现评审。
它被命名为 `_reviewed`，仅列出需要评审的任务的 UUID 值。
然后这个报告驱动一个交互式会话，您逐个任务进行，并有机会修改、跳过或将任务标记为已评审。

当您将任务标记为已评审时，Tasksh 会向任务添加一个 `reviewed` 时间戳，作为为此目的定义的 [UDA](../udas/)。
此属性在 `_reviewed` 报告过滤器中使用，以确保您不会比每周更频繁地评审同一任务。

`reviewed` 时间戳和 `_reviewed` 报告的组合意味着如果您今天评审所有任务，然后立即执行另一次评审，将不会产生任何任务进行评审。
过了一周后，您将能够再次评审所有任务。

这种"恢复"评审会话的能力意味着您可以随时停止评审会话，稍后恢复。
您甚至可以指定要评审多少个任务，这意味着您可以更频繁地评审一小组任务，使评审过程更短。

首次评审时，Tasksh 将自动配置 Taskwarrior 以创建 `_reviewed` 报告和 `reviewed` UDA。
创建报告后，您可以修改它。
这是报告定义：

```
$ task show report._reviewed

Config Variable              Value
---------------------------- -------------------------------------------------------
report._reviewed.columns     uuid
report._reviewed.description Tasksh review report.  Adjust the filter to your needs.
report._reviewed.filter      ( reviewed.none: or reviewed.before:now-1week ) and
                             ( +PENDING or +WAITING )
report._reviewed.sort        reviewed+,modified+
```

过滤术语 `reviewed.before:now-1week` 可以更改以满足您的需求。

## 启动 Tasksh

启动 tasksh，您将立即看到可用命令的摘要，然后是提示符：

```
$ tasksh

tasksh 1.1.0

    tasksh> review [N]       任务评审会话，可选在 N 个任务后截断
    tasksh> list             或任何其他 Taskwarrior 命令
    tasksh> diagnostics      Tasksh 诊断
    tasksh> help             Tasksh 帮助
    tasksh> exec ls -al      任何 shell 命令。也可以使用 '!ls -al'
    tasksh> quit             会话结束。也可以使用 'exit'

tasksh> 
```

您可以看到 `review` 是命令之一。
您可以简单地启动一个评审会话，该会话可以随时退出：

```
tasksh> review
...
```

或者您可以评审固定数量的任务：

```
tasksh> review 12
...
```

评审固定数量可以帮助您在方便的时候迭代评审所有任务，而无需一次性完成整个列表。
