---
title: "Taskwarrior - 配置"
---

# 配置

Taskwarrior 将所有配置信息存储在你的主目录中的一个名为 `.taskrc` 的文件中。
默认的 `.taskrc` 文件包含最少的条目，只有一个必需的设置，即：

```
data.location=~/.task
```

这是你唯一需要的设置，因为 Taskwarrior 对所有设置都有合理的默认值。
此文件实际上只是你希望用来覆盖这些默认值的设置列表。

## Config 命令

`config` 命令可用于修改你的 `.taskrc` 文件。
在此示例中，我们通过执行以下操作来禁用筛选器中的正则表达式支持：

```
$ task config regex off
Are you sure you want to change the value of 'regex' from 'on' to 'off'? (yes/no) yes
Config file ~/.taskrc modified.
```

你可以使用 'off'、'0'、'no' 或 'false'，这些都是禁用该功能的同义词。
系统会要求你确认更改，这由 `confirmation` 设置控制，当然你可以用以下方式禁用：

```
$ task config confirmation off
```

该命令的一般形式可以是以下任何一种：

```
task config name value
task config name ''
task config name
```

这三个示例分别展示了将 `name` 设置为 `value`、将 `name` 设置为空值以及删除设置。
注意只有删除设置才能移除覆盖，从而恢复默认值。

## Show 命令

`show` 命令显示所有当前配置设置，这是所有设置和默认值的列表，你的本地设置会覆盖这些设置，此外还包括任何命令行覆盖。
show 命令还会按你指定的关键字筛选设置，所以要查看 `minimal` 报告定义，你可以运行以下命令：

```
$ task show report.minimal

Config Variable            Value
-------------------------- ----------------------------------------
report.minimal.columns     id,project,tags.count,description.count
report.minimal.description Pending tasks by project and description
report.minimal.filter      ( status:pending or status:waiting )
report.minimal.labels      ID,Project,Tags,Description
report.minimal.sort        project+,description+,entry+
```

`show` 命令会高亮显示与默认值不同的值，并且还会告诉你是否有无法识别的设置。
这可能表示拼写错误或过时的设置。

## 包含

`.taskrc` 文件支持包含，例如用于主题文件。

```
include ~/themes/solarized-dark-256.theme
```

包含的文件应该包含 Taskwarrior 配置设置或嵌套包含。

## 命令行覆盖

`config` 命令对你的 `.taskrc` 文件进行永久更改，但你可以使用此技术临时覆盖单个命令的这些设置：

```
$ task rc.regex=off /[ISSUE-5]/ list
```

此功能的一种可能用途是覆盖 `data.location` 设置以使用备用任务列表：

```
$ task rc.data.location=/alternate/path/.task ...
```

## 环境变量

有两个环境变量可用于指定备用配置文件和备用数据位置。

```
TASKRC=~/.taskrc TASKDATA=~/.task task list
```

此示例使用环境变量来指定配置文件和数据目录。
