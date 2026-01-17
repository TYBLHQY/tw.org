---
title: "Taskwarrior - 哲学"
---

# Taskwarrior 哲学

Taskwarrior是按照一种哲学开发的，这种哲学解释了为什么做出了某些决定，以及将继续做出的决定。
所有Taskwarrior家族项目都遵循同样的哲学。

## 开放性

[源代码](https://github.com/GothenburgBitFactory/taskwarrior)、[计划](https://github.com/GothenburgBitFactory/taskwarrior/blob/develop/doc/devel/rfcs/plans.md)、[设计](https://github.com/GothenburgBitFactory/taskwarrior/blob/develop/doc/devel/rfcs/README.md)、[缺陷](https://github.com/GothenburgBitFactory/taskwarrior/issues)、[测试](https://github.com/GothenburgBitFactory/taskwarrior/actions)、[文档](../)和[网站](https://github.com/GothenburgBitFactory/tw.org)都是免费和开放源代码的。
您的数据以纯文本形式保存，永远不会被专有格式所困。
欢迎您为项目的各个方面做出贡献并指出改进。
没有隐含的议程。

该项目的目标是以最小的摩擦和最小的复杂性支持所有工作流，同时改进列表管理。

## 低摩擦力

允许您捕获任务的工具需要是低摩擦力的。
这意味着不应该有登录身份验证、冗长的启动延迟或交互阻碍简单地捕获信息。
不可能更容易了：

```
$ task add '...'
```

任何有摩擦力的工具都会阻止其自身使用。

## 无使用惩罚

对于您不使用的功能不应该有任何惩罚。
例如，Taskwarrior支持循环和依赖项，但如果您不使用这些功能，则不会有性能惩罚或成本。

但这也意味着当您需要时这些功能就在那里。
如果您决定开始使用优先级，那么您可以这样做，并且某些任务具有优先级而某些没有是可以的。

## 方法论无关

生产力方法论推广简单、习惯形成、可重复的过程和工作流，结合一些纪律为您提供有效的工作方法。
它们倾向于专注于维护高质量的列表，快速完成可以快速完成的事情，区分重要和不重要的事情，调整自己的步调，不遗忘事情。

虽然方法论很重要，但Taskwarrior不强加或偏好任何方法论，而是承认每个人的工作方式不同，对优先级、到期日期、依赖等事物强调不同。

您可能会从已知的方法论开始，但会发现有些部分对您效果好，有些则不好，您可能会发展您自己独特的方法论。
这意味着Taskwarrior以某种形式支持以上所有内容，当您开发自己的工作方法论时。

## 工具箱

支持所有方法论和工作流意味着有很多功能，但您不需要使用或甚至了解它们所有。
将Taskwarrior视为一个工具箱，让您遵循您选择的任何方法论，任何给定的方法论将仅使用功能的一个小子集。

您的厨房有许多工具和小工具，但您可能只使用其中的一小部分。
它们的存在是为了让您有选择。

## 扩展友好

使用行业标准JSON的导入/导出允许您将数据移动进出Taskwarrior，因此您可以提供前端或高阶功能：

```
$ task 1 export
[
{"description":"Send Alice a birthday card","due":"20161108T000000Z", ...}
]
```

钩子脚本允许您在输入和修改时修改任务，或拒绝更改以强制执行策略。

```
$ task add Paint the door
Warning: Task does not have an assigned project
```

助手命令允许您提取元数据：

```
$ task project:Home _ids
36
77
89
122
123
```

完整的DOM允许您向下钻取到单个数据：

```
$ task _get 123.entry.year
2016

$ task _get rc.dateformat
Y-M-D
```

## 社区

Taskwarrior因周围和支持它的社区而大大增强。
您将在线找到帮助、支持、扩展等。

与我们交谈。
加入社区，帮助他人，帮助自己，推进项目。
我们欢迎你们所有人加入我们的社区。

这是我们的[行为准则](../../conduct/)。

## 专注

Taskwarrior仔细限制其支持的功能，以便专注于做好一件事。
它不提供提醒和时间跟踪，因为有其他项目致力于很好地实现这些功能。

如果一个功能改进了我们管理任务列表的方式，那么它属于Taskwarrior，否则它属于其他软件。
