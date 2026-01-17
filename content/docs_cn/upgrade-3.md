---
title: "Taskwarrior - 升级到 Taskwarrior 3"
---

# 升级到 Taskwarrior 3

Taskwarrior 3 感起来很像 Taskwarrior 2.x：命令行界面没有改变。
但是，任务的存储和同步方式已经完全重写。
要升级，你需要将任务数据导入到 Taskwarrior 3 中。
请继续阅读以获取详细信息。

## 升级你的任务

安装 Taskwarrior 3.3.0 或更高版本，然后导入你的任务。
Taskwarrior 钩子在导入期间运行，因此在此调用中使用 `rc.hooks=0` 禁用它们：

```
task import-v2 rc.hooks=0
```

### 任务存储

Taskwarrior 2.x 在具有 `.data` 后缀的文件中存储任务。
Taskwarrior 3.x 在你的任务目录中的 `taskchampion.sqlite3` 中存储任务。

导入任务后，你可以备份或删除该目录中的 `*.data` 文件，因为它们不再使用。
只要 `*.data` 文件仍然存在，Taskwarrior 就会在每次运行时发出警告。

## 升级同步

Taskwarrior 2 使用 `taskd` 作为其同步的服务器。
Taskwarrior 3 有一个完全新的同步实现，不与 `taskd` 接口。
如果你一直在同步任务，你需要采用新的方法。

从 Taskwarrior 3.0 开始，推荐的解决方案是使用云存储后端。
有关如何配置新同步实现的详细信息，请参见 `task-sync(5)` 手册页。
