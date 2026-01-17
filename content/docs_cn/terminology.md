---
title: "Taskwarrior - 术语"
---

# 术语

Taskwarrior 使用许多不明显的术语。
这些术语在这里定义。

## 激活（active）

当一个任务被启动时，像这样：

```
$ task 1 start
```

它获得一个 `start` 属性，这是一个日期。
启动后，任务被认为是*活跃的*，直到完成、删除或停止。

## 别名（alias）

别名是可以在命令行上使用的名称，在运行时被其定义替换。
例如，您可以使用别名为命令创建同义词，如下所示：

```
$ task config alias.rm delete
```

然后每当遇到 `rm` 别名时，它被替换为 `delete`，使这两个命令等效：

```
$ task 1 delete
$ task 2 rm
```

别名可以出现在命令行的其他地方，不限于命令同义词。

## 注释（annotation）

`annotation` 是添加到现有任务的注释，是文本字符串的形式。
任务可以有任意数量的注释。

```
$ task add Buy the milk
$ task 1 annotate and Bread
$ task 1 annotate and Cheese
```

某些报告以完整形式显示注释，某些显示注释计数，某些只显示有注释的指示符。

## 属性（attribute）

任务由一组属性组成，这些属性简单地是名称/值对。
例如，`project` 是一个属性，名称是 `project`，值是您分配给该项目的内容。

```
$ task add project:Home Rake the leaves
```

这里您看到 `project` 属性被直接设置为 `Home`，`description` 属性间接设置为 `Rake the leaves`。

## 计算（calc）

Taskwarrior 包括一个名为 `calc` 的小实用程序，使用 Taskwarrior 表达式引擎来计算值。
此实用程序镜像 `calc` 命令。
这是一个例子：

```
$ calc '1 + 2 * 3'
7
```

`calc` 实用程序主要用于测试，允许访问内部功能，例如后缀表达式：

```
$ calc --postfix '1 2 3 * +'
7
```

虽然这只是一个简单的例子，但也支持日期、持续时间、布尔操作和许多运算符。

详见[计算命令](../commands/calc/)。

## 证书（certificate）

证书是标识实体的文档。
证书是 X.509 PEM 文件，是 [PKI](#pki) 的一部分。

## 命令（command）

Taskwarrior 命令可以是只读命令，其中数据未被修改：

```
list, info, calendar, ...
```

或者它可以是写命令，其中数据被修改：

```
add, modify, done, delete, ...
```

区别很重要，因为它决定了如何解释命令行参数。

## 完成（completed）

`Completed` 是任务所处的几个状态之一。
其他是 `pending`、`waiting`、`deleted` 和 `recurring`。

`completed` 状态表示任务的成功完成。

## 自定义报告（custom report）

Taskwarrior 有几个内置报告，其中一些可以通过配置修改。

此外，您可以创建您自己的自定义报告，其中您可以定义命令、属性显示、长度、筛选器、排序和中断。

[报告](../report/)给出了有关如何使用、修改和创建报告的详细说明。

## 日期格式（dateformat）

[TBD]

## 默认命令（default command）

Taskwarrior 支持默认命令的概念，当命令行上未指定其他命令时使用该概念。
假设您有此设置：

```
$ task config default.command list
$ task +tag
```

然后上面的 `list` 默认命令将在随后的命令中使用，有效地变成：

```
$ task +tag list
```

{{< label >}}2.4.0{{< /label >}} 当未指定任何命令时，唯一的参数是任务 `ID` 或 `UUID` 值，假设 `info` 命令，不管 `rc.default.command` 设置如何。

## 删除（deleted）

`Deleted` 是任务所处的几个状态之一。
其他是 `pending`、`waiting`、`completed` 和 `recurring`。

`deleted` 状态表示从列表中删除的不完整任务，可能是因为它不再相关，或其他人完成了它。

## 自食其果（dogfood）

指"吃自己的狗粮"的短语。
这描述了在开发产品时使用产品的做法。
这从一开始就鼓励产品的改进。

## 截止日期（due）

可以对任务应用截止日期，如下所示：

```
$ task 1 modify due:25th
```

具有截止日期的任务具有更高的紧迫性值，通常在报告中排序较高。
截止日期被认为是必须完成任务的日期。

如果截止日期在未来 7 天内，任务被认为到期。
'7' 可以通过 `due` 配置设置进行配置。
如果截止日期进一步进入未来，则不认为到期。
如果截止日期在过去，则认为逾期，这些任务显示在 `overdue` 报告中。

## 结束（end）

当任务完成或删除时，它会获得一个 `end` 日期。

## 进入（entry）

当首次创建任务时，它会自动分配一个 `entry` 日期。
此属性用于计算任务的年龄。
如果您希望输入旧任务，此日期是可修改的。

## 通过（en-passant）

*En-passant* 用于描述在对任务进行其他更改时注释或以其他方式修改任务的操作。
一个例子是删除任务并同时添加解释性注释。
除了执行这个：

```
$ task 123 annotate Could not reproduce bug
$ task 123 modify +cannot_reproduce
$ task 123 delete
```

这三个操作可以合并：

```
$ task 123 delete Could not reproduce bug +cannot_reproduce
```

对于除 `modify` 外的每个写命令，您可以执行*通过*修改。
`modify` 不同，更新任务描述而不是注释。

## 导出（export）

任务可以使用 `export` 命令从 Taskwarrior 导出。
您可以对导出命令应用筛选器来控制导出的任务。

任务使用 [Taskwarrior JSON 格式](https://github.com/GothenburgBitFactory/taskwarrior/blob/develop/doc/devel/rfcs/task.md) 导出。

```
$ task add Buy milk
$ task /milk/ export
{"description":"Buy milk","entry":"20140928T211124Z","status":"pending","uuid":"2e17b750-c137-4ddd-b6dd-c63e272107f4"}
```

导出的任务可能被导入。
这是为附加脚本和程序编写的推荐机制。

## 表达式（expression）

代数表达式是由常数、运算符和符号组成的数学表达式。
这是 Taskwarrior 命令行上下文中表达式的一些示例：

```
1
1 + 2
1 2 +
123.due + 4 weeks
pi * r^2
```

这种表达式可以出现在筛选器或修改中。

## 扩展（extension）

扩展是向 Taskwarrior 添加功能的程序。
这可以是包装器的形式（[Vit](https://gothenburgbitfactory.org/projects/vit/)）、或别名脚本、或[钩子](#hooks)脚本。

## 筛选器（filter）

筛选器是用于限制显示的任务数量的一组命令行参数。
例如，此命令使用三个筛选条件：

```
$ task project:Home +kitchen +weekend list
```

此命令将显示一个报告，其中包含具有项目值 'Home' 的任务以及具有 `kitchen` 和 `weekend` 标签的任务。
这三个术语与隐含的 `AND` 运算符组合。


此外，上面的命令将上面的三个术语与 `list` 命令的隐含筛选器组合在一起，即：

```
status:pending
```

因此总的来说，该命令有四个筛选条件，限制了显示的任务。
筛选器本质上是一个代数表达式，必须为每个任务求值为 `True`，才能将其包括在结果中。

大多数命令接受筛选器。
详见[筛选器](../filter/)。

## 重建工作集（也称为 GC）

每当必要时，Taskwarrior 都会重建"工作集"。
删除和完成的任务被移出工作集。
此过程保持工作集紧凑和快速读取，并保持 ID 号尽可能低。

虽然可以禁用重建以保持 ID 号静态，但[这被认为是个坏主意](../ids/)。

## 钩子（hooks）

钩子是一种允许其他程序/脚本在 Taskwarrior 执行的某些点运行的机制，以便可以影响处理。
这是一种扩展机制。

详见[钩子设计](../hooks/)文档。

## 中缀（infix）

中缀是运算符出现在其操作数之间的数学表达式符号。
这个例子：

```
1 + 2
```

表示两个数字的总和，`+` 运算符在中缀位置，数字之间。

## 信息（info）

`info` 报告（实际全名 `information`）是以人类可读形式显示*所有*任务元数据的报告。
此外，当提供任务 ID（或 UUID）并且未指定任何命令时，假设 `info` 命令：

```
$ task 123
...
```

## JSON

JSON（JavaScript 对象表示法）是一种行业标准、轻量级的数据交换格式。
人类和机器易于读写。
它得到大多数语言的良好支持。
JSON 是 Taskwarrior 唯一支持的导入/导出格式。
对任何其他格式的支持需要在 JSON 和其他格式之间进行转换。
Taskwarrior 在 `scripts/add-ons` 目录中提供了几个例子。

详见 [www.json.org](https://www.json.org)。

## 密钥（key）

密钥是指定加密密钥的文档。
密钥是 X.509 PEM 文件，是 [PKI](#pki) 的一部分。

## 列表中断（list break）

列表中断与报告排序相关，这样当报告按属性排序时，该属性的列表中断会导致在该属性的唯一值之间插入空行。
例如，排序定义：

```
$ task show report.list.sort

Config Variable  Value
---------------- ------------------------------------
report.list.sort start-,due+,project+/,urgency-
```

这里 `project` 属性按升序排序，也有列表中断，由 `/` 表示。

## 覆盖（override）

覆盖指在运行时取代配置设置的操作。
例如，`rc.color` 的支持默认为 'on'，但在运行时，您可以覆盖这个：

```
$ task rc.color=off list
```

这运行 `list` 报告但不使用颜色。

任何数量的设置都可以在运行时覆盖，覆盖也可以出现在 `rc.report.<report>.filter` 设置中为报告，所以您可以有一个不带颜色的报告运行，例如。

## 待处理（pending）

`Pending` 是任务所处的几个状态之一。
其他是 `deleted`、`waiting`、`completed` 和 `recurring`。

`pending` 状态表示等待完成或删除的不完整任务。

## 公钥基础设施（PKI）

PKI（公钥基础设施）是围绕证书构建的机制和策略的集合，包括它们的创建、验证和处理。

[详见 PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure)

## 准备就绪（ready）

准备就绪是显示准备就绪任务的报告的名称。
如果任务待处理、未被阻止，并且没有[已安排](#scheduled)日期，或者有一个在现在之前的，则认为任务*准备就绪*。

## 正则表达式（regex）

正则表达式或**正**则**表**达式是一种非常强大的搜索和匹配文本的机制。
这是一种描述搜索模式的复杂语言，允许进行复杂搜索。

假设您正在解决纵横字谜，需要一个以元音开头和结尾的五字母单词，您可以在正则表达式中表示它，它可能看起来像：

```
[aeiou]...[aeiou]
```

详见 [Wikipedia：正则表达式](https://en.wikipedia.org/wiki/Regular_expression)。
Taskwarrior 使用 [POSIX 扩展正则表达式](https://en.wikibooks.org/wiki/Regular_Expressions/POSIX-Extended_Regular_Expressions)。

## 反向波兰表示法（RPN）

**反**向**波**兰**表**示法或后缀表示法是一种数学表示法。
这个例子：

```
1 2 +
```

表示两个数字的总和，`+` 运算符在后缀位置，最后。

编写软件来评估后缀表达式比中缀表示法容易得多，这对人类更容易阅读。

## 已安排（scheduled）

`scheduled` 日期代表开始处理任务的第一个机会，因此通常设置为在[等待](#wait)日期之后，但在[截止](#due)日期之前。

如果您向任务添加已安排的日期，如下所示：

```
$ task 1 modify scheduled:2014-12-31
```

那么此任务将在该日期后被认为*准备就绪*。
[虚拟标签](#vtag) `READY` 反映了这一点，报告 `ready` 显示此类任务。

## 外壳（shell）

外壳是您的命令解释器。
您最可能使用 `bash` 外壳，但还有许多其他外壳。

任何具有复杂参数处理的程序都可能面临与外壳相关的几个问题。
外壳在将输入的文本传递给程序之前修改它。
例如，考虑这两个命令：

```
$ ls my_file
$ ls 'my_file'
```

在这两种情况下，`ls` 命令看到相同的命令行参数 `my_file`，因为外壳移除了引号。
还有许多其他情况也需要注意。

## 影子文件（shadow file）

影子文件是 Taskwarrior 的一个旧功能，每当数据更改时都会自动运行报告。
该报告是可配置的，输出放在文本文件中。
这通常用于使用[Conky](https://github.com/brndnmtthws/conky)或类似工具在桌面上显示任务列表。

从版本 2.4.0 开始，影子文件支持已被删除，取而代之的是基于钩子的解决方案，其示例随源代码提供。

## 特殊标签（special tag）

特殊标签是应用于任务时改变 Taskwarrior 对该任务的行为的标签。
支持多个特殊标签：
  ---------- ———————————————————————————————————————
  +nocolor   为此任务禁用颜色规则处理
  +nonag     此任务的完成会抑制所有打扰消息
  +nocal     此任务不会出现在日历上
  +next      提升任务，使其出现在 'next' 报告上
  ---------- ———————————————————————————————————————

详见[标签](#tag)、[虚拟标签](#vtag)。

## 启动（start）

如果对任务应用 `start` 日期，则该任务被认为*活跃*。
活跃任务具有更高的紧迫性值，可以使用 `active` 命令列出。
有两种方法来应用启动日期，后者给予您记录的启动日期的控制：

```
$ task 1 start
$ task 1 modify start:now
```

如果配置设置 `journal.info` 打开，那么当任务启动或停止时，会为此目的进行[注释](#annotation)。
累计的启动/停止时间可以在[信息](#info)报告中查看。

请注意，这并不意味着 Taskwarrior 中存在时间跟踪，这只是启动/停止时间的日志，不可修改。
许多用户将此日志与合法的时间跟踪能力混淆。
不要犯这个错误。

## 替换（substitution）

Taskwarrior 可以进行命令行替换，如下所示：

```
$ task 123 modify /this/that/
```

替换语法允许在任务描述和注释中进行文本替换。

## 同步（sync）

同步是一个过程，允许多个 Taskwarrior 实例共享数据。
每个实例定期连接到服务器以收集其他实例所做的更改，并发送本地所做的更改。
这通过 `task sync` 命令启动。

## 标签（tag）

任务可能附加了任何数量的标签。
标签是单个单词（无空格），可以在创建任务时包括：

```
$ task add Paint the walls +home +weekend
```

这向任务添加两个标签 `home` 和 `weekend`。
您可以修改任务以添加 `renovate` 标签：

```
$ task 1 modify +renovate
```

或删除 `weekend` 标签：

```
$ task 1 modify -weekend
```

详见[特殊标签](#stag)、[虚拟标签](#vtag)。

## 主题（theme）

Taskwarrior 主题是包含颜色定义的文件。
Taskwarrior 提供了多个主题，这些主题在您的 `.taskrc` 文件中列出，如果您取消注释其中一条线，则包含该主题，您会看到一组所选和主题颜色。
您的 `.taskrc` 文件应包含此部分：

```
# Color theme (uncomment one to use)
#include /usr/local/share/doc/task/rc/light-16.theme
#include /usr/local/share/doc/task/rc/light-256.theme
#include /usr/local/share/doc/task/rc/dark-16.theme
#include /usr/local/share/doc/task/rc/dark-256.theme
#include /usr/local/share/doc/task/rc/dark-red-256.theme
#include /usr/local/share/doc/task/rc/dark-green-256.theme
#include /usr/local/share/doc/task/rc/dark-blue-256.theme
#include /usr/local/share/doc/task/rc/dark-violets-256.theme
#include /usr/local/share/doc/task/rc/dark-yellow-green.theme
#include /usr/local/share/doc/task/rc/dark-gray-256.theme
#include /usr/local/share/doc/task/rc/dark-default-16.theme
#include /usr/local/share/doc/task/rc/dark-gray-blue-256.theme
```

所有这些行都被称为"被注释掉"，这意味着行开始处的 '#' 符号阻止 Taskwarrior 读取该行的其余部分。
删除 '#' 启用颜色主题。
例如，要启用 `dark-violets-256.theme` 文件，请将上面的内容更改为如下所示：

```
# Color theme (uncomment one to use)
#include /usr/local/share/doc/task/rc/light-16.theme
#include /usr/local/share/doc/task/rc/light-256.theme
#include /usr/local/share/doc/task/rc/dark-16.theme
#include /usr/local/share/doc/task/rc/dark-256.theme
#include /usr/local/share/doc/task/rc/dark-red-256.theme
#include /usr/local/share/doc/task/rc/dark-green-256.theme
#include /usr/local/share/doc/task/rc/dark-blue-256.theme
include /usr/local/share/doc/task/rc/dark-violets-256.theme
#include /usr/local/share/doc/task/rc/dark-yellow-green.theme
#include /usr/local/share/doc/task/rc/dark-gray-256.theme
#include /usr/local/share/doc/task/rc/dark-default-16.theme
#include /usr/local/share/doc/task/rc/dark-gray-blue-256.theme
```

## 用户定义属性（UDA）

Taskwarrior 支持大约 20 个左右的核心属性集合，其中包括 ID、描述、标签、截止日期等。
这些属性对于提供的基本功能是必要的。

但假设您想跟踪时间估算。
虽然没有该属性，但您可以将数据存储在属性中。
更好的解决方案是用户定义的属性，名为 `estimate`，类型为 `numeric`，您通过配置定义它，如下所示：

```
$ task config uda.estimate.label Estimate
$ task config uda.estimate.type duration
```

定义 UDA 后，您可以像使用任何其他核心属性一样使用它，如下所示：

```
$ task add Paint the door due:eom estimate:4hours
```

Taskwarrior 将忠实地存储、报告、排序和筛选 UDA 属性，但当然不理解它们的真实含义，因此这些值周围没有功能。

UDA 可能对紧迫性计算有贡献，使用：

```
urgency.uda.<name>.coefficient
```

UDA 可能具有默认值（`uda.<name>.default`）、允许值（`uda.<name>.values`）、类型（`string`、`date`、`duration` 和 `numeric`）以及报告标题的标签（`uda.<name>.label`）。

详见[用户定义属性（UDA）](../udas/)。

## 过期（until）

`until` 日期表示任务自动删除的日期。
这是一个到期日期。
如果您添加一个直到日期，如下所示：

```
$ task 1 modify until:eoy
```

那么此任务将在年底过期并消失。

## 紧迫性（urgency）

任务的紧迫性是一个数值，代表传达重要性的任务属性。
考虑这两个任务：

```
$ task add Buy the paint   due:saturday +home +renovate +store
$ task add Paint the walls due:sunday   +home +renovate
```

第一个任务到期较早，毫无疑问它比第二个任务更重要，毕竟，第二个任务被第一个任务阻止。
这里可以使用依赖项来表示这一点，但这只是一个例子。

我们希望第一个任务在排序列表中排列在第二个任务之上，在这种情况下是因为截止日期。
但并不是每个任务都有（或需要）截止日期。
因此，紧迫性是在排序列表中提升和降低任务的一种方式。
这无法通过简单地提供复杂的排序顺序来实现，因为每个人的使用模式都不同。

紧迫性实现为多项式，其中术语权重是可配置的，使属性成为紧迫性的重要贡献者（高正数）、边际贡献者（低正数）、分离器（负数）或完全无影响（零）。

虽然紧迫性价值本身是无意义的，但与其他任务相比较时，有助于按重要性排序任务。

`urgency` 属性在 `next` 报告中特别介绍，虽然它可以在任何报告中使用。

由于多项式术语权重（系数）都是可修改的，您可以调整它们使紧迫性值更接近您自己对重要性的概念。
如果您理解系数的使用方式，这可能会非常有益。

详见[紧迫性](../urgency/)。

## 通用唯一标识符（UUID）

**通**用**唯**一**标**识符是一个 128 位数字，用于唯一标识任务。
虽然任务的 ID 只是"工作集"中的行号，但 UUID 是永久的。

您可以互换使用 ID 和 UUID，尽管 ID 更加方便。

```
$ task _get 1.uuid
8ad2e3db-914d-4832-b0e6-72fa04f6e331
$ task _get 8ad2e3db-914d-4832-b0e6-72fa04f6e331.id
1
```

UUID 永不改变，不可修改。

## 冗长度（verbosity）

冗长度指 Taskwarrior 生成的输出。
例如，如果您运行报告，输出可能看起来像这样（为清楚起见排除了数据）：

```
$ task next rc.color=off

ID  Age  Project Tag Description               Urg
--- ---- ------- --- ------------------------ ----
  1 2wks    Home     Paint the walls           1.2
...

31 tasks
Configuration override rc.color:off
Filter: ( ( status == pending ) )
```

除了实际数据外，还有初始空行、一组列标题、另一个空行、摘要 '31 tasks'、配置覆盖和使用的筛选器。
所有这些装饰元素等都由 `verbose` 配置设置中的冗长度设置控制。

详见[冗长度](../verbosity/)。

## 虚拟标签（virtual tag）

筛选器的标签表示法很方便和简单，例如：

```
$ task +HOME list
...
$ task -HOME list
```

第一个列出仅具有 `HOME` 标签的任务，第二个列出仅不具有 `HOME` 标签的任务。

虚拟标签是执行简单筛选的一种方式，使用在运行时合成的标签，并表示任务的复杂内部状态。
例如，虚拟标签 `ACTIVE` 是筛选已启动的任务的简单方法，这样此命令：

```
$ task +ACTIVE list
```

是表示此等效命令的更友好和简单的方式：

```
$ task start.any: list
```

详见[标签](#tag)、[特殊标签](#stag)。

## 等待（wait）

如果您向任务添加 `wait` 日期，如下所示：

```
$ task 1 modify wait:30th
```

然后该任务从报告中隐藏，直到该月的 30 日，此时等待日期被删除，任务变为可见。

这对于远期的任务很有用，您可能不欣赏在每个列表上看到该任务，至少直到它接近截止日期。

请注意，使用此功能不需要截止日期。

## 等待状态（waiting）

`waiting` 命令是显示当前因为有未来 `wait` 日期而隐藏的任务的报告。

`Waiting` 也是任务所处的几个状态之一。
其他是 `pending`、`completed`、`deleted` 和 `recurring`。

`waiting` 状态表示从视图中隐藏的待处理任务。
