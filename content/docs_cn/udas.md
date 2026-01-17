---
title: "Taskwarrior - 用户定义属性 (UDA)"
---

# 用户定义属性 (UDA)

Taskwarrior支持一套任务的标准属性，称为核心属性。
这些包括`project`、`description`、`due`等。
有超过20个标准属性（请参见[columns](../commands/columns/)以获取完整列表）。
它们对于提供Taskwarrior的所有功能是必要的。
例如，`project`属性用于提供项目完成的反馈、`projects`命令本身以及项目层次过滤。
`project`属性有很多相关的功能，这就是为什么`project`是一个核心属性。

其他属性（如`priority`）没有很多相关的功能。
实际上，除了存储值、允许修改、排序和包含在报告中之外，`priority`字段贡献不大。
这就是为什么`priority`不是真正的核心属性，而是内置UDA。

有时候，人们会要求新的属性，因为他们的工作流包含Taskwarrior不支持的更多元数据。
一个非常常见的请求是
`estimate`属性，它会存储某种标量数量，可能是天数或大/中/小。
到目前为止，对这些请求的大多数回答是使用标签或注释来近似存储元数据。
现在我们有UDA来实现这一点。

## 什么是UDA？

UDA是您定义的新元数据项，taskwarrior忠实地存储、显示和修改它。
但这就是它的全部，因为Taskwarrior不会像`project`属性那样利用它的功能，而只是将其视为具有名称的数据值，允许您按其排序、在报告中使用它、导入和导出它。

目的是，一旦配置，UDA应与核心属性无法区分，不会带来性能损失。

## 数据类型

UDA有一个类型，可以是`string`、`numeric`、`date`或`duration`之一。如果UDA的类型为`date`，那么它自然只会接受日期值，就像核心属性`due`一样。
`string` UDA类型很特殊，因为也可以提供可接受值的列表，taskwarrior只有在新值在列表中时才允许修改。

## 示例：estimate

通过修改配置来创建UDA。
涉及两三个配置设置。
让我们创建一个数值类型的`estimate` UDA：

    $ task config uda.estimate.type numeric
    $ task config uda.estimate.label Est

就这样 - 但请注意那里有三条信息：首先是"estimate"，这是属性名称，然后是"numeric"，这是类型，最后是"Est"，这是在报告中包含UDA时使用的默认标签。
现在您可以添加或修改任务以包括估计。

    $ task add "Paint the door" project:Home estimate:4
    ...

## 示例：size

现在假设您在敏捷环境中开发软件。
您可能希望有一个`size`属性，其中可能包含固定的一组值，例如"large"、"medium"或"small"。
这是通过使用额外的配置设置来实现的：

    $ task config uda.size.type string
    $ task config uda.size.label Size
    $ task config uda.size.values large,medium,small

现在，如果您尝试存储"tiny"之类的值，taskwarrior将不允许它。

## 默认值

可以定义默认值。
继续上面的例子，要为每个任务指定
`medium`大小默认值，请使用此设置：

    uda.size.default=medium

此默认值将应用于任何其他方式没有`size`值的任务。

## 紧迫性

您可以指定UDA对任务的紧迫性计算有贡献。
例如，上面的`size` UDA可以给定紧迫性系数，如下所示：

    urgency.uda.size.coefficient=2.8

然后，每当任务在`size` UDA中具有非平凡值时，紧迫性就会提高2.8。

要分配具有空值的紧迫性系数，请在属性和系数之间留下无空间。
例如，要匹配紧迫性到空优先级，系数可以像这样分配：

    urgency.uda.priority..coefficient=2.5

## 孤立的UDA

假设您定义了一个`estimate` UDA并使用它。
然后您删除了UDA的配置。
您刚刚创建了一个数据被存储的情况，但不再是可以在报告或过滤器中使用的东西。
这是一个孤立的UDA。

您可能会认为这是一个不寻常的情况，但如果您将具有UDA的数据同步到没有配置UDA的taskwarrior安装，这正是会发生的情况。
数据丢失是不可接受的，所以taskwarrior将保留所有孤立的UDA数据，但不会让您操作它。
只有定义的UDA可以被操纵。
有一个例外 - `edit`命令将让您通过将UDA孤立的值设为空来删除它们，这样就消除了任何属性。

## 自定义报告

UDA字段可以在自定义报告中使用，就像任何其他属性一样。
包含作为孤立UDA的属性的报告不是有效的报告。

## 其他UDA用途

UDA对其他功能很有用。
一个例子是从Request Tracker导入缺陷。
Request Tracker中的附加元数据可以在taskwarrior中创建为UDA，这将允许完整导入而不会丢失数据。
