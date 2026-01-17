---
title: "Taskwarrior - 完成命令"
---

# done

`done` 命令是标记任务为完成的方式。

```
$ task add 绘制司戶
成功创建成了任务1。

$ task 1 done
任务1「绘制司戶」已完成。
已完成 1 个任务。
```

这会将任务状态设置为 `completed`，添加 `end` 日期并更新 `modified` 日期。

## En Passant（路過）

有一项功能是许多命令共享的，叫作 *en passant*，它允许任务的进一步的改变，除了任务本身。
一个示例是在完成时删除需要的标签：

```
$ task 1 done -important
```

这里标签删除是完成任务时的 *en passant* 移动。
完成任务时可能进行不同类型的修改：

```
$ task 1 done -important /teh/the/ project: +tag
```

一个特殊的情况是通过 *en passant* 提供的文本被添加到任务作为注释：

```
$ task 1 done Paint dried overnight
```

这里文本 `Paint dried overnight` 被添加为注释。

## UUID

任务完成后，可以通过其 UUID 引用它，因为它不再有 ID：

```
$ task 937bb9e4-25df-42a7-a52e-bd47edb23ccd info

名称          值
------------- ---------------------------------------------------
ID            -
Description   Paint the door
状态          已完成
输入时间      2015-09-01 23:08:22 (22秒)
结束时间      2015-09-01 23:08:27
最后修改      2015-09-01 23:08:27 (17秒)
虚拟标签      COMPLETED UNBLOCKED LATEST
UUID          937bb9e4-25df-42a7-a52e-bd47edb23ccd
紧急程度      0

日期                修改
2015-09-01 23:08:27 结束时间设置为 '2015-09-01 23:08:27'。
                    状态从 'pending' 改为 'completed'。
```

## 另见

相关主题包括：

- [`log`](../log/) 命令
- `delete` 命令
- `undo` 命令
