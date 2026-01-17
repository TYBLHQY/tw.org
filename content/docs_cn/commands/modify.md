---
title: "Taskwarrior - 修改命令"
---

# modify

`modify` 命令是改变任务的最直接的方式，例如提换描述：

```
$ task 1 modify 这是新的描述
```

以下是一个标签另一个从任务删除的例子：

```
$ task 1 modify -home +garden
```

同样的修改，但直接作用于多个任务：

```
$ task 1 3 5-10 modify -home +garden
```
相同的改变，但作用于案笫匹配过滤器的任务集合：

```
$ task \( project:outdoors or /planting/ \) modify -home +garden
```

更正任务描述中的拼写错误：

```
$ task 1 modify /teh/the/
```

改变任务的几乎所有内容：

```
$ task 1 modify +tag /from/to/ project:New priority:H depends:2 due:tomorrow recur:weekly New description
```

## 配置

Taskwarrior 有一个 'bulk'（批量）阈值，默认为三个任务。
如果你尝试在一个命令中修改超过三个任务，则需要额外的确认：

```
$ task 1-100 modify +later
  - Tags 将设置为 'later'。
修改任务 1 'Buy a new dog collar'？(yes/no/all/quit)
```

`rc.bulk` 配置设置可以修改以提高阈值，其值为 `0` 表示无穷大。
批量阈值旨在防止具有不正确过滤器的命令。

## 重复

如果你修改一个重复任务，系统会询问你是否要将更改传播到其他实例：

```
$ task add pay the rent recur:monthly due:2015-01-01
$ task list

ID R Due        Description
-- - ---------- ------------
 2 R 2015-01-01 pay the rent
 3 R 2015-02-01 pay the rent
 4 R 2015-03-01 pay the rent
 5 R 2015-04-01 pay the rent
 6 R 2015-05-01 pay the rent
 7 R 2015-06-01 pay the rent
 8 R 2015-07-01 pay the rent
 9 R 2015-08-01 pay the rent

$ task 2 modify /pay/Pay/
修改任务 2 'Pay the rent'。
这是一个重复任务。你是否想修改这个任务的所有待处理重复？(yes/no) yes
修改重复任务 2 'Pay the rent'。
修改重复任务 3 'Pay the rent'。
修改重复任务 4 'Pay the rent'。
修改重复任务 5 'Pay the rent'。
修改重复任务 6 'Pay the rent'。
修改重复任务 7 'Pay the rent'。
修改重复任务 8 'Pay the rent'。
修改重复任务 9 'Pay the rent'。
已修改 9 个任务。
```

拒绝修改只会影响指定的任务。

## 限制

- 当使用过滤器修改任务时，很容易忘记将更改限制为仅待处理的任务，需要将 `status:pending` 添加到过滤器。
  否则，它将改变所有已完成和已删除的任务。
- 使用 `rc.confirmation=off`、`rc.bulk=0`、`rc.recurrence.confirmation=off` 且没有过滤器，可能会造成重大损害。

## 另见

修改任务的其他方式包括：

- [`append`](../append/) 命令
- `edit` 命令
- [`prepend`](../prepend/) 命令
