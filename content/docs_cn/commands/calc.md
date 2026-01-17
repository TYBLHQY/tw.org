---
title: "Taskwarrior - 计算命令"
---

# calc

Taskwarrior 有一个 `calc` 命令，它公开所有其他命令和过滤器使用的代数表达式求值器。
这对于从命令行进行快速计算很方便，但与 DOM 访问相结合，可以非常有用。

## 数字

这可以用于使用 `+`、`-`、`*` 和 `/` 运算符进行基本数学运算：

```
$ task calc 1+2*3
7
```

在上面的示例中，需要注意不要让 `*` 运算符被 shell 展开为当前目录中的文件列表。
最好的解决方案是简单地引用该表达式。

除了整数，你还可以使用实数和科学计数法：

    $ task calc '1.23e-4.5 ^ 2'
    1.5129e-8

除了取幂 `^` 外，还有 `%` 模运算符。

## 布尔值

`and`、`or`、`xor`、`==`、`!==`、`=`、`!=`、`!`、`~`、`!~`、`<=`、`<`、`>=` 和 `>` 运算符允许布尔表达式：

```
$ task calc true and false
false
$ calc true or false
true
$ calc true xor true
false
$ calc '1 < 2 and -1 < 0'
true
```

数字可以用双重否定转换为布尔值：

```
$ task calc '!!3'
true
```

否则，类型转换是自动的。

## 文本

文本或字符串可以以更有限的方式进行操作，例如有连接：

```
$ task calc foo + bar
foobar
```

乘法支持复制形式：

```
$ task calc 'x * 40'
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

支持正则表达式，使用 `~` 匹配和 `!~` 不匹配运算符：

```
$ task calc 'foo ~ f'
true
$ task calc 'foo !~ z'
true
$ task calc '"The quick brown fox" ~ "q"'
true
```

同样，你需要保护 `~` 和 `!~` 字符免受 shell 的解释，在最后一个示例中，还要保护句子中的空格。

## 日期

对于日期支持，支持基本的日期同义词，如 `now`、`today`、`yesterday` 和 `tomorrow`：

```
$ task calc now
2014-06-28T12:44:07
$ task calc today
2014-06-28T00:00:00
```

日期可以添加、减去和比较，并与持续时间结合：

```
$ task calc tomorrow + 2 weeks - 1d
2014-07-12T00:00:00
```

表达式求值器理解 ISO-8601 日期格式，实际上支持超过 40 种格式。
例如，2014 年第 40 周何时开始？

```
$ task calc 2014W40
2014-09-28T00:00:00
```

FIFA 世界杯决赛什么时候进行，它在巴西当地时间下午 4 点开始，并以 EST（本地）时区显示？

```
$ task calc 2014-07-13T16:00:00-03:00
2014-07-13T15:00:00
```

## 持续时间

持续时间要么是日期之间的差异，要么是指定的间隔。
持续时间可以相加：

```
$ task calc 12h + 25m +30s
PT12H25mPT30S
```

减去日期会产生持续时间，在这里回答了这个月末（2014 年 6 月）和年末之间有多少时间的问题：

```
$ task calc eoy-eom
P184DT1H
```

请注意，"1H" 对应于夏令时结束时获得的额外小时。

32 位 `time_t` 问题（Unix Epoch，Y2K38）何时出现？

```
$ task calc later-now
P23Y209DT11H55M11S
```

看起来我们有 23 年的时间来处理这个问题。
不过更重要的是，距离 FIFA 世界杯决赛还有多久？

    $ task calc 2014-07-13T15:00:00-03:00 - now
    P15DT1H11M46S

持续时间使用 ISO-8601 指定格式显示，仅使用精确单位，即不按年或月显示。

## DOM 访问

表达式求值器可以访问 Taskwarrior DOM，这是任务信息的完整来源。
此信息可以使用常规 Taskwarrior DOM 引用在表达式中访问和使用：

```
$ task calc '1.urgency > 2.urgency'
false
```

由于 DOM 访问，`calc` 命令可以模拟 `_get` 辅助命令。
我什么时候输入任务 100？

```
$ task calc 100.entry.year
2012
```

那是哪一周？

```
$ task calc 100.entry.week
30
```

## Calc 实用程序

Taskwarrior 有一个独立的 `calc` 实用程序，具有相同的功能，除了 DOM 访问。
此实用程序用于测试，但有一些有趣的功能。

## Calc 后缀表示法

在内部，表达式求值器将中缀表达式（1 + 2 * 3）转换为后缀（1 2 3 * +），这更容易实现和优化，但 `calc` 公开了这一点：

```
$ calc --postfix '1 2 3 * +'
7
```

## Calc 调试

还支持 `--debug` 命令行选项，它在以后缀形式评估表达式时显示堆栈操作。

```
$ calc --postfix --debug '1 2 3 * +'
[0] eval push '1'
[1] eval push '2'
[2] eval push '3'
[3] eval pop '3'
[2] eval pop '2'
[1] eval operator '*'
[1] eval result push '6'
[2] eval pop '6'
[1] eval pop '1'
[0] eval operator '+'
[0] eval result push '7'
7
```

你可以看到处理开始时通过将三个值压入堆栈，当遇到 `*` 二元运算符时弹出两个值。
两个值相乘，结果 6 被压回。

接下来遇到 `+` 二元运算符，它从堆栈中弹出两个值，总和 7 被压回。

括号中的数字是执行操作前的堆栈大小。

如果在 Taskwarrior 表达式求值器中发现问题，可以通过这种方式使用 `calc` 实用程序进行验证和测试。
