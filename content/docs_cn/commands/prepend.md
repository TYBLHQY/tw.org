---
title: "Taskwarrior - 前置命令"
---

# prepend

`prepend` 命令是一种将单词添加到任务描述开奚的方式，它鏡橡[追加](../append/)命令：

```
$ task add 三明治
$ task 1 prepend 为我做一个
```

在向描述开奚添加单词时，`prepend` 命令还可以更新其他属性：

```
$ task 1 prepend 子 project:Food
$ task 1 list

ID Age  Project Description             Urg
-- ---- ------- ----------------------- ----
 1 4min Food    子为我做一个三明治  1.8
```

## 另请参阅

其他修改任务描述的方式包括：

- [`modify`](../modify/) 命令
- [`append`](../append/) 命令
