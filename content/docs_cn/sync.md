---
title: "Taskwarrior - 任务同步"
---

# 任务同步

Taskwarrior 支持在多个"副本"之间"同步"任务。
例如，你可能需要在笔记本电脑和台式机之间同步任务。

## 同步概览

使用 `task sync` 命令将任务的变更同步到服务器。
此命令发送任何新的本地更改到服务器，并应用服务器上它之前没有看到的任何更改。
一个变更从一个副本到达另一个副本需要两次 `task sync` 调用：一次在源上发送变更；一次在目标上接收变更。

例如，在笔记本电脑上：
```
$ task add +homework finish odd exercises in section 3.2
$ task sync
```

然后在台式机上：
```
$ task sync
$ task +homework list
ID Age  Tags     Description                             Urg 
 3 2h   homework finish odd exercises in section 3.2      0.8
```

注意：同步机制由 [TaskChampion](https://github.com/GothenburgBitFactory/taskchampion) 实现。
因此，任何使用 TaskChampion 的工具都可以与 Taskwarrior 同步。

### 不要使用外部同步工具

过去版本的 Taskwarrior 使用了更简单的格式来存储任务数据，通过谨慎使用可以使用 Syncthing 或 `rsync` 等文件同步工具来同步任务。
从 Taskwarrior 3.0 开始，任务存储在本地数据库中，格式不适合进行这种同步。
虽然它们可能在大多数时间都有效，但这样的配置可能会导致损坏，不被支持。

## 如何同步

要设置同步，你需要一个服务器。
有两个选项可用：

 - 商业云存储提供商之一；或
 - 在互联网可访问的系统上运行 TaskChampion 同步服务器，该系统可以是你自己的或由他人提供。

Taskwarrior 可以使用云存储提供商（如 AWS S3 或 Google Cloud Storage）来同步任务。
这需要少量费用，还需要在云提供商配置方面拥有专业知识。
也支持一些与 S3 兼容的存储服务。

[TaskChampion 同步服务器](https://github.com/GothenburgBitFactory/taskchampion-sync-server)在诸如 VPS 或网络托管提供商等服务器上运行很简单。
它可以以多种方式部署，并且可以简单地为单个用户配置或扩展以支持多个用户。
[同步服务器文档](https://gothenburgbitfactory.org/taskchampion-sync-server/)详细描述了服务器配置选项。

有关配置同步的详细信息，请参阅 [`task-sync(5)`](https://taskwarrior.org/docs/man/task-sync.5/) 手册页。
