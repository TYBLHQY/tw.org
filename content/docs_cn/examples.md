---
title: "Taskwarrior - 使用示例"
---

# 使用示例

这里有一些涵盖各种主题的 Taskwarrior 命令行示例。
虽然这不是功能和命令行的参考，但也许你会学到一些新东西...

某些示例被刻意选择是因为有多个解决方案，在这种情况下，所有解决方案都被呈现进行比较。

## 30秒教程

```
$ task add Read Taskwarrior documents later
$ task add priority:H Pay bills
$ task next
$ task 2 done
$ task
$ task 1 delete
```

## 创建任务

创建任务很简单，但这里有一些提示：

创建一个有截止日期的任务。

```
$ task add Pay the rent due:eom
```

创建一个任务，然后稍后添加截止日期。

```
$ task add Pay the rent Created task 12
$ task 12 modify due:eom
```

从任务中删除截止日期。

```
$ task 12 modify due:
```

创建一个多行描述的任务。

```
$ task add "Five syllables here
> Seven more syllables there
> Are you happy now?"
```

## 筛选器

筛选器是限制任务只显示你想看或修改的任务的方式。

显示我在过去 4 天添加的任务。

```
$ task entry.after:today-4days list
```

显示我昨天添加的任务。

```
$ task entry:yesterday list
```

显示我在过去 1 小时添加的任务。

```
$ task entry.after:now-1hour list
```

显示我在日期范围内完成的任务。

```
$ task end.after:2015-05-0 and end.before:2015-05-31 completed
```

显示我在过去一周内完成的任务。

```
$ task end.after:today-1wk completed
```

显示在 `This` 项目或 `That` 项目中的任务。

```
$ task project:This or project:That list
```

更复杂的代数筛选器。

```
$ task project:This and \( priority:H or priority:M \) list
```

在描述和注释中搜索 `pattern`：

```
$ task /pattern/ list
$ task rc.search.case.sensitive:yes /pattern/ list
$ task rc.search.case.sensitive:no  /pattern/ list
```

搜索没有项目的任务。

```
$ task project: list
```

## 报告

报告只是指定显示属性、排序、筛选器和名称的配置设置的集合。

临时更改报告的排序。

```
$ task rc.report.next.sort=due-,urgency- next
```

## 项目

一个项目可以分配给一个任务，该项目可能是多个词。

分配一个长的项目名称。

```
$ task add Rake the leaves project:'Home & Garden'
```

将所有任务移到新项目。

```
$ task project:OLDNAME modify project:NEWNAME
```

将所有待处理任务移到新项目。

```
$ task project:OLDNAME and status:pending modify project:NEWNAME
```

使用项目层次结构。

```
$ task add project:Home.Kitchen Clean the floor
$ task add project:Home.Kitchen Replace broken light bulb
$ task add project:Home.Garden Plant the bulbs
$ task project:Home.Kitchen count
2
$ task project:Home.Garden count
1
$ task project:Home count
3
```

当前使用哪些项目？

```
$ task projects
$ task _projects
```

我使用过的所有项目是什么？

```
$ task rc.list.all.projects=1 projects
$ task rc.list.all.projects=1 _projects
$ task _unique project
```

## 标签

标签只是单词字母数字标签，一个任务可以有任意数量的标签。

列出具有特定标签的任务。

```
$ task +home list
```

列出没有特定标签的任务。

```
$ task -home list
```

列出具有任何标签的任务。

```
$ task tags.any: list
$ task +TAGGED list
```

列出没有标签的任务。

```
$ task tags.none: list
```

列出具有两个特定标签的任务。

```
$ task +this +that list
$ task +this and +that list
```

列出具有两个特定标签之一的任务。

```
$ task +this or +that list
```

列出只有两个特定标签之一的任务。

```
$ task +this xor +that list
```

我当前使用什么标签？

```
$ task tags
```

我使用过的所有标签是什么？

```
$ task rc.list.all.tags=1 tags
$ task _tags
```

## 特殊标签

特殊标签是一个具有特定名称的标签，可以影响行为。

修改一个任务以提升其紧迫性，并可能导致它出现在 `next` 报告上。

```
$ task 1 modify +next
```

## 虚拟标签

虚拟标签是一个实际上不存在的标签，但使用标签筛选器语法来模拟标签，提供一个非常方便的快捷方式。
毕竟，编写一个筛选器来匹配今天到期的任务并不简单。

列出今天到期的任务。

```
$ task due.after:yesterday and due.before:tomorrow list
$ task +DUETODAY list
```

列出到期但不是今天的任务。

```
$ task +DUE -DUETODAY list
```

列出本周到期的任务。

```
$ task +WEEK list
```

列出逾期的任务。

```
$ task +OVERDUE list
```

此任务具有哪些虚拟标签？

```
$ task 1 info
```

## 循环任务

循环任务是你需要一次又一次做的任务。

我想创建一个任务，每个星期一上午 9:00 到期，从下周一开始。

```
$ task add Do the thing due:2015-06-08T09:00 recur:weekly
```

我想要每个月支付房租的提醒，但只需一年。

```
$ task add Pay rent due:28th recur:monthly until:now+1yr
```

## 优先级

自版本 2.4.3 以来，优先级现在是 [用户定义属性](../udas/)，因此可以配置。

使优先级 `L` 排序低于无优先级。

```
$ task config uda.priority.values H,M,,L
```

请注意，上述配置仅更改排序顺序。
`L` 优先级在紧迫性计算中仍将被视为高于无优先级。
要更改紧迫性计算，你还需要调整值的紧迫性系数。

我的工作流程需要更多优先级值。

```
$ task config uda.priority.values OMG,DoIt,Meh,Phfh,Nope,
```

我如何从任务中删除优先级？

```
$ task 1 modify priority:
```

## 颜色

我使用了一个颜色主题，但看不到任何颜色。
这通常是因为你的任务不包含截止日期、优先级等。
用这些命令证明颜色正在工作。

```
$ task color
$ task color legend
```

当任务输出进入文件或管道时，所有颜色都会丢失。
强制颜色，使用：

```
$ task rc._forcecolor:on rc.defaultwidth:120 rc.detection:off ...
```

## DOM

Taskwarrior DOM 是一个寻址机制，用于提供对所有存储数据的访问。
它可以在你的脚本中使用，或在命令行操纵任务时使用。

仅获取任务 12 的描述。

```
$ task _get 12.description
Rake the leaves
```

显示任务 12 的创建时间戳和最后修改日期。

```
$ task _get 12.entry 12.modified
2000-04-04T01:02:31 2014-08-24T13:31:43
```

获取我的终端窗口的尺寸。

```
$ task _get tw.width tw.height
```

添加一个任务，并将等待日期设置为截止日期前 4 天。

```
$ task add Pay the rent due:eom wait:due-4days
```

添加一个任务，并使用与任务 12 相同的截止日期。

```
$ task add Buy wine for the party due:12.due
```

获取任务 12 到期的周数。

```
$ task _get 12.due.week
```

获取下一个报告中使用的列。

```
$ task _get rc.report.next.columns
$ task show report.next.columns
```

## 配置

虽然你可以使用任何文本编辑器交互式编辑你的 `.taskrc` 文件，但也有一个命令可以做到这一点。
此外，你可以临时覆盖命令行上的这些设置。

将 `default.command` 设置为不同的报告。

```
$ task config default.command long
```

将 `default.command` 恢复到其原始设置。

```
$ task config default.command
```

将 `default.command` 设置为空值。

```
$ task config default.command ''
```

临时覆盖 default.command。

```
$ task rc.default.command:long
```

显示所有报告的排序顺序。

```
$ task show | grep report | grep sort
```

## 杂项

脚本编写者经常需要访问各种数据，可以获取所有数据，但有时通过奇怪的机制...

最新的任务 ID 是什么？

```
$ task newest rc.verbose=nothing limit:1 | cut -f1 -d' '
$ task rc.verbose=nothing rc.report.foo.columns:id rc.report.foo.sort:id- foo limit:1
```

任务的最少必要数据是什么？

```
$ echo '{"description":"A new task"}' | task import -
$ task add A new task
```

## 故障排除

Taskwarrior 有一些内置功能可帮助你克服障碍。

显示运行时诊断，以查看是否有问题。

```
$ task diagnostics
```

确定 Taskwarrior 使用哪个版本的 GnuTLS。

```
$ task diagnostics | grep gnutls
```

只是让我蛮力改变一个任务。
使用 Vim。

```
$ task 12 edit
$ EDITOR=vim task 12 edit
```

我无法列出已完成的任务，因为 `list` 报告有一个只显示待处理任务的筛选器。

```
$ task rc.report.list.filter: list
$ task all
```

我有最新版本吗？

```
$ curl -L https://gothenburgbitfactory.org/task/latest
{{< current_release "version" >}}
$ task --version 
{{< current_release "version" >}}
```
