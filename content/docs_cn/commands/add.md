---
title: "Taskwarrior - 添加命令"
---

# add

`add` 命令是创建任务的主要方式。
最简单的任务只需要一个描述：

```
$ task add 修复漏水管道
```

您可以如上所示输入描述，只需将单词作为命令行参数包含即可。
您也可以提供引用的字符串：

```
$ task add "Don't forget to shut off the main water valve first"
```

在此示例中，双引号隐藏了 "Don't" 中未关闭的单个引号。

您也可以使用引用的字符串输入多行描述：

```
$ task add "Five syllables here
Seven more syllables there
Are you happy now?"
```

引用任务描述是一个好主意，可以避免 shell 的某些问题，尽管不是必需的。
（外层）引号不被视为描述的一部分，实际上 Taskwarrior 从不看到它们，因为 shell 首先将其删除。
处理 shell 问题的详细信息在[转义 Shell 字符](#)页面中介绍。

## 其他属性

除了描述，还可以以以下方式提供其他属性：

```
$ task add 找到可调整的扳手 project:Home priority:H
```

此示例为新任务设置了项目和优先级。
请注意，任务描述中单词的顺序得以保留，但无需在命令末尾放置项目和优先级 - 它们可以出现在任何地方。
以下是两个等效命令以说明这一点：

```
$ task add 找到 project:Home 可调整的 priority:H 扳手
$ task add project:Home priority:H 找到可调整的扳手
```

除了项目和优先级，您还可以设置条目时间戳、开始时间戳、截止日期、标签、重复频率、截止日期和状态。
您无法在创建任务时添加注释，因为这会干扰任务描述。
您可以指定除 ID 和 UUID 之外的任何内容。
这些会自动创建。

## 限制

- 无法同时添加任务并为其注释。
  注释必须应用于现有任务。

## 另请参阅

其他创建任务的方式包括：

- [`log`](../log/) 命令
- `duplicate` 命令
- `import` 命令
