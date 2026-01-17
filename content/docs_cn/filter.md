---
title: "Taskwarrior - 过滤器"
---

# 过滤器

过滤器是一组命令行参数，用于指定一组任务，过滤器的复杂程度可以从简单到非常复杂。
最简单的过滤器是空过滤器，我们可以用 `count` 来说明这一点。

    $ task count
    100

这 100 个任务是待处理和已完成的任务，代表了 Taskwarrior 已知的所有任务。
任何接受过滤器的命令也接受无过滤器，如上所示。
现在我们引入一个带有一个条件的过滤器。

    $ task status:pending count
    38

这是指定属性值的 `name : value` 形式的示例。
此过滤器显示有 38 个待处理任务，因此有 62 个以某种方式不待处理。
这种过滤器形式也适用于其他属性：

    $ task project:Home count
    19

'Home' 项目中有 19 个任务。

`value` 参数可以留空以仅匹配那些没有分配相关类型值的项目。
例如，你可以计数未分配到任何项目的任务，如下所示：

    $ task project: count
    81

在此示例中，我们可以看到未分配"Home"项目的任务尚未分配到任何项目。

Taskwarrior 除了 `name : value` 过滤器外还有其他过滤器。
下面是两个示例，根据标签的存在和不存在进行过滤。

    $ task +work count
    14
    $ task -work count
    86

这显示 14 个任务有"work"标签，86 个没有该标签。
要在描述或注释中搜索单词：

    $ task /m..ting/ count
    3

在这里，我们看到 3 个任务在描述中有"m..ting"这个词。
这是一个正则表达式，尽管它也可以只是一个简单的词。

这假设你没有使用 `rc.regex` 配置设置禁用正则表达式支持。

## 复杂过滤器

通过添加更多过滤条件和逻辑，过滤器获得复杂性。
假设我们想查看有"Home"项目但没有"work"标签的任务数量。
只需在命令行上放置两个条件。

    $ task project:Home -work count
    18

过滤器中的两个条件都应用于所有任务的集合，换句话说，任务必须同时有"Home"项目且没有"work"标签才能被计数。

当过滤器包含多个这样的条件时，它们被视为逻辑与，即两个条件都必须匹配才能由过滤器选择任务。
如果过滤器中有三个条件，那么所有三个都必须匹配。

这个假设的与是另一种命令行语法快捷方式。
此命令行的长形式是：

    $ task project:Home and -work count
    18

看到过滤条件之间的 `and` 运算符了吗？
如果没有明确指定，就假设它在那里。
隐含的意思是也支持析取（'or'）。

    $ task project:Home or -work count
    90

此示例询问有多少任务是"Home"项目的一部分，或没有"work"标签 - 任一项都是匹配。

## 优先级

之前提到最简单的过滤器是空过滤器，例如 `count` 命令在使用。
现在我们考虑 `ls` 报告，它有一个过滤器：

    $ task show report.ls.filter

    Config Variable  Value
    ---------------- --------------
    report.ls.filter status:pending

此报告过滤器与你指定的命令行过滤器结合：

    $ task project:Home ls

这产生了一个组合过滤器：

    status:pending project:Home

它有一个隐含的与：

    status:pending and project:Home

现在，如果我们想使用 `ls` 命令的析取过滤器：

    $ task project:Home or project:Garden list

这被解释为：

    status:pending and (project:Home or project:Garden)

它包括用于分组析取的括号。

现在假设我们想在 `all` 命令中使用上面的过滤器：

    $ task status:pending and (project:Home or project:Garden) all
    ...

现在，我们有另一个问题 - 那些括号对 shell 有特殊含义，必须以以下方式之一进行转义：

    $ task status:pending and \(project:Home or project:Garden\) list
    ...
    $ task status:pending and '(project:Home or project:Garden)' list
    ...
    $ task status:pending and "(project:Home or project:Garden)" list

请注意，taskwarrior 命令行中使用的许多字符对 shell 有特殊含义，因此必须正确转义，如[此处所述](../escapes/)。

还要注意，当搜索在括号末尾没有指定的条目的任务时，需要添加空格，否则会得到"表达式中的括号不匹配"。

    # 这样
    $ task '(project: )'
    # 不是这样
    $ task '(project:)'

## 配置

仅在以下情况下启用时，模式过滤器中的正则表达式才被使用：

    regex=on

这是 2.4.0+ 的默认值。
如果未启用，模式是要匹配的字面字符串。
字符串搜索和正则表达式的大小写敏感性由以下控制：

    search.case.sensitive=yes

默认值是"yes"。
