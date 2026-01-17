---
title: "Taskwarrior - 信息命令"
---

# info

`info` 命令（完整的命令名是 `information`）是以人类可读的形式显示所有任务元数据的方式。
这包括[UDAs](../../udas/)。

```
$ task 1 info

Name          Value
------------- ------------------------------------------------------------------
ID            241
Description   Need to map stored duration values to the supported subset on load
Status        Pending
Project       tw.240
Entered       2014-09-19 11:32:22 (2 weeks)
Last modified 2014-09-19 11:32:22 (2 weeks)
Tags          bug
Virtual tags  PENDING READY TAGGED UNBLOCKED
UUID          91bbb01f-4a43-42bd-a7a3-03ce3a2451ff
Urgency       4.882
                           project     1  *    1 =     1
                              tags   0.8  *    1 =   0.8
                               age 0.041  *    2 = 0.082
                           TAG bug     1  *    3 =     3

Date                Modification
------------------- ------------------
2014-09-27 11:01:02 Tags set to 'bug'.
```

Taskwarrior {{< label >}}2.4.0{{< /label >}} 也会显示紧急程度计算的详细信息，如上所示。

Taskwarrior {{< label >}}2.4.2{{< /label >}} 也会显示有效的 [虚拟标签](../../tags/) 列表。

`info` 命令也接受 `UUID` 来识别任务，所以你也可以检查已完成和删除的任务。

```
$ task 91bbb01f-4a43-42bd-a7a3-03ce3a2451ff info

名称          值
------------- ------------------------------------------------------------------
ID            241
描述          需要将存储的持续时间值映射到加载时支持的子集
状态          待处理
项目          tw.240
输入时间      2014-09-19 11:32:22 (2周)
最后修改      2014-09-19 11:32:22 (2周)
标签          bug
虚拟标签      PENDING READY TAGGED UNBLOCKED
UUID          91bbb01f-4a43-42bd-a7a3-03ce3a2451ff
紧急程度      4.882
                           project     1  *    1 =     1
                              tags   0.8  *    1 =   0.8
                               age 0.041  *    2 = 0.082
                           TAG bug     1  *    3 =     3

日期                修改
------------------- ------------------
2014-09-27 11:01:02 标签设置为 'bug'。
```

`info` 命令也接受过滤器，例如搜索：

```
$ task /duration\\ values/ info
...
```

Taskwarrior {{< label >}}2.4.0{{< /label >}} 会在未指定命令且唯一的参数是 ID 或 UUID 时使用 `info` 命令，例如：

```
$ task 242
...
```

## 配置

日期格式可以使用 `dateformat.info` 覆盖。

将 `journal.info` 设置为 'yes' 会显示任务的更改日志，如上面的示例所示。

## 另见

检查任务的其他方式包括：

- [`edit`](#) 命令
- [`export`](#) 命令
