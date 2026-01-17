---
title: "Taskwarrior - 已弃用的功能"
---

# 已弃用的功能

Taskwarrior、Tasksh 和 Timewarrior 有很多功能。
随着每个新版本的发布，新功能会被添加，有时功能也会被删除。
功能可能会被删除，如果它已被更好的东西所取代、不再相关，或非常有问题。

随着 Taskwarrior 命令行语法在每个版本中变得更加形式化和规则化，会有相应的一系列更改来删除模糊的命令行语法，并添加一致的语法。

在删除功能之前，如果可能的话，会将其标记为已弃用。
这意味着它将在一个版本中出现在此列表中，然后在未指定的后续版本中被删除。
弃用起到了对消失功能的早期警告作用，给用户时间来适应或抱怨。

删除功能后，任何相关的配置设置也会被删除。
你可以通过使用 `show` 命令来查看你的 `.taskrc` 是否包含任何过时的设置：

```
$ task show
...
```

`show` 命令突出显示各种问题。

## Taskwarrior

### Push/Pull/Merge [deprecated since 2.3.0] [removed in 2.4.0]

Starting with Taskwarrior 2.3.0, the `push`, `pull` and `merge` commands are deprecated, and gone from 2.4.0.
The `sync` command in 2.3.0 replaces this functionality.

### Shadow Files [deprecated since 2.4.0] [removed in 2.4.0]

Starting in 2.4.0, shadow files are replaced by an equivalent hook mechanism.

### Bare Word Search Terms [deprecated since 2.4.0]

Simple words being interpreted as search terms is deprecated in 2.4.0 and will be removed in a future release.
The preferred syntax is `task /pattern/ ...`.

### The 'limit' Pseudo Attribute [deprecated since 2.4.0]

The `limit:20` pseudo attribute looks like a task attribute, but is just syntactic sugar.
It is deprecated and will be replaced by `rc.limit` in a future release.

### The 'urgency.inherit.coefficient' Setting [deprecated since 2.4.5]

Urgency inheritance changes with 2.4.5, and this settings will no longer be used.

### ISO Date and Time non-extended formats. [deprecated since 2.4.5]

The non-extended formats are just strings of numbers without separators.
Here is an extended date `2015-06-25` and here is the non-extended date `20150625`, which looks like a number or even an abbreviated UUID.
Here is an extended time `12:34:56`, and an non-extended time `123456`, with the same issue.
The non-extended forms will be removed.

### Helper commands for unique lists [deprecated since 2.4.5]

The `_ids`, `_projects`, `_tags`, and `_uuids` helper commands are deprecated in favor of the new `_unique` command.

### Virtual tag `DUETODAY` [deprecated since 2.6.0]

The `DUETODAY` virtual tag is a synonym for the more consistently named `TODAY` virtual tag.

### DOM references: context.* [deprecated since 2.6.0]

The dom references `context.program`, `context.args`, `context.width`, and `context.height` are replaced by `tw.program`, `tw.args`, `tw.width`, and `tw.height`.

## Tasksh

None yet.

## Timewarrior

None yet.
