---
title: "Taskwarrior - 转义命令行字符"
draft: false
---

# 转义命令行字符

某些字符被 shell 解释。
例如，`&`。
如果你想在任务描述中包含 `&`，你需要对其进行转义，以便 shell 不会解释它。
例如：

    $ task add Buy bread & milk

这个命令是一个错误，因为有 `&`。
shell 会将其视为两个命令：

    $ task add Buy bread &
    $ milk

shell 将 `&` 字符视为命令完整的指示符，应该在后台运行。
然后 shell 将"milk"视为一个命令本身，但它不是。
解决这个问题的一种方法是单独转义 `&` 字符：

    $ task add Buy bread \& milk

另一种方法是用 `'` 或 `"` 字符引用整个描述：

    $ task add "Buy bread & milk"

如果你想在任务描述本身中使用引号，你可以单独转义它们：

    $ task add Don\'t forget eggs

在其他情况下，shell 会看到空格并将参数分开。
例如，此命令：

    $ task 123 modify /from this/to that/

被分解成几个参数，可以用引号纠正：

    $ task 123 modify "/from this/to that/"

## 包含空格的项目

假设你想使用包含空格的项目名称。
这需要转义引号：

    $ task add Buy potatoes project:\'Food and Groceries\'

原因是，当 shell 看到引号时，它会将引号之间的内容保留为单个参数，但不会保留引号本身。
通过在引号中添加反斜杠，你可以保证 taskwarrior 看到引号。
这是一个不幸的情况，但对于接受多种命令行字符的程序来说是一个常见的情况。

## 关闭解析

有一个特殊的命令行参数是两个连字符 `--`，当使用这个时，从该点开始禁用特殊的命令行处理，这意味着所有后续参数都被视为任务描述的一部分：

    $ task add -- project:Home needs scheduling

## 当一切都失败了...

有一个解决方案。
我们不喜欢它，但它存在于困难的情况下。
[`edit` 命令](#) 是绕过所有 shell 问题的一种方法。
只需这样做：

    $ task add placeholder
    $ task 1 edit

[`edit` 命令](#) 会将你放入文本编辑器，你可以在其中编写任意复杂的任务描述和注释，而无需考虑引号和转义。

## 特殊 Shell 字符

有许多字符被 shell 视为特殊，可能需要进行转义才能在 taskwarrior 命令行中使用。
这些字符包括：

    $ ! ' " ( ) ; \ ` * ? { } [ ] < > | & % # ~ @
