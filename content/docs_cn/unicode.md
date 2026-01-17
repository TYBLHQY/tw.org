---
title: "Taskwarrior - 统一码字符集"
---

# 统一码字符集

所有 Taskwarrior 中的文本都是 UTF8，这意味着可以输入和存储任何 Unicode 字符。
这是一个演示：

    $ task add Download U+266C U+2669 for the plane
    Created task 5.
    $ task 5 list
    ID Age   P Description                    Urg 
     6 10s   M Download ♬ ♩ for the plane      3.9

支持 `U+NNNN` 和 `\uNNNN` 指定符，但通常使用第一种更简单，不需要在 shell 和脚本中转义反斜杠。

请注意，您在终端中使用的字体决定了这些字符是否呈现，因此可以输入没有字形的字符。
