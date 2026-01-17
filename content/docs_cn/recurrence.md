---
title: "Taskwarrior - 循环如何工作"
---

# 循环如何工作

循环任务是一个具有到期日期的任务，它不断地作为提醒回来。
这是一个例子：

```
task add Pay the rent due:1st recur:monthly until:2015-03-31
Created task 123.
```

此任务具有到期日期、每月循环和与租约结束相一致的可选截止日期。

循环任务被赋予`recurring`的状态，它将其隐藏在视图外，尽管您可以在`all`报告中看到它。
您创建的循环任务称为模板任务，从该任务创建循环任务实例。
因此模板保持隐藏，从其中产生的循环实例是您将看到并完成的任务。

这是对模板任务的一个看法：

```
$ task 123 info

Name        Value
ID          123
Description Pay the rent
Status      Recurring
Recurrence  monthly
Entered     2014-03-01 12:13:28 (42 secs)
Due         2014-04-01 00:00:00
Until       2015-03-31 00:00:00
UUID        64bcf8fd-74d5-40d2-9e57-1d6a5922cdfc
Urgency     2.4
```

现在，如果您运行一个报告，例如`task list`，您将看到生成的该循环任务的第一个实例。
我们可以看一下该实例：

```
$ task 124 info

Name        Value
ID          124
Description Pay the rent
Status      Pending
Recurrence  monthly
Parent task 64bcf8fd-74d5-40d2-9e57-1d6a5922cdfc
Mask Index  0
Entered     2014-03-01 12:17:03 (15 secs)
Due         2014-04-01 00:00:00
Until       2015-03-31 00:00:00
UUID        29d2df7a-1165-4559-b974-a727519e00f1
Urgency     2.4
```

请注意实例如何具有`pending`的状态，以及对模板任务的引用（父任务）。
此外，您可以看到它继承了循环和描述，如果有项目、优先级和标签，那些也会被继承。

循环实例具有名为'Mask Index'的属性，其值为零。
这表示它是许多循环任务实例中的第一个，计数从零开始。

现在，如果我们回头看模板任务，我们会看到更改：

```
$ task 123 info

Name          Value
ID            123
Description   Pay the rent
Status        Recurring
Recurrence    monthly
Mask          -
Entered       2014-03-01 12:13:28 (3 mins)
Due           2014-04-01 00:00:00
Until         2015-03-31 00:00:00
Last modified 2014-03-01 12:17:03 (24 secs)
UUID          64bcf8fd-74d5-40d2-9e57-1d6a5922cdfc
Urgency       2.4

Date                Modification
2014-03-01 12:17:03 Mask set to '-'.
                    Modified set to '2014-03-01 12:17:03'.
```

模板任务现在有一个`mask`属性和一些更改历史。
该`mask`是任务状态的字符串。
它的值为`-`，表示创建了一个实例（任务124），因为它的长度为一个字符。
`-`意味着该实例仍处于待处理状态。
这就是模板任务跟踪需要生成什么以及不需要生成什么的方式。
当任务实例更改状态时，该`-`变成`+`（已完成）或`X`（已删除）或`W`（等待中）。

请注意，您从不直接与任务123（模板任务）交互。
它被隐藏是有原因的。
相反，您与循环任务实例交互，在大多数情况下，更改会传播到模板任务，有时还会传播到其他同级任务。

在此示例中，为下一个到期期间生成一个任务实例。
这是因为配置设置`recurrence.limit`设置为1，即默认值。
如果将这个数字增加到2，那么您将看到生成的下一个2个实例。
请注意，这只会生成两个步骤进入未来，而不考虑这两个实例是否已完成 - 不要期望完成第一个任务并立即看到一个新任务弹出。
