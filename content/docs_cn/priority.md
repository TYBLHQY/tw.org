---
title: "Taskwarrior - 优先级"
---

# 优先级

Taskwarrior 从一开始就支持任务优先级的概念。
优先级定义为有四个允许值，`H`、`M` 和 `L`，并另外可选没有优先级。
这些值代表高、中、低和无优先级。
`L` 值被认为是比无优先级更高的优先级。
优先级被用来在大多数内置报告中对任务进行排序。

从 Taskwarrior 2.4.3 开始，优先级不再是核心属性，而是被替换为等效的 [用户定义属性](../udas/)。
这提供了几个优点，用户现在可以配置优先级和影响紧迫性的属性以更接近地匹配他们的需求。

此更改应该不会被注意到，但有一些差异。
本文档介绍了你如何进一步自定义优先级以匹配*你*对优先级含义的概念。

## 默认配置

以下是 UDA 优先级的新默认配置，它与旧核心属性优先级非常接近。

```
color.uda.priority.H=color255
color.uda.priority.L=color245
color.uda.priority.M=color250

uda.priority.label=Priority
uda.priority.type=string
uda.priority.values=H,M,L,

urgency.uda.priority.H.coefficient=6.0
urgency.uda.priority.L.coefficient=1.8
urgency.uda.priority.M.coefficient=3.9
```

需要注意几点，首先是颜色规则现在是 UDA 颜色规则。
与 Taskwarrior 分发的主题都已更新以适应此更改，但你可能拥有尚未修改的本地覆盖。

`uda.priority.values` 设置指定可能值，即 `H`、`M`、`L` 和无优先级。
注意末尾的逗号后面没有值 - 这是指定允许空值的方式。

## 自定义配置

如果你认为优先级 `L` 应该是最低值，甚至低于无值，你可以这样做：

```
$ task config uda.priority.values H,M,,L
```

Those two commas indicate that the blank value lies between `M` and `L`.
这两个逗号表示空白值位于 `M` 和 `L` 之间。

请注意，上述配置仅更改排序顺序。
`L` 优先级在紧迫性计算中仍将被视为高于无优先级。
要更改紧迫性计算，你还需要调整值的紧迫性系数。

如果你想要表示严重性的优先级，你可以这样做：
```
$ task config uda.priority.values Critical,Important,
```

你使用的值数量没有实际限制。
此示例建议你可能想要将 `priority` 重命名为 `severity` 代替。

## 警告

如果你在不同的客户端之间同步任务，你需要以相同的方式配置这些客户端，否则你会发现 Taskwarrior 会强制执行默认配置。
