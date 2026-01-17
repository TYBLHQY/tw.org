---
title: "Taskwarrior - 上下文"
---

# 上下文

上下文与一个位置相关联。
例如，你可能在三个不同的位置执行任务：

- 办公室
- 家里
- 学习

与你在办公室时间相关的任务在你在家时就没有意义，反之亦然。
这只是一个例子，你的上下文可能会非常不同。

如果 Taskwarrior 允许你指定当前活跃的上下文，那么列出的任务可以被相应地过滤。
那时你就会在一个上下文中工作。
因此，上下文是一个命名的过滤器，当前上下文是一种默认过滤器的形式。

## 定义上下文

为了在一个上下文中工作，你首先需要定义该上下文。
由于上下文本质上是一个任务过滤器，定义上下文实际上是定义一个命名的过滤器。
在这个例子中，我们使用新的 `context define` 命令从上面的列表定义我们的上下文：

```
$ task context define work +work or +freelance
$ task context define study +school or +homework or +lab
$ task context define home -work -freelance -school -homework -lab
```

上下文定义可以包含任何形式的代数表达式，就像一个过滤器一样。
在这个例子中，上下文完全基于标签。
注意，`home` 被定义为既不是 `work` 也不是 `study`。这意味着每个任务都被考虑到，尽管这不是必需的。

尝试用名称 `define`、`list`、`show`、`none` 或 `delete` 定义上下文是一个错误。

## 设置上下文

要设置或切换当前上下文，只需：

```
$ task context home
$ task list
...
```

如果你尝试使用未定义的上下文，Taskwarrior 会报告错误。

现在，当上下文设置为 `home` 时，列出的所有任务将与定义的 `home` 上下文相关。
每个报告后都会有一个脚注提醒你当前上下文。

## 显示上下文

虽然当前上下文包含在每个报告之后的脚注中，但这可以通过详细程度控制禁用。
要显示当前上下文：

```
$ task context show
home
```

这也可以使用 `_get` 获得：

```
$ task _get rc.context
home
```

## 列出所有上下文

您可以使用新的 `context list` 命令列出所有上下文：

```
$ task context list
上下文 过滤器
------- ----------------------------------
home    -work -freelance -school -homework
study   +school or +homework
work    +work or +freelance
```

## 清除上下文

要清除当前上下文：

```
$ task context none
```

上下文 `none` 有特殊的意义
所有后续命令都不会应用任何隐式上下文过滤器。

## 删除上下文

要删除一个上下文：

```
$ task context delete study
```

现在您不能再将上下文设置为 `study`。如果您删除上下文时当前上下文已经是 `study`，则上下文会被清除。

## 对命令的影响

所有接受过滤器的报告都会使用上下文（如果已定义和设置）。

## 相关支持

`tasksh` 程序将在其提示符中显示当前上下文。

## 实现细节

上下文将存储在 `rc.context` 中，定义的上下文将作为 `rc.context.<name>` 存储在 `.taskrc` 文件中。

当使用上下文过滤器时，它将隐含地被括号包围，以便它可能包含任意逻辑。
