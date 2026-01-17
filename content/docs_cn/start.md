---
title: "Taskwarrior - 下一步是什么?"
---

## 什么是 Taskwarrior?

Taskwarrior 从命令行管理您的待办事项列表。
它灵活、快速、高效、不显眼，完成其工作后自动退出。

Taskwarrior 可以扩展以适应您的工作流程。
将其用作简单应用程序来捕获任务、显示列表以及移除任务。
但如果您充分利用其功能，它会成为一个复杂的数据查询工具，帮助您保持组织有序并完成工作。

Taskwarrior 是一个活跃的项目，我们几乎每天都在修复错误、改进和添加功能。

## 为什么选择 Taskwarrior?

使用 Taskwarrior 的五个好理由

1. 您是否在寻找一个不显眼、快速、高效、灵活的命令行工具来轻松管理任务列表?
   Taskwarrior 在设计上具有低摩擦力，可让您捕获详细信息，然后立即回到工作。
   ```
   $ task add Prepare the first draft of the proposal due:friday
   ```
   Taskwarrior 使用自然而富有表现力的命令行语法。

2. Taskwarrior 是方法论中立的。
   无论您遵循 [GTD](https://gettingthingsdone.com)、使用 [番茄工作法](https://www.pomodorotechnique.com/)，还是只是做对您有用的事情，Taskwarrior 都提供功能来帮助您，而不是限制您。

3. Taskwarrior 拥有一个活跃而友好的社区，为新手和有经验的用户提供支持和各种形式的帮助。[从这里开始](../../support/) 获取支持选项列表。
   需要立即获得答案 - 查看您的手册页和[在线文档](../)。
   需要向某人提问? 查看我们的 [Discord 服务器](https://discord.gg/eRXEHk8w62)。

4. Taskwarrior 在尽可能多的方面是开放的:
   - 它是[免费且开源](https://github.com/GothenburgBitFactory/taskwarrior)的，使用 MIT 许可证
   - 它使用 SQLite 数据库文件进行存储，易于读取。可以进行直接修改，但由于内部一致性要求，不建议这样做。
   - 它导入和导出 [JSON](https://github.com/GothenburgBitFactory/taskwarrior/blob/develop/doc/devel/rfcs/task.md)，因此您的数据永远不会被关押
   - 有 [DOM](../dom/) 访问和 [Hook 脚本 API](../hooks/)
   - 有许多可用的免费开源[扩展脚本](../../tools/)
   - 有 [Vit](https://gothenburgbitfactory.org/projects/vit/)，一个基于 curses 的 UI
   - 有 [BugWarrior](https://github.com/GothenburgBitFactory/bugwarrior)，因此您可以从十多个不同的 bug 系统导入您的 bug 问题

5. Taskwarrior 是一个活跃而充满活力的项目。
   去年，它平均每天有 5.58 项更改。
   Taskwarrior 享受许多贡献者的活跃参与，目前有超过 60 名代码补丁提供者。
   但还有更多贡献者 (252 名) 帮助进行文档编制、错误修复、支持、想法、请求和扩展。
   它只会继续变得更好。

## 快速演示

让我们看看 Taskwarrior 的实际应用。
我们首先向列表中添加几个任务。

```
$ task add Buy milk
Created task 1.

$ task add Buy eggs
Created task 2.

$ task add Bake cake
Created task 3.
```

现在让我们看看列表。

```
$ task list

ID Description
-- -----------
1  Buy milk
2  Buy eggs
3  Bake cake

3 tasks.
```

假设我们已经购买了材料，并希望将前两个任务标记为完成。

```
$ task 1 done
$ task 2 done
$ task list

ID Description
-- -----------
1  Bake cake

1 task.
```

这些是前三个功能，即 `add`、`list` 和 `done` 命令，但它们代表了开始使用 Taskwarrior 真正需要了解的全部内容。

但还有数百个其他功能，所以如果您学到更多，您就能做得更多。
您可以完全自由选择如何使用 Taskwarrior：坚持上面的简单三个命令，或了解复杂的过滤、自定义报告、用户定义的元数据、颜色规则、hook 脚本、同步等等。

## 获取您的副本

有几种方法可以获取 Taskwarrior 的副本:

- 安装二进制软件包。
  您的操作系统可能已经有可用的二进制软件包。
  这些软件包通常名称为 'task'。
- 从 [这里](../../download/) 下载发布 tarball，然后确保您已安装 libuuid-dev (可能称为 uuid-dev)。
  然后使用 cmake、GCC 4.7 / Clang 3.3，[构建 Taskwarrior](../build/)。
- 使用 [git](https://git-scm.com) 克隆代码存储库，切换到当前开发分支，并 [构建 Taskwarrior](../build/)。

## 下一步呢?

可能最重要的下一步就是简单地开始使用 Taskwarrior。
捕获您的任务，不要试图记住它们。
定期检查任务列表以保持更新。
查阅任务列表来指导您的操作。
养成这个习惯。

不久，您会意识到您可能想要修改您的工作流程。
也许您缺少截止日期，需要更明确的期限。
也许您需要更充分地利用标签来帮助您以不同方式过滤任务。
您会知道您的工作流程是否不能像它应该能够的那样帮助您。

此时，您可能会更仔细地查看 [文档](../) 和推荐的 [最佳实践](../best-practices/)。

欢迎使用 Taskwarrior。
