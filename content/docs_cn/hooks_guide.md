---
title: "Taskwarrior - 钩子作者指南"
---

# 钩子作者指南

本指南介绍了编写和测试 Taskwarrior 钩子脚本的过程。
虽然对于开发人员来说这是一个简单而直接的过程，但仍然有许多注意事项。
将开发一个钩子脚本，并讨论各种问题。

## 示例钩子脚本

例如，我们将创建一个钩子脚本，该脚本检测描述中引用 Taskwarrior 错误号的任务（即"TW-179"），并将错误号替换为链接到错误的 URL。
每当在任务描述中找到模式 `tw-179` 时，它应该更改为 `https://github.com/GothenburgBitFactory/taskwarrior/issues/179`。

此脚本只需搜索模式并执行替换，仅用于新任务。
这将是一个非常简单的钩子脚本。

## 选择一种语言

您可以使用任何语言编写钩子脚本。
但还有更多要考虑的：

- 性能是否是一个问题？
  您可能不需要担心性能，因为添加或修改任务所花费的时间是一次性成本。
  如果它影响报告，性能会更重要。
- 您的语言是否有易于使用的 JSON 解析器？
  很可能是这样，但用户的机器上是否安装了它？
  您是否会要求用户安装其他软件？
- 如果您考虑编译语言，您将发送源代码还是二进制文件？
  开发人员通常安装了编译器，但普通用户没有。
  发送二进制文件意味着您需要为不同的操作系统和版本提供二进制文件。

此示例将用 Python 2.6+ 编写，因为 Python 众所周知、现代且普遍可用。
它有一个内置的 JSON 解析器。
这对于此任务是理想的。

## 钩子 API

阅读并理解[钩子 API](../hooks/)。
这很重要，因为钩子脚本必须符合 API 要求。
Taskwarrior 对合规性要求严格。
钩子脚本有能力伤害数据，所以它们受到仔细监视。

## 框架

从 API 中，我们知道 `on-add` 钩子脚本将需要从标准输入读取一行 JSON，并发出可选修改的一行 JSON，可选包括反馈，并以零状态退出以指示成功。

首先，这是一个兼容的 `on-add` 钩子脚本，什么都不做，*但它做对了*。它可以是任何 `on-add` 脚本的基础。

```
#!/usr/bin/env python

import sys
import json

added_task = json.loads(sys.stdin.readline())
print(json.dumps(added_task))
sys.exit(0)
```

此脚本从输入读取一行 JSON 并解析它。
此 JSON 表示正在添加的任务。
然后将 JSON 序列化并写入输出，不进行修改。
零退出代码表示接受添加的任务。

虽然此脚本对任务什么都不做，但它只需要添加几行即可完成。

## 测试

您可以从 Taskwarrior 独立测试钩子脚本，这是个好主意。
首先我们使脚本可执行，然后我们从命令行运行它并向其提供样本 JSON。
这是一个示例测试运行，使用有效的 JSON，但它不是有效的任务 - 它只是一个测试。

```
$ chmod +x hook.py
$ echo '{"name":"value"}' | ./hook.py
{"name": "value"}
$ echo $?
0
```

在这里，钩子脚本被设为可执行，然后样本 JSON `{"name":"value"}` 作为输入提供。
脚本将 JSON 输出未修改，退出代码为零。
此脚本有效。

现在我们向脚本添加逻辑来做一些事情。

## 实现

对于实现，脚本需要查找错误号。
Taskwarrior 错误号可以用正则表达式表示，如下所示：

```
\b(tw-\d+)\b
```

脚本现在被修改为 `import re`，并在描述属性上执行替换。
通过将原始描述与修改后的描述进行比较，脚本知道何时提供反馈。
这是更新的脚本：

```
#!/usr/bin/env python

import sys
import re
import json

added_task = json.loads(sys.stdin.readline())
original = added_task['description']
added_task['description'] = re.sub(r'\b(tw-\d+)\b',
                                   r'https://github.com/GothenburgBitFactory/taskwarrior/issues/\1',
                                   original,
                                   flags=re.IGNORECASE)
print(json.dumps(added_task))

if original != added_task['description']:
    print 'Link added'

sys.exit(0)
```

再次使用更好的输入进行测试会产生：

```
$ echo '{"description":"foo tw-179 bar"}' | ./hook.py
{"description": "foo https://github.com/GothenburgBitFactory/taskwarrior/issues/179 bar"}
Link added
$
$ echo $?
0
```

脚本已正确识别错误号，并将其替换为正确的 URL。
反馈消息指示这一点。
我们准备安装此钩子脚本并使用 Taskwarrior 进行测试。

## 安装和启用

要安装脚本，将其复制到 `~/.task/hooks` 目录，如有必要，创建该目录，并确保脚本可执行。
它还必须与事件相关联，这可以通过将其命名为 `on-add*` 来完成。

```
$ mkdir -p ~/.task/hooks
$ cp hook.py ~/.task/hooks/on-add-bug-link.py
$ chmod +x ~/.task/hooks/on-add-bug-link.py
```

有一个配置设置启用/禁用钩子，您需要确保启用钩子，尽管这是默认值：

```
$ task _get rc.hooks
on
```

现在运行 `diagnostics` 命令，它将总结它找到的钩子：

```
$ task diag
...
Hooks
    Scripts: Enabled
             <user>/.task/hooks/on-add-bug-link.py (executable)
...
```

我们看到 Taskwarrior 发现了钩子脚本。
现在让我们看看它的运作情况，请注意 `--` 终止符正被使用，所以 `tw-179` 不被视为数学表达式：

```
$ task add -- Contains no bug number
Created task 181.
$ task add -- Fix tw-179
Created task 182.
Link added
$
$ task _get 182.description
Fix https://github.com/GothenburgBitFactory/taskwarrior/issues/179
```

它有效，但我们在这里做了最少的测试。
如果您编写具有任何非平凡能力的钩子脚本，您的测试应该更加彻底。
这仅仅是一个例子。

## 调试

Taskwarrior 有一个钩子调试配置设置，它将显示 Taskwarrior 如何处理钩子输入和输出、发生了什么以及花费了多长时间。
这是一个类似的任务添加的调试信息请求。
输出经过编辑，仅显示相关的钩子信息。

```
$ task rc.debug.hooks=2 add -- Fix tw-98765
...
Found hook script <user>/.task/hooks/on-add-bug-link.py
...
Hook: Calling <user>/.task/hooks/on-add-bug-link.py
Hook: input
  {"description":"Fix tw-98765","entry":"20150301T154518Z","modified":"20150301T154518Z","status":"pending","uuid":"daa3ff05-f716-482e-bc35-3e1601e50778"}
Timer Hooks::execute (<user>/.task/hooks/on-add-bug-link.py) 0.031061 sec
Hook: output
  {"status": "pending", "entry": "20150301T154518Z", "uuid": "daa3ff05-f716-482e-bc35-3e1601e50778", "description": "Fix https://github.com/GothenburgBitFactory/taskwarrior/issues/98765", "modified": "20150301T154518Z"}
  Link added
Hook: Completed with status 0
...
Perf task 2.4.2 f0cc015 20150301T154759Z init:3388 load:2001 gc:0 filter:0 commit:230 sort:0 render:0 hooks:33565 total:39184

Created task 183.
Configuration override rc.debug.hooks:2
Link added
```

输出显示找到了钩子脚本并运行了它，显示了输入和输出以及时序信息、反馈和状态。

在这种情况下，钩子脚本在 31 毫秒内运行，这当然足够快，不会导致用户想知道发生了什么。
在这个例子中，所有钩子处理在 33 毫秒内完成。

## 发布

完成钩子脚本后，您会分享您的脚本吗？
这当然是可选的，但如果您这样做，请考虑许可证和版权，建立网络存在，以便可以找到并下载它，也许在脚本中放入联系信息，以便您能被告知问题，然后告诉人们关于它。

您可以告诉我们您的钩子脚本，因为我们很想在[工具](../../tools/)页面上列出它，以及许多其他脚本。
