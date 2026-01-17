---
title: "Taskwarrior - 搜索"
---

# 搜索

在任务中搜索关键字和模式很简单，使用 `/pattern/` 语法。
首先创建一些示例任务，然后我们将搜索它们。

```
$ task add foo
$ task add bar
$ task add baz
```

为了按关键字 `foo` 找到第一个任务，我们这样做：

```
$ task /foo/ list

ID Age   D Description Urg
-- ----- - ----------- ----
 1 1min    foo            0
```

`/` 字符界定搜索项，指示 Taskwarrior 应该做什么。
因为任务注释也是可搜索的文本，我们可以确保任何包含模式 `/foo/` 的注释也会被找到。
让我们添加一个包含这样注释的任务：

```
$ task 3 annotate footwear
$ task /foo/ long

ID Created    Mod   Recur Description
-- ---------- ----- ----- ---------------------
 3 2014-09-28 2min        baz
                            2014-09-28 footwear
 1 2014-09-28 2min       foo
```

这里使用 `long` 报告，以便我们可以看到完整的注释文本。
请注意，任务 1 描述中的 `foo` 和任务 3 注释中的 `footwear` 都被找到了。

## 正则表达式

从版本 {{< label >}}2.4.0{{< /label >}} 开始，所有搜索项默认为[正则表达式](../terminology/#regex)。
这意味着我们可以使用这个模式进行搜索，该模式表示 `f` 后跟两个 `o` 字符：

```
$ task /fo{2}/ long

ID Created    Mod   Recur Description
-- ---------- ----- ----- ---------------------
 3 2014-09-28 3min        baz
                            2014-09-28 footwear
 1 2014-09-28 3min       foo
```

在旧版本中，您需要像这样显式启用正则表达式支持：

```
$ task rc.regex:on /fo{2}/ long

ID Created    Mod   Recur Description
-- ---------- ----- ----- ---------------------
 3 2014-09-28 3min        baz
                            2014-09-28 footwear
 1 2014-09-28 3min       foo
```

或者您可以将设置放在 `.taskrc` 文件中。
您也可以关闭正则表达式支持：

```
$ task rc.regex:off /fo{2}/ long

No matches.
```

这失败是因为搜索项 `/fo{2}/` 这次被视为文本而不是正则表达式，这个项不会出现在任何任务中。

## Shell

如果您的搜索项包含一个或多个空格，那么您的 [shell](../terminology/#shell) 会将搜索模式分解为两个参数，Taskwarrior 会感到困惑。
通过引用或转义来解决这个问题，如这些示例所示：

```
$ task '/foo bar/' list
$ task /foo\ bar/ list
```

这保证 Taskwarrior 看到一个参数 `/foo bar/` 而不是两个 `/foo`、`bar/`。

## 运算符

搜索模式语法 `/pattern/` 是为了方便起见，但有更强大的低级运算符，使得上述模式等同于：

```
$ task description~foo list
```

这里 `~` 匹配运算符的工作方式类似于 Bash 中的。
要反转它以搜索*不*包含该模式的描述，使用不匹配运算符：

```
$ task 'desc!~foo' list
```

这里您看到 `!~` 不匹配运算符、缩写的 `desc` 属性名称和引用，因为 Bash 会以自己的方式解释 `!` 字符。
