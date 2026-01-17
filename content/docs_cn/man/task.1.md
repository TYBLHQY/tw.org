---
title: 'Taskwarrior - task.1 for task-3.4.2 (中文)'
viewport: 'width=device-width, initial-scale=1'
layout: single
---
# 名称

task - 一个命令行任务管理器。

# 概述

**task &lt;filter&gt; &lt;command&gt; \[ &lt;mods&gt; | &lt;args&gt;
\]**  
**task --version**

# 描述

Taskwarrior 是一个命令行任务管理器。它维护一个您需要做的任务列表，允许您添加/修改任务，并跟踪他们的状态。
Taskwarrior 不仅仅是一个任务列表。

本质上，Taskwarrior 是一个列表处理程序。您添加文本和相关的其他参数，然后以整洁的方式重新显示信息。当您添加到期日期和循环规则时，它会变成待办事项列表程序。当您添加优先级、标签（一个词的描述符）、项目组等时，它会变成一个有组织的待办事项列表程序。

# 筛选

&lt;filter&gt; 由零个或多个搜索条件组成，用于选择任务。例如，要列出所有属于"Home"项目的待处理任务：

      task project:Home list

您可以指定多个筛选条件，每个条件都会进一步限制结果：

      task project:Home +weekend garden list

此示例应用了三个筛选器："Home"项目、"weekend"标签，以及描述或注释必须包含字符序列"garden"。在此示例中，"garden"在内部被转换为：

      description.contains:garden

作为一个方便的快捷方式。这里的"contains"是一个属性修饰符，用于对筛选器进行比简单的存在或不存在更多的控制。有关修饰符的完整列表，请参阅下面的"属性修饰符"部分。

注意，筛选器可能没有任何条件，这意味着所有任务都适用于该命令。这可能很危险，此特殊情况需要确认，无法覆盖。例如，此命令：

      task modify +work
      此命令没有筛选器，将修改所有任务。您确定吗？(yes/no)

将向所有任务添加"work"标签，但仅在确认后才会执行。

更多筛选器示例：

      task                                      <command> <mods>
      task 28                                   <command> <mods>
      task +weekend                             <command> <mods>
      task +bills due.by:eom                    <command> <mods>
      task project:Home due.before:today        <command> <mods>
      task ebeeab00-ccf8-464b-8b58-f7f2d606edfb <command> <mods>

默认情况下，筛选器元素使用隐式"and"运算符组合，但也可以使用"or"和"xor"，条件是包括括号：

      task '( /[Cc]at|[Dd]og/ or /[0-9]+/ )'      <command> <mods>

括号将逻辑术语与任何默认命令筛选器或隐式报告筛选器隔离，这些将使用隐式"and"组合。

筛选器可以使用 ID 或 UUID 号码来定位特定任务。要指定多个任务，请使用以下形式之一（用空格分隔的 ID 号码、UUID 号码或 ID 范围列表）：

      task 1 2 3                                    delete
      task 1-3                                      info
      task 1 2-5 19                                 modify pri:H
      task 4-7 ebeeab00-ccf8-464b-8b58-f7f2d606edfb info

注意，可能需要正确转义特殊字符和引号，以避免它们在 shell 中的特殊含义。有关更多信息，请参阅"指定描述"部分。

# 修改

&lt;mods&gt; 由零个或多个更改组成，应用于所选任务，例如：

      task <filter> <command> project:Home
      task <filter> <command> +weekend +garden due:tomorrow
      task <filter> <command> Description/annotation text
      task <filter> <command> /from/to/     <- 替换第一个匹配项
      task <filter> <command> /from/to/g    <- 替换所有匹配项

# 子命令

Taskwarrior 支持不同类型的命令。有读取命令、写入命令、杂项命令和脚本帮助命令。读取命令不允许修改任务。写入命令可以改变任务的几乎所有方面。提供脚本帮助命令来帮助您编写附加脚本，例如 shell 补全（仅生成最少的输出，如 verbose=nothing 一样）。受 *context* 显式影响的命令标记为这样。

# 读取子命令

报告是读取子命令。Taskwarrior 中预定义了几个报告。这些报告的输出和排序行为可以在配置文件中配置。另请参阅手册页 taskrc(5)。还有其他不是报告的读取子命令。

**task --version**  
这是 Taskwarrior 支持的唯一传统命令行参数，用于附加脚本验证已安装的 Taskwarrior 的版本号，而无需调用创建默认文件的机制。

**task &lt;filter&gt;**  
在未指定命令的情况下，运行默认命令，并应用筛选器。

**task &lt;filter&gt; active**  
显示所有与筛选器匹配的已启动但未完成的任务。

**task &lt;filter&gt; all**  
显示所有与筛选器匹配的任务，包括循环任务的父任务。

**task &lt;filter&gt; blocked**  
显示所有与筛选器匹配的、当前被其他任务阻止的任务。

**task &lt;filter&gt; blocking**  
显示所有与筛选器匹配的、阻止其他任务的任务。

**task &lt;filter&gt; burndown.daily**  
按天显示图形化的燃尽图。受 context 影响。

**task &lt;filter&gt; burndown.weekly**  
按周显示图形化的燃尽图。注意"burndown"是"burndown.weekly"报告的别名。受 context 影响。

**task &lt;filter&gt; burndown.monthly**  
按月显示图形化的燃尽图。受 context 影响。

**task calendar \[due|&lt;month&gt; &lt;year&gt;|&lt;year&gt;\] \[y\]**  
显示标记了到期任务的每月日历。显示一条水平的月份线。如果提供了"y"参数，将显示至少一个完整年份。如果提供了年份（如"2015"），则显示该完整年份。如果同时指定了月份和年份（"6 2015"），则显示的月份从指定的月份和年份开始。如果提供了"due"参数，将显示最早到期任务的起始月份。

**task colors \[&lt;sample&gt; | legend\]**  
显示所有可能的颜色、命名的样本或包含所有当前定义的颜色的图例。

**task columns \[&lt;substring&gt;\]**  
显示所有支持的列和格式化样式。创建自定义报告时很有用。如果提供了子字符串，仅显示匹配的列名。

**task commands**  
显示所有支持的命令，以及每个命令的一些详细信息。

**task &lt;filter&gt; completed**  
显示所有与筛选器匹配的已完成任务。

**task &lt;filter&gt; count**  
仅显示与筛选器匹配的任务数。受 context 影响。

**task &lt;filter&gt; export**  
以 JSON 格式导出所有任务。如果希望保存，可将输出重定向到文件，或将其管道传递给另一个命令或脚本以转换为另一种格式。您可以在 &lt;https://taskwarrior.org/tools/&gt; 在线找到这些示例脚本：

<!-- -->

      export-csv.pl
      export-sql.py
      export-xml.py
      export-yaml.pl
      export-html.pl
      export-tsv.pl
      export-xml.rb
      export-ical.pl
      export-xml.pl
      export-yad.pl

**task &lt;filter&gt; ghistory.annual**  
按年显示任务状态的图形化报告。

**task &lt;filter&gt; ghistory.monthly**  
按月显示任务状态的图形化报告。注意"ghistory"是"ghistory.monthly"的别名。

**task &lt;filter&gt; ghistory.weekly**  
按周显示任务状态的图形化报告。

**task &lt;filter&gt; ghistory.daily**  
按天显示任务状态的图形化报告。

**task help**  
显示长使用文本。

**task &lt;filter&gt; history.annual**  
按年显示任务历史报告。受 context 影响。

**task &lt;filter&gt; history.monthly**  
按月显示任务历史报告。注意"history"是"history.monthly"的别名。受 context 影响。

**task &lt;filter&gt; history.weekly**  
按周显示任务历史报告。受 context 影响。

**task &lt;filter&gt; history.daily**  
按天显示任务历史报告。受 context 影响。

**task &lt;filter&gt; ids**  
应用筛选器，然后仅提取任务 ID 并将其显示为空格分隔的列表。这对于作为任务命令的输入很有用，可以达到以下目的：

<!-- -->

      task $(task project:Home ids) modify priority:H

此示例首先获取 project:Home 筛选器的 ID，然后为每个任务设置优先级为 H。这也可以直接实现：

      task project:Home modify priority:H

此命令主要供外部脚本使用。

**task &lt;filter&gt; uuids**  
对所有任务（甚至已删除和已完成的任务）应用筛选器，然后仅提取任务 UUID 并将其显示为空格分隔的列表。这对于作为任务命令的输入很有用，可以达到以下目的：

<!-- -->

      task $(task project:Home status:completed uuids) modify status:pending

此示例首先获取 project:Home 和 status:completed 筛选器的 UUID，然后再次使每个任务处于待处理状态。

此命令主要供外部脚本使用。

**task udas**  
显示定义的 UDA 列表，包括其名称、类型、标签和允许的值。还显示 UDA 使用情况和任何孤立的 UDA。

**task &lt;filter&gt; information**  
显示指定任务的所有数据和元数据。这是显示任务所有方面的唯一方法，包括更改历史。

**task &lt;filter&gt; list**  
提供与筛选器匹配的任务的标准列表。

**task &lt;filter&gt; long**  
提供与筛选器匹配的任务的最详细列表。

**task &lt;filter&gt; ls**  
提供与筛选器匹配的任务的短列表。

**task &lt;filter&gt; minimal**  
提供与筛选器匹配的任务的最少列表。

**task &lt;filter&gt; newest**  
显示与筛选器匹配的最新任务。

**task &lt;filter&gt; next**  
显示最紧急的任务页面，按紧急性排序，这是一个计算值。

**task &lt;filter&gt; ready**  
显示最紧急的就绪任务页面，按紧急性排序，启动的任务优先。就绪任务是未安排的任务，或已安排日期已过且不在等待中的任务。

**task &lt;filter&gt; oldest**  
显示与筛选器匹配的最旧任务。

**task &lt;filter&gt; overdue**  
显示所有与筛选器匹配的、超过到期日期的未完成任务。

**task &lt;filter&gt; projects**  
列出当前由待处理任务使用的所有项目名称，以及每个项目的任务数量。受 context 影响。

**task &lt;filter&gt; recurring**  
显示与筛选器匹配的所有循环任务。

**task &lt;filter&gt; unblocked**  
显示所有未被其他任务阻止的任务，匹配筛选器。

**task &lt;filter&gt; waiting**  
显示所有与筛选器匹配的等待任务。

# 写入子命令

**task add &lt;mods&gt;**  
向任务列表添加新的待处理任务。它受当前设置的 context 影响。

**task &lt;filter&gt; annotate &lt;mods&gt;**  
向现有任务添加注释。

**task &lt;filter&gt; append &lt;mods&gt;**  
将描述文本追加到现有任务。

**task &lt;filter&gt; delete &lt;mods&gt;**  
从任务列表中删除指定任务。受 context 影响。

**task &lt;filter&gt; denotate &lt;mods&gt;**  
删除指定任务的注释。如果提供的描述与注释完全匹配，则删除相应的注释。如果提供的描述与注释部分匹配，则删除首个部分匹配的注释。受 context 影响。

**task &lt;filter&gt; done &lt;mods&gt;**  
将指定的任务标记为完成。受 context 影响。

**task &lt;filter&gt; duplicate &lt;mods&gt;**  
复制指定的任务并允许修改。受 context 影响。

**task &lt;filter&gt; edit**  
启动文本编辑器以直接修改任务的所有方面。通常，这不是修改任务的推荐方法，但在特殊情况下提供。请谨慎使用。受 context 影响。

**task import \[&lt;file&gt; ...\]**  
导入 JSON 格式的任务。可用于添加新任务或更新现有任务。任务由其 UUID 标识。

如果未指定文件或"-"，则从 STDIN 导入任务。

如果 import 要在自动化工作流中使用，建议设置 rc.recurrence.confirmation 为适当的级别。请参阅 taskrc(5)。

对于导入其他文件格式，标准 task 版本包含一些示例脚本，例如：

      import-todo.sh.pl
      import-yaml.pl

**task import-v2**  
从 Taskwarrior v2.x 格式导入任务。这用于从版本 2.x 升级到版本 3.x。

**task log &lt;mods&gt;**  
添加一个已经完成的新任务到任务列表。它受当前设置的 context 影响。

**task &lt;filter&gt; modify &lt;mods&gt;**  
使用提供的信息修改现有任务。

**task &lt;filter&gt; prepend &lt;mods&gt;**  
将描述文本前置到现有任务。受 context 影响。

**task &lt;filter&gt; purge**  
从数据文件中永久删除指定的任务。只有已删除的任务才能被清除。此命令仅具有本地效果，由它引入的更改不会同步。受 context 影响。

警告：导致永久的、不可逆的数据丧失。

**task &lt;filter&gt; start &lt;mods&gt;**  
将指定的任务标记为已启动。受 context 影响。

**task &lt;filter&gt; stop &lt;mods&gt;**  
从指定的任务中删除*start* 时间。受 context 影响。

# 杂项子命令

杂项子命令要么不接受命令行参数，要么接受非标准参数。

**task calc &lt;expression&gt;**  
计算代数表达式。可用于测试 Taskwarrior 如何解析和计算命令行上给定的表达式。

示例：

        task calc 1 + 1
        2

        task calc now + 8d
        2015-03-26T18:06:57

        task calc eom
        2015-03-31T23:59:59

**task config \[&lt;name&gt; \[&lt;value&gt; | ''\]\]**  
直接在 Taskwarrior 配置中添加、修改和删除设置。此命令要么使用新的"value"值修改"name"设置，要么添加等同于"name=value"的新条目：

<!-- -->

        task config name value

此命令设置空值。这具有压制任何默认值的效果：

        task config name ''

最后，此命令从 .taskrc 文件中删除任何"name=..."条目：

        task config name

**task context &lt;name&gt;**  
设置当前活动的 context。请参阅 CONTEXT 部分。

示例：

        task context work

**task context delete &lt;name&gt;**  
删除名称为 &lt;name&gt; 的 context。如果正在删除的 context 被设置为活动的，它将被取消设置。

示例：

        task context delete work

**task context define &lt;name&gt; &lt;filter&gt;**  
定义名称为 &lt;name&gt、定义为 &lt;filter&gt; 的新 context。此命令不影响当前设置的 context，仅添加新的 context 定义。

示例：

        task context define work project:Work
        task context define home project:Home or +home
        task context define superurgent due:today and +urgent

**task context list**  
输出可用 context 的列表及其定义。

**task context none**  
清除当前活动的 context（如果已设置）。

**task context show**  
显示当前活动的 context 及其定义。

**task diagnostics**  
显示诊断信息，这是报告问题所需的信息类型。报告错误时，平台、版本和环境可能很重要。运行此命令会生成应附加到错误报告中的类似信息的摘要。

它包括编译器、库和软件信息。它不包括任何个人信息，除了您的任务数据位置。

此命令还对您的数据执行诊断扫描，查找常见问题，例如重复的 UUID。

**task execute &lt;external command&gt;**  
执行指定的命令。本身不太有用，但结合别名和扩展使用时可提供无缝集成。

**task logo**  
显示 Taskwarrior 徽标。

**task news**  
在安装新版本的 Taskwarrior 时指导用户浏览重要的发布说明。它提供个性化反馈、弃用警告和适用的使用建议。

**task reports**  
列出所有支持的报告。这包括内置报告和您定义的任何自定义报告。

**task show \[all | &lt;substring&gt;\]**  
显示所有当前设置。如果指定了子字符串，仅显示包含该子字符串的设置。

**task &lt;filter&gt; stats**  
显示由筛选器定义的任务的统计信息。受 context 影响。

**task &lt;filter&gt; summary**  
按项目显示聚合任务状态的报告。受 context 影响。

**task sync**  
sync 命令使用已配置的 Taskserver 同步数据。

注意：如果使用多个同步客户端，请确保在主客户端上启用此设置（这是默认值）：

      recurrence=on

在所有其他客户端上（这不是默认值）：

      recurrence=off

这是避免复制循环任务的循环错误的解决方法。

**task &lt;filter&gt; tags**  
显示所使用的所有标签的列表。任何使用的特殊标签都会突出显示。请注意，虚拟标签未列出 - 它们并不真正存在，只是其他任务元数据的方便符号。尝试添加或删除虚拟标签是一个错误。受 context 影响。

**task timesheet \[&lt;weeks&gt;\]**  
显示完成和启动的任务的每周报告。

**task undo**  
撤销最近的操作。遵守确认设置。

**task version**  
显示 Taskwarrior 版本号。

# 辅助子命令

**task \_aliases**  
生成所有别名的列表，用于自动补全。

**task \_columns**  
仅显示支持的列的列表。

**task \_commands**  
生成所有命令的列表，用于自动补全。

**task \_config**  
列出所有支持的配置变量，用于补全。

**task \_context**  
列出所有可用的 context 变量。

**task &lt;filter&gt; \_ids**  
仅显示匹配任务的 ID，以列表形式。在 _unique 中已弃用。

**task \_show**  
显示配置设置的组合默认值和覆盖值，供第三方应用程序使用。

**task &lt;filter&gt; \_unique &lt;attribute&gt;**  
报告属性值的唯一集合。例如，要查看所有活动项目：

<!-- -->

      task +PENDING _unique project

**task &lt;filter&gt; \_uuids**  
仅显示所有任务（甚至已删除和已完成的任务）中匹配任务的 UUID，以列表形式。在 _unique 中已弃用。

**task \_udas**  
仅以列表形式显示定义的 UDA 名称。

**task &lt;filter&gt; \_projects**  
仅显示所有已使用项目名称的列表。在 _unique 中已弃用。

**task &lt;filter&gt; \_tags**  
仅显示所有已使用标签的列表，用于自动补全。在 _unique 中已弃用。

**task &lt;filter&gt; \_urgency**  
显示任务的紧急程度。

**task \_version**  
仅显示 Taskwarrior 版本号。

**task \_zshcommands**  
生成所有命令的列表，用于 zsh 自动补全。

**task &lt;filter&gt; \_zshids**  
显示匹配任务的 ID 和描述。

**task &lt;filter&gt; \_zshuuids**  
显示匹配任务的 UUID 和描述。

**task \_get &lt;DOM&gt; \[&lt;DOM&gt; ...\]**  
访问和显示 DOM 引用。用于从任务或系统中提取单个值。支持的 DOM 引用是：

<!-- -->

      rc.<name>
      tw.syncneeded
      tw.program
      tw.args
      tw.width
      tw.height
      tw.version
      context.program    （2.6.0 中弃用）
      context.args       （2.6.0 中弃用）
      context.width      （2.6.0 中弃用）
      context.height     （2.6.0 中弃用）
      system.version
      system.os
      <id>.<attribute>
      <uuid>.<attribute>

请注意，"rc.&lt;name&gt;"引用可能需要使用"--"转义以防止该引用被解释为覆盖。

请注意，如果 DOM 引用无效或引用计算为缺失值，命令将以 1 退出。

另外，特定类型属性的某些组件可以由 DOM 引用提取。

      $ task _get 2.due.year
      2015

有关支持的属性特定 DOM 引用的完整列表，请查阅在线文档：
&lt;https://taskwarrior.org/docs/dom.html&gt;

# 属性和元数据

**ID**  
任务可以通过 ID 唯一指定，这是"工作集"任务的索引（主要是待处理和循环任务）。任务的 ID 可能会改变，但仅当运行显示 ID 的报告时才会改变。修改任务时，依靠最后显示的 ID 是安全的。始终运行报告以检查您具有任务的正确 ID。ID 可以作为序列给任务，例如：

<!-- -->

    task 1,4-10,19 delete

**+tag|-tag**  
标签是与任务关联的任意词。使用 + 向任务添加标签，使用 - 从任务中删除标签。任务可以有任何数量的标签。

某些标签（称为"特殊标签"）可用于影响任务的处理方式。例如，如果任务具有特殊标签"nocolor"，则它不受所有颜色规则的影响。支持的特殊标签是：

        +nocolor     禁用此任务的颜色规则处理
        +nonag       此任务的完成禁止所有 nag 消息
        +nocal       此任务不会显示在日历上
        +next        提升任务使其显示在"next"报告中

还有虚拟标签，它们以标签形式代表任务元数据。这些标签不存在，但可用于筛选任务。支持的虚拟标签是：

        ACTIVE       匹配任务是否已启动
        ANNOTATED    匹配任务是否有注释
        BLOCKED      匹配任务是否当前被阻止
        BLOCKING     匹配任务是否阻止其他任务
        CHILD        匹配任务是否有父任务（在 2.6.0 中弃用）
        COMPLETED    匹配任务是否具有已完成状态
        DELETED      匹配任务是否具有已删除状态
        DUE          匹配任务是否在未来 7 天内到期（请参阅 rc.due）
        INSTANCE     匹配任务是否是循环实例
        LATEST       匹配任务是否是最新添加的任务
        MONTH        匹配任务是否在本月到期
        ORPHAN       匹配任务是否有任何孤立的 UDA 值
        OVERDUE      匹配任务是否逾期
        PARENT       匹配任务是否是父任务（在 2.6.0 中弃用）
        PENDING      匹配任务是否具有待处理状态
        PRIORITY     匹配任务是否有优先级
        PROJECT      匹配任务是否有项目
        QUARTER      匹配任务是否在本季度到期
        READY        匹配任务是否可操作
        SCHEDULED    匹配任务是否已安排
        TAGGED       匹配任务是否有标签
        TEMPLATE     匹配任务是否是循环模板
        TODAY        匹配任务是否在今天到期
        TOMORROW     匹配任务是否在某个明天到期
        UDA          匹配任务是否有任何 UDA 值
        UNBLOCKED    匹配任务是否未被阻止
        UNTIL        匹配任务是否过期
        WAITING      匹配任务是否在等待
        WEEK         匹配任务是否在本周到期
        YEAR         匹配任务是否在本年度到期
        YESTERDAY    匹配任务是否在某个昨天到期

您可以使用 +BLOCKED 来筛选被阻止的任务，或 -BLOCKED 来筛选未被阻止的任务。类似地，-BLOCKED 等同于 +UNBLOCKED。尝试添加或删除虚拟标签是一个错误。

**project:&lt;project-name&gt;**  
指定任务相关的项目。

**status:pending|deleted|completed|waiting|recurring**  
指定任务的状态。

**priority:H|M|L or priority:**  
为任务指定高、中、低和无优先级。

**due:&lt;due-date&gt;**  
指定任务的到期日期。

**recur:&lt;frequency&gt;**  
指定任务循环的频率。

**scheduled:&lt;ready-date&gt;**  
指定任务可以完成的日期之后。

**until:&lt;expiration date of task&gt;**  
指定任务的到期日期，之后任务将被删除。

**limit:&lt;number-of-rows&gt;**  
指定报告应显示的所需任务数。如果给定正整数，则使用此参数。值"page"也可以使用，并将限制报告输出为尽可能多的文本行。这默认为 25 行。

**wait:&lt;wait-date&gt;**  
当为任务指定等待日期时，它从大多数内置报告中隐藏，这些报告排除 +WAITING。当日期在过去时，任务不被视为 +WAITING，并再次变得可见。注意，为了兼容性，这样的任务显示为具有状态"waiting"，但这将在将来的版本中改变。

**depends:&lt;id1,id2 ...&gt;**  
声明此任务取决于 id1 和 id2。这意味着任务 id1 和 id2 应该在此任务之前完成。因此，此任务将显示在"blocked"报告上。它接受以逗号分隔的 ID 号、UUID 号和 ID 范围列表。当通过"-"为此列表的任何元素添加前缀时，指定的任务将从依赖列表中删除。

**entry:&lt;entry-date&gt;**  
出于报告目的，指定任务的创建日期。

**modified:&lt;modified-date&gt;**  
指定最最近的修改日期。

# 属性修饰符

属性修饰符改进筛选器。支持的修饰符是：

> **before（同义词 under, below）**  
> **after（同义词 over, above）**  
> **by**  
> **none**  
> **any**  
> **is（同义词 equals）**  
> **isnt（同义词 not）**  
> **has（同义词 contains）**  
> **hasnt**  
> **startswith（同义词 left）**  
> **endswith（同义词 right）**  
> **word**  
> **noword**

它们可以应用于所有常规属性（见上文）和以下计算属性：

> **urgency**（或简称 **urg**）

例如：

        task due.before:eom priority.not:L list

*before* 修饰符用于比较值，保留语义，因此 project.before:B 列出所有以"A"开头的项目。优先级"L"在"M"之前，due:2011-01-01 在 due:2011-01-02 之前。同义词"under"和"below"包括在内以允许更自然地读取筛选器。

*after* 修饰符是 *before* 修饰符的倒数。

*by* 修饰符与"before"相同，但它也包括所讨论的时刻。例如：

        task add test due:eoy

在使用包括的筛选器"by"时会被找到：

        task due.by:eoy

但使用非包括筛选器"before"时则不会：

        task due.before:eoy

这同样适用于其他命名日期，例如"eom"、"eod"等；修饰符使用"<="而不是"<"（如"before"一样）进行比较。

*none* 修饰符要求属性没有值。例如：

        task priority:      list
        task priority.none: list

是等价的，并列出没有优先级的任务。

*any* 修饰符要求属性有一个值，但任何值都可以。

*is* 修饰符需要与值完全匹配。

*isnt* 修饰符是 *is* 修饰符的倒数。

*has* 修饰符用于搜索子字符串，例如：

        task description.has:foo list
        task foo                 list

这些是等价的，并将返回任何在描述或注释中有"foo"的任务。

*hasnt* 修饰符是 *has* 修饰符的倒数。

*startswith* 修饰符匹配属性的左侧或开始，例如：

        task project.startswith:H list
        task project:H            list

是等价的，并将匹配任何以"H"开头的项目。匹配所有不以"H"开头的项目通过以下方式完成：

        task project.not:H         list

*endswith* 修饰符匹配属性的右侧或末尾。

*word* 修饰符要求属性包含指定的整个词，例如此：

        task description.word:foo list

将匹配描述"foo bar baz"，但不匹配"dog food"。

*noword* 修饰符是 *word* 修饰符的倒数。

# 表达式和运算符

您可以在筛选器表达式中使用以下运算符：

      and  or  xor  !               逻辑运算符
      <  <=  =  ==  !=  !==  >=  >  关系运算符
      (  )                          优先级

例如：

      task due.before:eom priority.not:L list
      task '( due < eom or priority != L )' list
      task '! ( project:Home or project:Garden )' list

*=* 运算符测试近似相等性。如果日期在同一天，则日期比较相等（忽略小时和分钟）。如果左操作数以右操作数开头，则字符串比较相等。*==* 运算符测试精确相等性。*!=* 和 *!==* 运算符是 *=* 和 *==* 的否定。否定运算符是 *!*。

请注意，使用除"and"运算符之外的逻辑运算符时需要括号。原因是某些报告包含必须与命令行组合的筛选器。考虑此示例：

      task project:Home or project:Garden list

虽然这看起来正确，但实际上并不。"list"报告包含筛选器：

      task show report.list.filter

      配置变量           值
      -----------------  --------------
      report.list.filter status:pending

这意味着该示例实际上是：

      task status:pending project:Home or project:Garden list

隐含的"and"运算符使其成为：

      task status:pending and project:Home or project:Garden list

这是一个优先级错误 - "and"和"or"需要使用括号分组，如下所示：

      task status:pending and ( project:Home or project:Garden ) list

因此原始示例必须输入为：

      task '( project:Home or project:Garden )' list

这包括引号以转义括号，以便 shell 不会解释它们并将其从 Taskwarrior 隐藏。

运算符、属性修饰符和其他语法糖之间存在冗余。例如，以下都是等价的：

      task foo                      list
      task /foo/                    list
      task description.contains:foo list
      task description.has:foo      list
      task 'description ~ foo'      list

# 指定日期和频率

## 日期

Taskwarrior 从命令行读取日期，并在报告中显示日期。预期和所需的日期格式由配置变量 *dateformat* 确定

> 精确规范  
>
> <!-- -->
>
>       task ... due:7/14/2008
>
> ISO-8601  
>
> <!-- -->
>
>       task ... due:2013-03-14T22:30:00Z
>
> 相对措辞  
>
> <!-- -->
>
>       task ... due:now
>       task ... due:today
>       task ... due:yesterday
>       task ... due:tomorrow
>
> 带序数的日期号  
>
> <!-- -->
>
>       task ... due:23rd
>       task ... due:3wks
>       task ... due:1day
>       task ... due:9hrs
>
> 下一个（工作）周（星期一）、日历周（星期日或星期一）、月、季度和年的开始  
>
> <!-- -->
>
>       task ... due:sow
>       task ... due:soww
>       task ... due:socw
>       task ... due:som
>       task ... due:soq
>       task ... due:soy
>
> 当前（工作）周（星期五）、日历周（星期六或星期日）、月、季度和年的末尾  
>
> <!-- -->
>
>       task ... due:eow
>       task ... due:eoww
>       task ... due:eocw
>       task ... due:eom
>       task ... due:eoq
>       task ... due:eoy
>
> 在某个时刻或之后  
>
> <!-- -->
>
>       task ... wait:later
>       task ... wait:someday
>
> 这将等待日期设置为 12/30/9999。
>
> 下一个出现的工作日  
>
> <!-- -->
>
>       task ... due:fri
>
> 可预测的假日  
>
> <!-- -->
>
>       task ... due:goodfriday
>       task ... due:easter
>       task ... due:eastermonday
>       task ... due:ascension
>       task ... due:pentecost
>       task ... due:midsommar
>       task ... due:midsommarafton
>       task ... due:juhannus

## 频率

循环周期。Taskwarrior 支持多种指定循环任务*frequency*的方式。请注意，频率可以缩写。

> daily, day, 1day, 1days, 2day, 2days, 1da, 2da, ...  
> 每天或多天。
>
> weekdays  
> 星期一、星期二、星期三、星期四、星期五，并跳过周末。
>
> weekly, 1wk, 2wks, ...  
> 每周或多周。
>
> biweekly, fortnight  
> 每两周。
>
> monthly, month, 1mo, 2mo, ...  
> 每个月。
>
> quarterly, 1qtr, 2qtrs, ...  
> 每三个月、一个季度或多个季度。
>
> semiannual  
> 每六个月。
>
> annual, yearly, 1yr, 2yrs, ...  
> 每年或多年。
>
> biannual, biyearly, 2yr  
> 每两年。

# 上下文

Context 是一个用户定义的查询，自动应用于所有筛选任务列表的命令和创建新任务的命令（add、log）。例如，任何报告命令的结果都将受当前活动 context 的影响。以下是受影响的命令列表：

        add
        burndown
        count
        delete
        denotate
        done
        duplicate
        edit
        history
        log
        prepend
        projects
        purge
        start
        stats
        stop
        summary
        tags

所有其他命令不受 context 影响。

        $ task list
        ID Age Project  Description        Urg
        1  2d  Sport    Run 5 miles        1.42
        2  1d  Home     Clean the dishes   1.14

        $ task context home
        Context 'home' set. Use 'task context none' to remove.

        $ task list
        ID Age Project  Description        Urg
        2  1d  Home     Clean the dishes   1.14
        Context 'home' set. Use 'task context none' to remove.

任务列表自动筛选为 project:Home。

        $ task add Vaccuum the carpet
        Created task 3.
        Context 'home' set. Use 'task context none' to remove.

        $ task list
        ID Age Project  Description         Urg
        2  1d  Home     Clean the dishes    1.14
        3  5s  Home     Vaccuum the carpet  1.14
        Context 'home' set. Use 'task context none' to remove.

请注意，新添加的任务"Vaccuum the carpet"自动设置了"project:Home"。

如上例所示，context 通过将其名称指定给"context"命令来应用。要更改当前应用的 context，只需将新 context 的名称传递给"context"命令。

要取消设置任何 context，请使用"none"子命令。

        $ task context none
        Context unset.

        $ task list
        ID Age Project  Description         Urg
        1  2d  Sport    Run 5 miles         1.42
        2  1d  Home     Clean the dishes    1.14
        3  7s  Home     Vaccuum the carpet  1.14

Context 可以使用"define"子命令定义，同时指定新 context 的名称和分配的筛选器。

        $ task context define home project:Home
        Are you sure you want to add 'context.home.read' with a value of 'project:Home'? (yes/no) yes
        Are you sure you want to add 'context.home.write' with a value of 'project:Home'? (yes/no) yes
        Context 'home' successfully defined.

请注意，您被单独提示设置"read"和"write"context。这允许您指定仅适用于报告命令或仅适用于创建任务的命令的 context。

要删除定义，请使用"delete"子命令。

        $ task context delete home
        Are you sure you want to remove 'context.home.read'? (yes/no) yes
        Are you sure you want to remove 'context.home.write'? (yes/no) yes
        Context 'home' deleted.

要检查当前活动的 context 是什么，请使用"show"子命令。

        $ task context show
        Context 'home' with

        * read filter: '+home'
        * write filter: '+home'

        is currently applied.

Context 可以存储任意复杂的筛选器。

        $ task context define family project:Family or +paul or +nancy
        Are you sure you want to add 'context.family.read' with a value of 'project:Family or +paul or +nancy'? (yes/no) yes
        Are you sure you want to add 'context.family.write' with a value of 'project:Family or +paul or +nancy'? (yes/no) no
        Context 'family' successfully defined.

Context 是永久的，当前设置的 context 名称存储在"context"配置变量中。context 定义存储在"context.&lt;name&gt;.read"配置变量（用于报告命令）和"context.&lt;name&gt;.write"配置变量（用于任务添加，即 task add/log）中。

请注意，在上面的示例中，用户决定不将复杂筛选器定义为可写 context。此决定的原因是示例中的复杂筛选器不直接转换为修改。实际上，如果将这样的 context 用作可写 context，则会发生以下情况：

        $ task add Call Paul
        Created task 4.
        Context 'family' set. Use 'task context none' to remove.

        $ task 4 list
        ID Age  Project Tags       Description      Urg
         4 9min Family  nancy paul or or Call Paul    0

复杂筛选器使用和修改之间没有明确的映射（应仅设置项目吗？仅标签？两者都设置？）。另外请注意描述中存在"or"运算符。Taskwarrior 在这里不试图猜测用户意图，而是期望用户设置"context.&lt;name&gt;.write"变量以明确说明其意图，例如：

        $ task config context.family.write project:Family
        Are you sure you want to change the value of 'context.family.write' from 'project:Family or +paul or +nancy' to 'project:Family'? (yes/no) yes
        Config file /home/tbabej/.config/task/taskrc modified.

        $ task context
        Name   Type  Definition                        Active
        family read  project:Family or +paul or +nancy yes
               write project:Family                    yes
        home   read  +home                             no
               write +home                             no

请注意 context"family"的 read 和 write context 如何不同，而对于 context"home"，它们保持相同。

另外，每个配置参数可以通过指定 context.&lt;name&gt;.rc.&lt;parameter&gt; 覆盖当前 context。例如，如果 family context 的默认命令应该显示 family_report：

        $ task config context.family.rc.default.command family_report

# 命令缩写

所有 Taskwarrior 命令都可以缩写，只要使用唯一前缀，例如：

      $ task li

是

      $ task list

的明确缩写

但

      $ task l

可能是 list、ls 或 long。

请注意，您可以使用配置设置限制最少缩写大小：

      abbreviation.minimum=3

# 指定描述

某些任务描述需要转义，因为 shell 和某些字符对 shell 的特殊含义。这可以通过向描述添加引号或转义特殊字符来完成：

      $ task add "quoted ' quote"
      $ task add escaped \' quote

参数 --（双破折号）告诉 Taskwarrior 将所有其他参数视为描述：

      $ task add -- project:Home needs scheduling

在其他情况下，shell 看到空格并分解参数。例如，此命令：

      $ task 123 modify /from this/to that/

被分解为多个参数，这通过引号更正：

      $ task 123 modify "/from this/to that/"

有时需要强制 shell 将引号完整地传递给 Taskwarrior，因此您可以使用：

      $ task add project:\'Three Word Project\' description

Taskwarrior 仅使用 UTF8 编码支持 Unicode。

# 配置文件和覆盖选项

Taskwarrior 将其配置存储在用户主目录中的文件中：~/.taskrc。默认配置文件可以被覆盖：

**task rc:&lt;path-to-alternate-file&gt; ...**  
指定优先级最高的备用配置文件。

**TASKRC=&lt;path-to-alternate-file&gt; task ..**  
环境变量指定要使用的备用配置文件。

**XDG\_CONFIG\_HOME=&lt;path-to-alternate-config-home&gt; task ..**  
环境变量指定要使用的备用配置文件。

**task rc.&lt;name&gt;:&lt;value&gt; ...**  
**task rc.&lt;name&gt;=&lt;value&gt; ...** 指定单个配置文件覆盖。

**TASKDATA=/tmp/.task task ...**  
环境变量覆盖默认值和任务数据目录的"data.location"配置设置。

# 更多示例

有关示例，请参阅从以下开始的在线文档

> &lt;https://taskwarrior.org/docs&gt;

请注意，在线文档可能比此手册页更详细且更当前。

# 文件

~/.taskrc  
用户配置文件 - 另请参阅 taskrc(5)。请注意，这可以在命令行或 TASKRC 环境变量上覆盖。另外，如果 *~/.taskrc* 不存在，而 XDG_CONFIG_HOME 环境变量已定义，taskwarrior 将检查 $XDG_CONFIG_HOME/task/taskrc 是否存在并尝试读取它

~/.task  
任务存储其数据的默认目录。可以在配置变量"data.location"中配置位置，或使用 TASKDATA 环境变量覆盖。

~/.task/taskchampion.sqlite3  
数据库文件。

# 版权和信用

Copyright (C) 2006 - 2021 T. Babej, P. Beckingham, F. Hernandez.

Taskwarrior 在 MIT 许可证下分发。有关更多信息，请参见
https://www.opensource.org/licenses/mit-license.php。

# 参见

**taskrc(5),** **task-color(5),** **task-sync(5)**

有关 Taskwarrior 的更多信息，请参见以下内容：

官方网站  
&lt;https://taskwarrior.org&gt;

官方代码库  
&lt;https://github.com/GothenburgBitFactory/taskwarrior&gt;

您可以通过以下方式联系项目  
&lt;support@GothenburgBitFactory.org&gt;

# 报告错误

Taskwarrior 中的错误可能会报告给问题跟踪器  
&lt;https://github.com/GothenburgBitFactory/taskwarrior/issues&gt;
