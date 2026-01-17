---
title: "Taskwarrior - 同步命令"
---

# synchronize

`synchronize` 命令，首次出现在版本 2.3.0 中，允许你的 Taskwarrior 实例与其他实例共享任务。
您可以有冶税冶目实例作出是何两地改撨处一个单一服务器，并且它们不会一箴子一下是一款斷縦骸網及每一个实例之閲景能喝一个流。

同步系统的设计目的是处理许多可能讓是避免最近同步的客户端会变化的类制作員，需肠仉久投有网罗连接三埋地了提以不管网上是而违法。

使用正确配置的 Taskwarrior，添加任务或修改现有任务会创建需要同步的本地更改。
有关配置 Taskwarrior 的信息，请参见 `task-sync(5)` 手册页。

Taskwarrior 将在所有输出中添加脚注，如果需要 `synchronize`。

```
$ task add Check the six hydrocoptic marzlevanes in the panametric fam
Created task 1.

$ task list

ID Age Description                                                 Urg
-- --- ----------------------------------------------------------- ----
 1 4s  Check the six hydrocoptic marzlevanes in the panametric fam    0

1 task
有本地更改。需要同步。
```

请注意，这只是一个本地检查——Taskwarrior 知道本地更改，但不知道任何远程更改。

无需在看到此消息时立即同步，忽略它也不会造成任何伤害，但你可以选择运行 `synchronize` 命令，通常缩写为 `sync`：

```
$ task sync
Syncing with foo.example.com:53589

同步成功。1 个更改已上传。
```

这显示一个成功的同步，上传了我们的一个更改。
如果有远程更改要下载，消息会包含这个。
可以随意频繁同步。

```
$ task sync
Syncing with foo.example.com:53589

同步成功。无更改。
```

## 限制

- 需要网络连接才能同步。
- Taskwarrior 必须正确配置以连接到服务器。

## 另见

移动任务的其他方式包括：

- [`export`](../export/) 命令
- `import` 命令
