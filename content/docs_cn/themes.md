---
title: "Taskwarrior - 颜色主题"
---

# 颜色主题

Taskwarrior 支持颜色主题。
这些只是具有定义的颜色规则和规则优先级的配置文件，可以包含在你的 `.taskrc` 文件中，如下所示：

    include /path/to/dark-blue-256.theme

分布中包含了多个主题，默认的 `.taskrc` 文件引用所有这些主题，但这些行被注释掉了。
取消注释一行以使用该主题。

## 覆盖颜色

你可以通过在 include 之后放置更改来覆盖颜色设置：

    include /path/to/dark-blue-256.theme
    color.overdue=bold white on red

这样，主题就成了你指定颜色首选项的基础。

## 默认主题

默认情况下，在没有选择主题的情况下，Taskwarrior 使用简单的深色主题
（`dark-16.theme` 或 `dark-256.theme`，取决于你的系统）。这意味着假设你的终端具有深色背景。
如果你使用浅色背景，这看起来会很糟糕，你应该选择一个浅色主题。

有一个 `no-color.theme` 文件根本没有颜色，虽然这听起来没用，但它允许你从没有颜色开始，然后添加你自己的颜色。
如果你正在构建自己的主题，这是你应该开始的。

## 主题样本

下面是每个提供的主题的示例。

## dark-16.theme

[![主题样本](../../images/dark-16.png)](../../images/dark-16.png)

## dark-256.theme

[![主题样本](../../images/dark-256.png)](../../images/dark-256.png)

## dark-blue-256.theme

[![主题样本](../../images/dark-blue-256.png)](../../images/dark-blue-256.png)

## dark-gray-256.theme

[![主题样本](../../images/dark-gray-256.png)](../../images/dark-gray-256.png)

## dark-gray-blue-256.theme

[![主题样本](../../images/dark-gray-blue-256.png)](../../images/dark-gray-blue-256.png)

## dark-green-256.theme

[![主题样本](../../images/dark-green-256.png)](../../images/dark-green-256.png)

## dark-red-256.theme

[![主题样本](../../images/dark-red-256.png)](../../images/dark-red-256.png)

## dark-violets-256.theme

[![主题样本](../../images/dark-violets-256.png)](../../images/dark-violets-256.png)

## dark-yellow-green-256.theme

[![主题样本](../../images/dark-yellow-green-256.png)](../../images/dark-yellow-green-256.png)

## light-16.theme

[![主题样本](../../images/light-16.png)](../../images/light-16.png)

## light-256.theme

[![主题样本](../../images/light-256.png)](../../images/light-256.png)

## solarized-dark-256.theme

[![主题样本](../../images/solarized-dark-256.png)](../../images/solarized-dark-256.png)

## solarized-light-256.theme

[![主题样本](../../images/solarized-light-256.png)](../../images/solarized-light-256.png)

## bubblegum-256.theme

This theme was created in order to both be aesthetically pleasing and fix all [legibility issues I faced with every other theme](https://github.com/GothenburgBitFactory/taskwarrior/issues/3376).

[![主题样本](../../images/bubblegum-256.png)](../../images/bubblegum-256.png)