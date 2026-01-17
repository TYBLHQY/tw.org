---
title: "Taskwarrior - 对象模型"
---

# 对象模型

Taskwarrior 有一个文档对象模型，或 DOM，它定义了一种引用 taskwarrior 管理的所有数据的方法。
你可能熟悉 Web 浏览器实现的 DOM，它允许你以编程方式访问页面上的详细信息。
例如：

```
document.getElementById("myAnchor").href
```

Taskwarrior 允许以类似的形式进行相同类型的数据访问，例如：

```
1.description
```

这引用了任务 1 的描述文本。
有一个 [`_get` 帮助命令](../commands/_get/) 使用 DOM 引用查询数据。
让我们通过首先创建一个详细的任务来看到它的实际作用。

```
$ task add Buy milk due:tomorrow +store project:Home pri:H
$ task 1 info

Name          Value
------------- ------------------------------------------
ID            1
Description   Buy milk
Status        Pending
Project       Home
Priority      H
Entered       2014-09-28 21:53:59 (4 seconds)
Due           2014-09-29 00:00:00
Last modified 2014-09-28 21:53:59 (4 seconds)
Tags          store
UUID          c0ab2bf6-b4f5-45c2-8420-18ab4f1ba7e7
Urgency       16.56
                           project     1  *    1 =     1
                          priority     1  *    6 =     6
                              tags   0.8  *    1 =   0.8
                               due  0.73  *   12 =  8.76
```

该任务的所有属性都可以通过 DOM 引用访问。
以下是一些例子：

```
$ task _get 1.description
Buy milk

$ task _get 1.uuid
c0ab2bf6-b4f5-45c2-8420-18ab4f1ba7e7

$ task _get c0ab2bf6-b4f5-45c2-8420-18ab4f1ba7e7.id
    1

$ task _get 1.due.year
2014

$ task _get 1.due.julian
272
```

## 支持的引用

### `system.version`

taskwarrior 的版本，例如：

```
$ task _get system.version
2.4.0
```

### `system.os`

操作系统，例如：

```
$ task _get system.os
FreeBSD
```

### `rc.<name>`

任何配置值默认值，应用了 `.taskrc` 中的任何覆盖，然后最后应用任何命令行覆盖。
例如：

```
$ task _get rc.data.location
~/.task
```

### `<attribute>`

任何任务属性。
请注意，没有指定任务 ID 或 UUID，因此此变体仅在命令行上有用，如下所示：

```
$ task add Pay rent due:eom wait:'due - 3days'
```

请注意，"due"是命令行前面的 DOM 引用。

### `<id>.<attribute>`

指定任务 ID 的任何属性。
例如：

```
$ task add Fix the leak depends:3 scheduled:3.due
```

这使新任务依赖于任务 3，并计划在任务 3 的到期日期。
请注意，"3.due"是特定任务的 DOM 引用。

### `<uuid>.<attribute>`

指定任务 UUID 的任何属性，如上所示。

任何类型为 `date` 的属性都可以直接作为日期访问，或者可以通过该日期的元素访问。
例如：

* `<date>.year` - {{< label >}}2.4.0{{< /label >}} 年份，例如：
  ```
  $ task _get 1.due.year
  2014
  ```

* `<date>.month` - {{< label >}}2.4.0{{< /label >}} 月份，例如：
  ```
  $ task _get 1.due.month
  9
  ```

* `<date>.day`  - {{< label >}}2.4.0{{< /label >}} 月份中的第几天，例如：
  ```
  $ task _get 1.due.day
  29
  ```

* `<date>.week` - {{< label >}}2.4.0{{< /label >}} 日期的周数，例如：
  ```
  $ task _get 1.due.week
  40
  ```

* `<date>.weekday` - {{< label >}}2.4.0{{< /label >}} 日期的编号工作日，其中 0 是星期日，6 是星期六。
  例如：
  ```
  $ task _get 1.due.weekday
  1
  ```

* `<date>.julian` - {{< label >}}2.4.0{{< /label >}} 日期的儒略日，即年内日期的日期号。
  例如，1 月 1 日是 1，2 月 10 日是 41。
  例如：
  ```
  $ task _get 1.due.julian
  272
  ```

* `<date>.hour` - {{< label >}}2.4.0{{< /label >}} 一天中的小时，例如：
  ```
  $ task _get 1.due.hour
  0
  ```

* `<date>.minute` - {{< label >}}2.4.0{{< /label >}} 一小时中的分钟，例如：
  ```
  $ task _get 1.due.minute
  0
  ```

* `<date>.second` - {{< label >}}2.4.0{{< /label >}} 一分钟中的秒数，例如：
  ```
  $ task _get 1.due.second
  0
  ```

标签可以作为单个项 `<attribute>` 访问，各个标签可以访问：

* `tags.<literal>` - {{< label >}}2.4.0{{< /label >}} 直接访问，每个标签，例如：
  ```
  $ task _get 1.tag.home
  home
  ```
  如果标签存在，它会被显示，否则结果为空，Taskwarrior 退出时带有非零状态。
  ```
  $ task _get 1.tag.DUE
  DUE
  $ task _get 1.tag.OVERDUE
  ```

  虚拟标签也以相同方式工作。

注释是复合数据结构，有两个元素，分别是 `description` 和 `entry`。
注释由序数访问。

* `annotations.<N>.description` - {{< label >}}2.4.0{{< /label >}} 第 N 个注释的描述，例如：

  $ task _get 1.annotations.0.description

* `annotations.<N>.entry` - {{< label >}}2.4.0{{< /label >}} 第 N 个注释的创建时间戳，例如：
  ```
  $ task _get 1.annotations.0.entry
  ```
  请注意，因为 `entry` 是 `date` 类型，可以寻址各个元素，如上所示，例如：
  ```
  $ task _get 1.annotations.0.entry.year
  2014
  ```

UDA 值可以以相同的方式访问。
