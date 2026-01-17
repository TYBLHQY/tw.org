---
title: "Taskwarrior - 追加命令"
---

# append

`append` 命令是将单词添加到任务描述末尾的一种方式：

```
$ task add 修复管道
$ task 1 append 在房子被淹之前
```

在向描述末尾添加单词时，`append` 命令还可以更新其他属性：

```
$ task 1 append 并在冬天之前 project:Home +repair
$ task 1 list

ID Age  Project Tags   Description                                            Urg
-- ---- ------- ------ ------------------------------------------------------ ----
 1 4min Home    repair 修复管道，在房子被淹之前，冬天之前  1.8
```

## 另请参阅

其他修改任务描述的方式包括：

- [`modify`](../modify/) 命令
- [`prepend`](../prepend/) 命令
