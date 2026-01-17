---
title: "Taskwarrior - 列表报告"
---

# list

`list` 报告是一个可自定义的报告，这意味着报告的许多方面都是可自定义的。
您可以通过修改 Taskwarrior 配置来覆盖任何方面。
`list` 报告的定义可以通过这样的 `show` 命令来显示：

```
$ task show report.list

Config Variable         Value
----------------------- --------------------------------------------------------------
report.list.columns     id,start.age,entry.age,depends.indicator,priority,project,
                        tags,recur.indicator,scheduled.countdown,due,until.age,
                        description.count,urgency
report.list.description Most details of tasks
report.list.filter      status:pending
report.list.labels      ID,Active,Age,D,P,Project,Tags,R,Sch,Due,Until,Description,Urg
report.list.sort        start-,due+,project+/,urgency-
```

这是定义报告的五个设置。
描述和标签是直白的文本。

## 列

列是任务元数据，带有可选的格式说明符。
例如，`description.count` 列表示描述列，但不包括注释；而是显示注释的计数（如果有）。
你可以使用 [`columns` 命令](../columns/) 查看支持的列和格式。

## 过滤器

运行此报告时会自动应用过滤器。
在这种情况下，过滤器使用 `status:pending` 仅显示待处理的任务。这可能会造成一些混淆，因为如果你运行这个命令：

```
$ task status:completed list
```

然后过滤器 `status:pending` 与命令行过滤器 `status:completed` 结合时定义了两个不相交的集合，不显示任何结果。
这是因为过滤项假定使用 `and` 运算符组合，除非另有指定。
相反，要查看已完成的任务，你需要使用不对状态进行过滤的报告，`all` 报告恰好做了这个，所以通过运行以下命令得到预期的结果：

```
$ task status:completed all
```

## 排序

排序设置是列的列表，带有可选的方向（升序或降序）和可选的中断指示符。
排序列不必是报告中包含的列。

有四个排序键，按顺序应用，所以报告按开始日期、然后截止日期、然后项目、然后紧急程度排序。
从示例中，第三个排序键是：

```
project+/
```

分解一下，这意味着 `project` 属性按升序排列（因此是 `+`），斜杠（`/`）表示这是一个中断列，这意味着在每个唯一值之前插入一个空行。

## 配置

有一个配置选项，名为 `print.empty.columns`，默认为 `off`。
这意味着如果报告有一列，其中没有任务显示的值，则不显示此空列。
这会导致报告的宽度窄得多。

`list` 报告有 13 列，但由于此设置，输出中很少看到超过 6 或 7 列。

## 另见

- [报告](../../report/)
- [`columns`](../columns/) 命令
