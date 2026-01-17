---
title: 'Taskwarrior - taskrc.5 for task-3.4.2 (中文)'
viewport: 'width=device-width, initial-scale=1'
layout: single
---
# 名称

taskrc - task(1) 命令的配置详情。

# 概述

**$HOME/.taskrc**  
**task rc:&lt;directory-path&gt;/.taskrc ...**  
**TASKRC=&lt;directory-path&gt;/.taskrc task ...**  
**XDG\_CONFIG\_HOME=&lt;directory-path&gt;/task/taskrc task ...****

# 描述

**Taskwarrior** 从称为 *.taskrc* 的文件获取其配置数据。此文件通常位于用户的主目录中：

        $HOME/.taskrc

可以使用运行 task 时的 *rc:* 属性覆盖默认位置：

        $ task rc:<directory-path>/.taskrc ...

或使用 TASKRC 环境变量：

        $ TASKRC=/tmp/.taskrc task ...

另外，如果不存在 ~/.taskrc，taskwarrior 将检查 XDG_CONFIG_HOME 环境变量是否已定义：

        $ XDG_CONFIG_HOME=~/.config task ...

可以使用运行 task 时的 *rc.&lt;name&gt;:* 属性覆盖单个选项：

        $ task rc.<name>:<value> ...

或

        $ task rc.<name>=<value> ...

如果 **Taskwarrior** 运行时没有现有的配置文件，它将询问是否应该在用户的主目录中创建默认的示例 *.taskrc* 文件。

.taskrc 文件遵循非常简单的语法，定义名称/值对：

        <name> = <value>

&lt;name&gt;、"="和&lt;value&gt; 周围可能有空格，会被忽略。&lt;value&gt; 中的空格会被保留。逗号分隔的列表中不允许使用空格。条目必须在一行上，没有连续。值支持 UTF8 和 JSON 编码，如 \uNNNN。

请注意，Taskwarrior 对用于表示布尔项的值很灵活。您可以使用"1"来启用，任何其他值都被解释为禁用。值"on"、"yes"、"y"和"true"也支持。

        include <file>

"include"和&lt;file&gt; 周围可能有空格。文件可以是绝对路径或相对路径，特殊字符"~"被展开为 $HOME。如果指定了相对路径，将相对于以下目录进行评估（按优先级顺序列出）：1. 当前工作目录 2. 包含 taskrc 文件的目录 3. 包管理器设置的目录（通常包含预定义的主题）

请注意，环境变量也会在路径中展开（以及任何其他 taskrc 变量）。

        # <comment>

注释由字符"#"组成，并从"#"扩展到行尾。没有办法对多行块进行注释。可能有空行。

几乎每个值都有默认设置，空的 .taskrc 文件是使用所有默认值的文件。因此，.taskrc 文件的内容代表对默认值的覆盖。要完全删除默认值，必须有如下条目：

        <name> =

此条目使用空值覆盖默认值。

# 编辑

您可以手动编辑 .taskrc 文件，也可以使用"config"命令。要在 .taskrc 文件中永久设置值，请使用此命令：

        $ task config nag "You have more urgent tasks."

要删除条目，请使用此命令：

        $ task config nag

Taskwarrior 将使用默认值。要显式将值设置为空并因此避免使用默认值，请使用此命令：

        $ task config nag ""

Taskwarrior 还将使用此命令显示所有设置：

        $ task show

并且，还将对文件中的所有值执行检查，警告您发现的任何问题。

# 嵌套配置文件

.taskrc 可以使用 **include** 语句包含包含配置设置的其他文件：

        include <path/to/the/configuration/file/to/be/included>

通过使用 include 文件，您可以将主配置文件分解为包含相关配置数据的多个文件，如颜色等。

在 .taskrc 中有两个很好的 include 用途，如下所示：

        include holidays.en-US.rc
        include dark-16.theme

这包括与 Taskwarrior 一起分发的两个标准文件，这些文件定义了一组美国假日，并设置了 16 色主题以用于对报告和日历进行着色。

# 环境变量

这些环境变量会覆盖默认值，但不会覆盖命令行参数。

**TASKDATA=~/.task**  
这覆盖 Taskwarrior 数据的默认路径。

**TASKRC=~/.taskrc**  
这覆盖默认 RC 文件。

如果 *~/.taskrc* 不存在，将检查此环境变量

**XDG\_CONFIG\_HOME=~/.config**  
如果设置，taskwarrior 将查找 *$XDG\_CONFIG\_HOME/task/taskrc* 文件

# 配置变量

有效的变量名称及其默认值为：

## 文件

**data.location=$HOME/.task**  
这是包含所有 Taskwarrior 数据的目录的路径。默认情况下，它设置为 ~/.task，例如：/home/paul/.task

请注意，您可以使用 **~** shell 元字符，它将被正确展开。

请注意，TASKDATA 环境变量会覆盖此设置。

**hooks.location=$HOME/.task/hooks**  
这是 hook 脚本目录的路径。默认情况下是 ~/.task/hooks。

**gc=1**  
可用于暂时暂停重建，以便任务 ID 不会改变。重建需要对数据库的读写访问，因此禁用 `gc` 可能会导致更好的性能。

请注意，这应该以命令行覆盖的形式使用（task rc.gc=0 ...），而不是在 .taskrc 文件中永久使用，因为这会显著影响长期性能。

**purge.on-sync=0**  
如果设置，旧任务将在每次同步后自动清除。任务被识别为"旧"当它们的状态为"已删除"且 180 天内未被修改时。

**hooks=1**  
此主控制开关启用 hook 脚本处理。默认值为"1"，但某些扩展和环境可能需要禁用 hook。

**exit.on.missing.db=0**  
当设置为"1"时，如果数据库（~/.task 或 rc.data.location 或 TASKDATA 覆盖）丢失，会导致程序退出。默认值为"0"。

## 终端

**detection=1**  
确定是否使用 ioctl 来建立您使用的窗口大小，用于文本换行。

**limit:25**  
指定报告应显示的所需任务数量，如果给定正整数。值 'page' 也可以使用，它将限制报告输出为屏幕上能容纳的行数。默认值为 '25'。

**defaultwidth=80**  
当自动检测支持不可用时使用的输出宽度。默认为 80。如果设置为 0，则被解释为无限宽度，因此没有自动换行；这在将报告输出重定向到文件以进行后续处理时很有用。

**defaultheight=24**  
当自动检测支持不可用时使用的输出高度。默认为 24。如果设置为 0，则被解释为无限高度。这在将图表重定向到文件以进行后续处理时很有用。

**avoidlastcolumn=0**  
导致终端宽度减 1 被用作完整宽度。这避免了在最后一列中放置颜色代码，这可能会导致 Cygwin 用户的问题。默认值为 '0'。

**hyphenate=1**  
当换行在单词中间发生时进行连字符处理。默认值为 '1'。

**editor=vi**  
指定当使用 **task edit &lt;ID&gt;** 命令时要使用的文本编辑器。Taskwarrior 会首先查找此配置变量。如果找到，它被使用。否则它会查找 $VISUAL 或 $EDITOR 环境变量，最后默认使用 "vi"。

**reserved.lines=1**  
这是在屏幕底部为 shell 提示符预留的行数。这仅在使用 'limit:page' 时被引用。

## 杂项

**verbose=1|0|nothing|list...**  
当设置为 "1"（默认），有用的解释性注释会被添加到 Taskwarrior 的所有输出中。设置为 "0" 意味着你会看到常规输出。

特殊值 "nothing" 可用于消除所有可选输出，这导致仅显示格式化的数据，没有其他内容。这个输出最容易被 shell 脚本解析和使用。

或者，你可以指定一个逗号分隔的详细级别令牌列表，用于控制生成输出的特定场合。此列表可能包含：

        blank      在输出中插入额外的空行，以便清晰
        header     显示在报告输出之前的消息（这包括 .taskrc/.task 覆盖和 "[task next]" 消息）
        footnote   显示在报告输出之后的消息（主要是状态消息和更改描述）
        label      制表报告上的列标签
        new-id     对任何新任务的 ID 提供反馈（对于 ID 为 0 的新任务（例如新完成的任务），提供 UUID）
        new-uuid   对任何新任务的 UUID 提供反馈。覆盖 new-id。对自动化很有用。
        news       提醒读取新版本亮点直到用户运行 "task news"
        affected   报告 'N 个任务受影响' 等
        edit       用于 'edit' 命令的详细模板
        special    应用特殊标签时的反馈
        project    关于项目状态更改的反馈
        sync       关于同步的反馈
        filter     显示命令中使用的过滤器
        context    显示当前上下文。显示在脚注中。
        override   配置选项被覆盖时的通知
        recur      创建新的重复任务实例时的通知
        default    关于 taskwarrior 选择执行默认操作的通知

"affected"、"new-id"、"new-uuid"、"project"、"override" 和 "recur" 令牌表示 "footnote"。

"default" 令牌表示 "header"。

请注意，"1" 设置等同于指定所有令牌，而 "nothing" 设置等同于不指定任何令牌。

这里是快捷等效项：

        verbose=on
        verbose=blank,header,footnote,label,new-id,news,affected,edit,special,project,sync,filter,override,recur

        verbose=0
        verbose=blank,label,new-id,edit

        verbose=nothing
        verbose=

这些额外的注释被发送到标准错误以供 header、footnote 和 project。其他的被发送到标准输出。

**confirmation=1**  
可以是 "1" 或 "0"，确定 Taskwarrior 是否会在删除任务或执行撤销命令之前要求确认。默认值为 "1"。出于安全考虑，考虑启用此功能。

**allow.empty.filter=1**  
空过滤器与写入命令结合可能是误修改所有任务的一种方式，当检测到这种情况时，需要确认。设置为 '0' 意味着使用没有过滤器的写入命令是错误的。

**indent.annotation=2**  
控制在描述字段下方显示的注释的缩进空格数。默认值为 "2"。

**indent.report=0**  
控制整个报告输出的缩进。默认为 "0"。

**row.padding=0**  
控制报告输出每行周围的左右填充。默认值为 "0"。

**column.padding=0**  
控制报告输出列之间的填充。默认值为 "1"。

**bulk=3**  
是一个数字，默认为 3。当此数字或更多的任务在单个命令中被修改时，无论 **confirmation** 变量的值如何，都需要确认。特殊值 bulk=0 被视为无穷大。

这对于防止大规模无意的更改很有用。

**nag=You have more urgent tasks.**  
这可能是一个文本字符串，或为空。当任务被启动或完成时，当有其他任务具有更高的紧急性时，它被用作提示。默认值为："You have more urgent tasks"（你有更紧急的任务）。这是一个温和的提醒，你正在与你自己的紧急性设置相矛盾。

**list.all.projects=0**  
可以是 "1" 或 "0"，确定 'projects' 命令是列出你使用过的所有项目名称，还是仅列出在活跃任务中使用的名称。默认值为 "0"。

**summary.all.projects=0**  
如果设置为 "1"，在摘要报告中显示所有项目，即使没有待处理的任务。默认值为 "0"。

**complete.all.tags=1**  
可以是 "1" 或 "0"，确定选项卡完成脚本是否考虑你使用过的所有标签名称，或仅考虑在活跃任务中使用的名称。默认值为 "0"。

**list.all.tags=1**  
可以是 "1" 或 "0"，确定 'tags' 命令是列出你使用过的所有标签名称，还是仅列出在活跃任务中使用的名称。默认值为 "0"。

**print.empty.columns=1**  
可以是 "1" 或 "0"，确定是否打印没有任何任务数据的列。默认为 "0"。

**search.case.sensitive=1**  
可以是 "1" 或 "0"，确定关键字查找和对描述和注释的替换是否以区分大小写的方式进行。在大多数平台上默认为 "1"。在 Cygwin 上默认为 "0"，原因是旧的正则表达式库对大小写不敏感的问题。

**regex=1**  
控制是否启用正则表达式支持。默认值为 "1"。

**xterm.title=1**  
在运行报告时设置 xterm 窗口标题。默认为 "0"。

**expressions=infix|postfix**  
为中缀表达式（1 + 2）或后缀表达式（1 2 +）设置偏好。默认为中缀。

**json.array=1**  
确定 export 命令是否将 JSON 输出括在 '\[...\]' 中，并在每个导出的任务对象之后添加 ',' 以创建格式正确的 JSON 数组。使用 json.array=0 时，export 将原始 JSON 对象写入 STDOUT，每行一个。默认为 "1"。

**\_forcecolor=1**  
当输出不直接发送到 TTY 时，Taskwarrior 会自动关闭颜色。例如，此命令：

> $ task list > file

不会使用任何颜色。要覆盖此设置，请使用：

> $ task rc.\_forcecolor=yes list > file

默认为 "0"。

**active.indicator=\***  
要在 start.active 列中显示的字符或字符串。默认为 \*。

**tag.indicator=+**  
要在 tag.indicator 列中显示的字符或字符串。默认为 +。

**dependency.indicator=D**  
要在 depends.indicator 列中显示的字符或字符串。默认为 D。

**uda.&lt;name&gt;.indicator=U**  
要在 &lt;uda&gt;.indicator 列中显示的字符或字符串。默认为 U。

**recurrence=1**  
控制是否启用重复，以及重复任务是否继续生成新的任务实例。默认为 "1"。

如果你正在同步多个客户端，建议你在主客户端上设置 'recurrence=1'，在所有其他客户端上设置 'recurrence=0'。这是一个重复错误的解决方案。

**recurrence.confirmation=prompt**  
控制对重复任务的更改是否在有或没有确认的情况下传播到其他子任务。值 'yes' 意味着在没有确认的情况下传播更改。值 'no' 意味着不传播更改，也不要求确认。值 'prompt' 每次都会提示你。默认为 'prompt'。

**recurrence.indicator=R**  
要在 recurrence_indicator 列中显示的字符或字符串。默认为 R。

**recurrence.limit=1**  
要显示的未来重复任务数。默认为 1。例如，如果添加一个周末重复任务，截止日期为明天，并且 recurrence.limit 设置为 2，那么报告将列出 2 个待处理的重复任务，一个用于明天，一个用于一周后的明天。

**abbreviation.minimum=2**  
任何缩写命令/值的最小长度。这意味着 "ve"、"ver"、"vers"、"versi"、"versio" 都将等同于 "version"，但 "v" 不会。默认值为 2。

**debug=0**  
Taskwarrior 有一个调试模式，导致诊断输出被显示。通常这不是任何人想要的，但当报告错误时，调试输出可能很有用。它也可以帮助解释命令行是如何被解析的，但这些信息以开发人员友好的方式显示，而不是用户友好的方式。

打开调试会自动设置 debug.hooks=1 和 debug.parser=1（如果它们还没有分配的值）。默认为 "0"。

**debug.hooks=0**  
控制 hook 系统诊断级别。级别 0 表示无诊断。级别 1 显示 hook 调用。级别 2 还显示退出状态和 I/O。

**debug.parser=0**  
控制解析器诊断级别。级别 0 显示无诊断。级别 1 显示最终解析树。级别 2 显示解析所有阶段的解析树。级别 3 显示表达式求值详情。

**obfuscate=0**  
当设置为 '1' 时，会用 'xxx' 替换所有报告文本。这对于在错误报告中共享报告输出很有用。默认值为 '0'。

**alias.rm=delete**  
Taskwarrior 支持命令别名。此别名为 delete 命令提供了替代名称（rm）。你可以使用别名为任何命令提供替代名称。你可能使用的几个命令实际上是别名——例如 'history' 报告或 'export'。

**burndown.cumulative=1**  
可以是 "1" 或 "0"，控制 burndown 命令的行为。当设置为 1 时，它会汇总所有完成的任务，否则它们只会在任务完成的区间内被绘制。默认为 1。

## 日期

**dateformat=Y-M-D**  

**dateformat.report=**  

**dateformat.holiday=YMD**  

**dateformat.edit=Y-M-D H:N:S**  

**dateformat.info=Y-M-D H:N:S**  

**dateformat.annotation=**  

**report.X.dateformat=Y-M-D**  
这是一个字符串，定义了 Taskwarrior 如何格式化日期值。配置变量的优先级顺序为 report.X.dateformat 然后 dateformat.report 然后 dateformat 用于格式化报告中的截止日期。如果 report.X.dateformat 和 dateformat.report 都未设置，那么 dateformat 将应用于日期。输入的日期以及报告中显示的所有其他日期都根据 dateformat 进行格式化。

默认值是 ISO-8601 标准：Y-M-D。该字符串可能包含以下字符：

        m  最小位数月份，例如 1 或 12
        d  最小位数日期，例如 1 或 30
        y  两位数年份，例如 09 或 12
        D  两位数日期，例如 01 或 30
        M  两位数月份，例如 01 或 12
        Y  四位数年份，例如 2009 或 2015
        a  工作日的短名称，例如 Mon 或 Wed
        A  工作日的长名称，例如 Monday 或 Wednesday
        b  月份的短名称，例如 Jan 或 Aug
        B  月份的长名称，例如 January 或 August
        v  最小位数周，例如 3 或 37
        V  两位数周，例如 03 或 37
        h  最小位数小时，例如 3 或 21
        n  最小位数分钟，例如 5 或 42
        s  最小位数秒，例如 7 或 47
        H  两位数小时，例如 03 或 21
        N  两位数分钟，例如 05 或 42
        S  两位数秒，例如 07 或 47
        J  三位数儒略日，例如 023 或 365
        j  儒略日，例如 23 或 365
        w  工作日，例如 0 表示星期一，5 表示星期五

> 字符 'v'、'V'、'a' 和 'A' 只能用于格式化打印的日期（不解析它们）。

> 该字符串也可能包含其他字符作为间隔符或格式。dateformat 的其他值的示例：

        d/m/Y  会用于输入和输出 24/7/2009
        yMD    会用于输入和输出 090724
        M-D-Y  会用于输入和输出 07-24-2009

> dateformat.report 的其他值的示例：

        a D b Y (V)   会输出 "Fri 24 Jul 2009 (30)"
        A, B D, Y     会输出 "Friday, July 24, 2009"
        wV a Y-M-D    会输出 "w30 Fri 2009-07-24"
        yMD.HN        会输出 "110124.2342"
        m/d/Y H:N     会输出 "1/24/2011 10:42"
        a D b Y H:N:S 会输出 "Mon 24 Jan 2011 11:19:42"

> 当至少有一个全局日期字段被设置时，未定义的字段被设置为其最小有效值（月份和日期为 1，小时、分钟和秒为 0）。否则，它们被设置为 "now" 的相应值。例如：

        8/1/2013  使用 m/d/Y   意味着 2013 年 8 月 1 日午夜（推断）
        8/1 20:40 使用 m/d H:N 意味着 2013 年 8 月 1 日（推断）20:40

**date.iso=1**  
启用 ISO-8601 日期支持。默认值为 "1"。

## 日历

**weekstart=Sunday**  
确定一周的起始日。有效值仅为 Sunday 或 Monday。默认值为 "Sunday"。

**displayweeknumber=1**  
确定在使用 "task calendar" 命令时是否显示周数。周数取决于一周的起始日。默认值为 "1"。

**due=7**  
这是定义任务何时被认为是到期的天数，并相应地进行着色。默认值为 7。

**calendar.details=sparse**  
如果设置为"full"，运行"task calendar"将显示落在日历周期内的有截止日期任务的
详情。相应的天数将在日历中进行颜色编码。如果设置为"sparse"，只有相应的天数
将被颜色编码，不会显示详情。通过将变量设置为"none"来关闭显示有截止日期的详情。
默认值为"sparse"。

**calendar.details.report=list**  
运行"task calendar"命令时显示有截止日期任务详情时要运行的报告。默认值为"list"。

**calendar.offset=0**  
如果为"1"，日历报告中的第一个月将由calendar.offset.value中指定的偏移值有效地
改变。默认值为"0"。

**calendar.offset.value=-1**  
要应用于日历报告中第一个月的偏移值。默认值为"-1"。

**calendar.holidays=none**  
如果设置为full，运行"task calendar"将通过对相应天数进行颜色编码来在日历中
显示假期。还显示带有假期日期和名称的详细列表。如果设置为sparse，只有这些天
会被颜色编码，不会显示有关假期的详细信息。通过将变量设置为none来关闭假期显示。
默认值为"none"。

**calendar.legend=1**  
决定日历图例是否显示。默认值为"1"。

**calendar.monthsperline=N**  
决定"task calendar"命令在屏幕上显示多少个月份。默认值是屏幕能容纳的
数量。如果指定的月份数量超过屏幕能容纳的数量，Taskwarrior将仅显示
能够容纳的数量。

## 日志条目

**journal.time=0**  
可能为"1"或"0"，决定'start'和'stop'命令在执行时是否应记录注释。
默认值为"0"。对应注释的文本由以下设置控制：

**journal.time.start.annotation=Started task**  
当执行start命令并设置了journal.time时记录的注释的文本。

**journal.time.stop.annotation=Stopped task**  
当执行stop命令并设置了journal.time时记录的注释的文本。

**journal.info=1**  
启用时，此设置导致每个任务的更改日志由'info'命令显示。默认值为"1"。

## 假期

假期可以直接在.taskrc文件中输入，也可以通过在.taskrc中指定的include文件输入。
对于单日假期，需要给定名称和日期：

        holiday.towel.name=Day of the towel
        holiday.towel.date=20100525

对于跨越多天的假期（如度假），您可以使用开始日期和结束日期：

        holiday.sysadmin.name=System Administrator Appreciation Week
        holiday.sysadmin.start=20100730
        holiday.sysadmin.end=20100805

> 日期应根据dateformat.holiday变量中的设置输入。

> 以下假期是自动计算的：耶稣受难日（goodfriday）、复活节（easter）、
> 复活节星期一（eastermonday）、耶稣升天节（ascension）、
> 五旬节（pentecost）。这些假期的日期是给定的关键字：

        holiday.eastersunday.name=Easter
        holiday.eastersunday.date=easter

请注意，Taskwarrior发行版包含可以像这样包含的示例假期文件：

        include holidays.en-US.rc

## 依赖关系

**dependency.reminder=1**  
决定依赖关系链违反是否生成提醒。

**dependency.confirmation=1**  
决定依赖关系链修复是否需要确认。

## 颜色控制

**color=1**  
可能为"1"或"0"。决定Taskwarrior是否使用颜色。当为"0"时，
将使用破折号（-----）来为列标题下划线。

**fontunderline=1**  
决定即使启用了颜色，是否应使用字体下划线或ASCII破折号来下划线列标题。

Taskwarrior有多种着色规则。它们对应任务的特定属性，例如任务到期或处于活跃状态，
并指定该任务的自动着色。根据您的终端，有效颜色列表可以通过运行以下命令获得：

> **task colors**

> 请注意，此处未列出默认值 - 默认值现在对应于dark-256.theme（Linux）
> 和dark-16.theme（其他）主题值。着色规则如下所示：

> **color.due.today** 任务在今天到期  
> **color.active** 任务已启动，因此是活跃的。  
> **color.scheduled** 任务已安排，因此准备好工作。  
> **color.until** 任务有过期日期。  
> **color.blocking** 任务在依赖关系中阻止另一个任务。  
> **color.blocked** 任务被依赖关系阻止。  
> **color.overdue** 任务已逾期（到期早于现在）。  
> **color.due** 任务即将到期。  
> **color.project.none** 任务未分配项目。  
> **color.tag.none** 任务没有标签。  
> **color.tagged** 任务至少有一个标签。  
> **color.recurring** 任务是循环的。  
> **color.completed** 任务已完成。  
> **color.deleted** 任务已删除。

> 要禁用具有默认值的着色规则，请将值设置为空，例如：
>
>         color.tagged=
>
> > 默认情况下，规则生成的颜色会混合。这具有通过产生任何特定规则不直接使用的
> > 组合来传达附加信息的优点。
> >
> > 但是，颜色混合可能会产生不期望的高亮组合。在这些情况下，
> > 使用以下选项禁用此行为：
>
> **rule.color.merge=1**  
> 可以是"1"或"0"。当为"0"时，禁用由不同颜色规则生成的颜色的合并。
> 如果您的配色方案产生令人不愉快的前景色和背景色组合，请使用此选项。
>
> 有关颜色详细信息，请参见task-color(5)手册页。

某些属性（如标签、项目和关键字）可以有自己的着色规则。

**color.tag.X=yellow**  
为任何具有标签X的任务着色。

<!-- -->

**color.project.X=on green**  
为分配给项目X的任何任务着色。

<!-- -->

**color.keyword.X=on blue**  
为任何在描述或任何注释中包含X的任务着色。

<!-- -->

**color.uda.X=on green**  
为具有用户定义属性X的任何任务着色。

<!-- -->

**color.uda.X.VALUE=on green**  
为将用户定义的属性X设置为VALUE的任何任务着色。

<!-- -->

**color.uda.X.none=on green**  
为不具有用户定义的属性X的任何任务着色。

<!-- -->

**color.error=white on red**  
为任何错误消息着色。

<!-- -->

**color.warning=bold red**  
为任何警告消息着色。

<!-- -->

**color.header=green**  
为在报表输出之前打印的任何消息着色。

<!-- -->

**color.footnote=green**  
为最后打印的任何消息着色。

<!-- -->

**color.summary.bar=on green**  
为总结进度栏着色。应包含背景颜色。

<!-- -->

**color.summary.background=on black**  
为总结进度栏着色。应包含背景颜色。

<!-- -->

**color.calendar.today=black on cyan**  
日历中今天的颜色。

<!-- -->

**color.calendar.due=black on green**  
日历中有截止日期任务的天数的颜色。

<!-- -->

**color.calendar.due.today=black on magenta**  
日历中今天有截止日期任务的颜色。

<!-- -->

**color.calendar.overdue=black on red**  
日历中有逾期任务的天数的颜色。

<!-- -->

**color.calendar.scheduled=black on orange**  
日历中有计划任务的天数的颜色。

<!-- -->

**color.calendar.weekend=bright white on black**  
日历中周末的颜色。

<!-- -->

**color.calendar.holiday=black on bright yellow**  
日历中假期的颜色。

<!-- -->

**color.calendar.weeknumber=black on white**  
日历中周数的颜色。

<!-- -->

**color.label=**  
为报告标签着色。默认为不使用颜色。

<!-- -->

**color.label.sort=**  
为排序列的报告标签着色。默认为color.label。

<!-- -->

**color.alternate=on rgb253**  
交替任务的颜色。这是为报告中的每个其他任务应用特定颜色，
这可以使视觉上分离任务更容易。当由于长描述或注释而导致任务跨多行显示时，
这特别有用。

<!-- -->

**color.history.add=on red**  

  
**color.history.done=on green**

  
**color.history.delete=on yellow**

> 为ghistory报告图表的条形着色。默认为红色、绿色和黄色条形。

**color.burndown.pending=on red**  

  
**color.burndown.started=on yellow**

  
**color.burndown.done=on green**

> 为burndown报告图表的条形着色。默认为红色、绿色和黄色条形。

**color.undo.before=red**  

  
**color.undo.after=green**

> undo命令使用的颜色，指示即将被还原的更改的前后值。
>
> 当前不支持。

**color.sync.added=green**  

  
**color.sync.changed=yellow**

  
**color.sync.rejected=red**

> sync命令输出的颜色。

**rule.precedence.color=due.today,active,blocking,blocked,overdue,due,**  
**scheduled,keyword.,project.,tag.,uda.,recurring,**
**tagged,completed,deleted**

此设置指定颜色规则的优先级，从最高到最低。请注意，前缀'color.'被省略（为简洁起见），
并且任何通配符值（color.tag.XXX）被缩短为'tag.'，这将所有特定标签规则放在同一优先级，
同样是为了简洁。

<!-- -->

**color.debug=green**  

为所有调试输出着色（如果启用）。

## 紧迫性

紧迫性计算使用多项式，其中有几项，每一项都有一个可配置的系数。这些系数是：

**urgency.blocking.coefficient=8.0**  

阻止任务的紧迫性系数

**urgency.blocked.coefficient=-5.0**

> 被阻止任务的紧迫性系数

**urgency.due.coefficient=12.0**

> 到期日期的紧迫性系数

**urgency.waiting.coefficient=-3.0**

> 等待状态的紧迫性系数

**urgency.active.coefficient=4.0**

> 活跃任务的紧迫性系数

**urgency.scheduled.coefficient=5.0**

> 计划任务的紧迫性系数

**urgency.project.coefficient=1.0**

> 项目的紧迫性系数

**urgency.tags.coefficient=1.0**

> 标签的紧迫性系数

**urgency.annotations.coefficient=1.0**

> 注释的紧迫性系数

**urgency.age.coefficient=2.0**

> 任务年龄的紧迫性系数

**urgency.age.max=365**

> 最大年龄（以天为单位）。经过这么多天后，由于年龄的原因，任务的紧迫性
> 将不再增加。

**urgency.user.tag.&lt;tag&gt;.coefficient=...**

> 特定标签系数。

**urgency.user.tag.next.coefficient=15.0**

> 标签'next'的紧迫性系数。

**urgency.user.project.&lt;project&gt;.coefficient=...**

> 特定项目系数。

**urgency.user.keyword.&lt;keyword&gt;.coefficient=...**

> 特定描述关键字系数。

**urgency.uda.&lt;name&gt;.coefficient=...**

> UDA数据的存在/不存在。

**urgency.uda.&lt;name&gt;.&lt;value&gt;.coefficient=...**

> UDA数据的特定值。

系数反映紧迫性计算中各种项的相对重要性。这些是默认值，可能被修改以适应您的
偏好，但仔细考虑任何修改是很重要的。

**urgency.inherit=0**

> 实际上不是系数。启用时，阻止任务会继承它们阻止的任务中发现的最高紧迫性值。
> 这是递归完成的。建议将urgency.blocking.coefficient和
> urgency.blocked.coefficient设置为0.0，以便此设置最有用。

## 默认值

**default.project=foo**  
为*task add*命令提供默认项目名称（如果您未指定）。默认值为空。

**default.due=...**  
为*task add*命令提供默认到期日期（如果您未指定）。您可以使用日期或
相对于"现在"的持续时间值。默认值为空。

**default.scheduled=...**  
为*task add*命令提供默认计划日期（如果您未指定）。您可以使用日期或
相对于"现在"的持续时间值。默认值为空。

**uda.&lt;name&gt;.default=...**  
为使用*task add*命令时的UDA字段提供默认值（如果您未指定值）。默认值为空。

**default.command=next**  
提供一个默认命令，每当Taskwarrior在没有参数的情况下调用时都会运行。例如，
如果设置为：

<!-- -->

        default.command=project:foo list

> 那么如果未指定命令，Taskwarrior将运行"project:foo list"命令。
> 这意味着仅通过键入

        $ task
        [task project:foo list]

        ID Project Pri Description
         1 foo     H   Design foo
         2 foo         Build foo

## 报告

可以使用以下配置变量自定义报告。可以使用每个报告的相应变量设置输出列、
其标签和排序顺序。每个报告名称用作"命令"名称。例如

**task overdue**  

**report.X.description**  
运行"task help"命令时报告X的描述。

**report.X.columns**  
这是列和格式指示符的逗号分隔列表。有关选项和示例的完整列表，请参见命令
'task columns'。

**report.X.context**  
一个布尔值，表示给定的报告是否应该尊重（应用）当前活动的上下文。
请参见"上下文"部分了解有关上下文的详细信息。默认为1。

**report.X.labels**  
生成报告X时将使用的每个列的标签。标签是逗号分隔列表。

**report.X.sort**  
生成的报告X中任务的排序顺序。排序顺序通过使用列ID后缀为"+"表示升序或"-"
表示降序来指定。排序ID用逗号分隔。例如：

<!-- -->

        report.list.sort=due+,priority-,start.active-,project+

另外，在"+"或"-"之后，可能有一个solidus"/"，表示列值改变后有中断。
例如：

        report.minimal.sort=project+/,description+

此排序顺序现在指定每个项目之间都有一个列表换行符。列表换行符只是一条
空行，提供视觉分组。

"none"的特殊排序值表示不需要排序，任务将以选择它们的顺序（如果有）呈现。

"random"的特殊排序值表示任务将按随机顺序排序。每次运行带有'sort:random'
的报告时，都会生成一个新的随机顺序。

**report.X.filter**  
这添加了一个过滤器到报告X，以便只有与过滤标准匹配的任务才会在生成的报告中显示。

有一个'report.timesheet.filter'的特殊情况，即使'timesheet'报告不是很可定制的，
也可以指定它。

**report.X.dateformat**  
这添加了一个dateformat到报告X，它将被"due date"列使用。如果未设置，
则将按此顺序使用dateformat.report和dateformat。有关序列占位符的详细信息，
请参见**日期**部分。

**report.X.annotations**  
这增加了在报告中控制任务的注释输出的可能性。已弃用。改用带有格式的
**description**列（例如，**description.count**）。

Taskwarrior附带许多预定义报告，它们是：  

**next**  
列出最重要的任务。

**long**  
列出所有待处理任务和所有数据，匹配指定的条件。

**list**  
列出所有匹配指定条件的任务。

**ls**  
匹配指定条件的所有任务的简短列表。

**minimal**  
所有与指定条件匹配的任务的最小列表。

**newest**  
显示最新任务。

**oldest**  
显示最旧的任务。

**overdue**  
列出匹配指定条件的逾期任务。

**active**  
列出匹配指定条件的活跃任务。

**completed**  
列出匹配指定条件的已完成任务。

**recurring**  
列出匹配指定条件的循环任务。

**waiting**  
列出匹配指定条件的所有等待任务。

**all**  
列出所有匹配指定条件的任务。

**blocked**  
列出所有具有依赖关系的任务。

## 用户定义的属性

用户定义的属性（UDA）是一种扩展机制，允许您为Taskwarrior定义新属性来存储和
显示。一个这样的例子是可以用来存储与任务相关的时间估计的'estimate'属性。
此'estimate'属性未内置到Taskwarrior中，但通过一些简单的配置设置，您可以
指示Taskwarrior存储此项目，并为自定义报告和过滤器提供对其的访问。

这允许您扩展Taskwarrior以适应您的工作流程，或打破规则并使用Taskwarrior存储
和同步不一定与任务相关的数据。

一个重要的限制是，由于这是一个允许定义任何新属性的开放系统，Taskwarrior无法
理解该属性的含义。因此，虽然Taskwarrior会忠实地存储、修改、报告、排序和过滤
您的UDA，但它对其一无所知。例如，如果您定义一个名为'estimate'的UDA，
Taskwarrior不会知道此值是周、小时、分钟、金钱还是其他资源计数。

**uda.&lt;name&gt;.type=string|numeric|uuid|date|duration**  

定义称为'&lt;name&gt;'的UDA，指定类型。

<!-- -->

**uda.&lt;name&gt;.label=&lt;column heading&gt;**  

为称为'&lt;name&gt;'的UDA提供默认报告标签。

<!-- -->

**uda.&lt;name&gt;.values=A,B,C**  

仅对于'string'类型UDA，这提供可接受值的逗号分隔列表。在此示例中，
'&lt;name&gt;' UDA只能包含值'A'、'B'或'C'，但也可能不包含任何值。

请注意，值的顺序很重要，表示从最高（'A'）到最低（'C'）的排序顺序。

请注意，允许空白值。

<!-- -->

**uda.&lt;name&gt;.default=...**  

为称为'&lt;name&gt;'的UDA提供默认值。

<!-- -->

**示例'estimate'UDA**  
此示例显示了一个'estimate' UDA，它为任务的大小存储特定值。注意'trivial'
后面的空白值。

**uda.estimate.type=string**  
**uda.estimate.label=Size Estimate**  
**uda.estimate.values=huge,large,medium,small,trivial,**

> 请注意，值是排序的
>
> huge &gt; large &gt; medium &gt; small &gt; trivial &gt; ''

## 上下文

上下文设置是一种机制，允许用户设置永久过滤器，从而避免需要重复指定一个过滤器。
有关使用情况的更多详细信息，请参见task(1)手册页。

当前上下文与所有用户提供的上下文的定义一起存储在.taskrc文件中。

**context=&lt;name&gt;**  

存储当前活动的上下文的值。

<!-- -->

**context.&lt;name&gt;.read=&lt;filter&gt;**  

  
**context.&lt;name&gt;.write=&lt;modifications&gt;**

> 存储名为&lt;name&gt;的读取或写入上下文的定义。读取上下文是上下文
> 处于活动状态时应用的默认过滤器。写入上下文是上下文处于活动状态时
> 应用到新添加任务的默认修改。

**context.&lt;name&gt;.rc.&lt;key&gt;=&lt;value&gt;**  

rc类型允许为当前上下文覆盖任何配置参数，例如，如果应将home上下文的
默认命令更改为home_report，则可以添加以下语句：

context.home.rc.default.command=home_report

## 同步

这些配置设置用于连接和同步任务与任务服务器。

# 致谢和版权

版权所有©2006 - 2021 T. Babej、P. Beckingham、F. Hernandez。

此手册页最初由Federico Hernandez编写。

Taskwarrior在MIT许可证下分发。有关更多信息，请参见
https://www.opensource.org/licenses/mit-license.php。

# 参见

**task(1),** **task-color(5),** **task-sync(5)**

有关Taskwarrior的更多信息，请参见以下内容：

官方网站位于  
&lt;https://taskwarrior.org&gt;

官方代码仓库位于  
&lt;https://github.com/GothenburgBitFactory/taskwarrior&gt;

您可以通过发送电子邮件与项目联系  
&lt;support@GothenburgBitFactory.org&gt;

# 报告缺陷

Taskwarrior中的缺陷可以报告到问题跟踪器  
&lt;https://github.com/GothenburgBitFactory/taskwarrior/issues&gt;
