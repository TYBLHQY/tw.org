---
title: 'Taskwarrior - task-color.5 for task-3.4.2 (中文)'
viewport: 'width=device-width, initial-scale=1'
layout: single
---
# 名称

task-color - Taskwarrior 命令行任务管理器的颜色教程。

# 自动禁用颜色

Taskwarrior 会检测输出是否被发送到终端。
当 Taskwarrior 检测到输出不是发送到终端时，颜色会被禁用。

        $ task list > file.txt

我们真的想要所有这些颜色控制代码在文件中吗？Taskwarrior 假设您不想要，并在生成输出时暂时将颜色设置为"off"。这解释了以下命令的输出：

        $ task show | grep '^color '
        color                        off

无论设置如何，它始终返回"off"，因为输出被发送到管道。

如果您想要那些颜色代码，可以通过将 _forcecolor 变量设置为 on 来覆盖此行为，如下所示：

        $ task config _forcecolor on
        $ task config | grep '^color '
        color                        on

或通过临时覆盖它，如下所示：

        $ task rc._forcecolor=on config | grep '^color '
        color                        on

# 可用颜色

Taskwarrior 有一个"color"命令，将显示它能够显示的所有颜色。尝试此：

        $ task color

输出无法在手册页中复制，但您应该会看到一组颜色样本。您看到的数量取决于您的终端程序呈现它们的能力。

您至少应该看到基本颜色和效果 - 如果您看到，那么您具有 16 色支持。如果您的终端支持 256 颜色，您会知道的！

# 16 色支持

基本颜色支持通过命名颜色提供：

        black, red, blue, green, magenta, cyan, yellow, white

前景色（对于文本）只需指定为上述颜色之一，或根本不指定以使用默认终端文本颜色。

背景颜色通过使用单词"on"和上述颜色之一来指定。一些示例：

        green                 # 绿色文本，默认背景色
        green on yellow       # 绿色文本，黄色背景
        on yellow             # 默认文本颜色，黄色背景

这些颜色可以通过使前景加粗或使背景明亮来进一步修改。一些示例：

        bold green
        bold white on bright red
        on bright cyan

单词的顺序并不重要，所以以下是等价的：

        bold green
        green bold

但"on"很重要 - "on"前的颜色是前景色，"on"后的颜色是背景色。

有一个额外的"underline"属性可以使用：

        underline bold red on black

以及一个"inverse"属性：

        inverse red

Taskwarrior 有一个帮助您可视化这些颜色组合的命令。尝试此：

        $ task color underline bold red on black

您可以使用此命令查看各种颜色组合的工作方式。您还将看到一些显示的示例颜色，如上所述，以及所请求的示例。

某些组合看起来非常好，有些看起来很糟。不同的终端程序确实实现了略有不同的"红色"版本，例如，因此您可能会看到跨机器的一些意外变化。您的显示器的亮度也是一个因素。

# 256 色支持

使用 256 颜色采用相同的形式，但名称不同，某些颜色可以以不同的方式引用。首先是按颜色序数，如下所示：

        color0
        color1
        color2
        ...
        color255

这使您可以访问所有 256 种颜色，但对您的帮助不大。此范围是 8 种基本颜色（color0 - color7）、8 种更亮的变化（color8 - color15）的组合。然后是 216 种颜色块（color16 - color231）。然后是 24 种灰色块（color232 -
color255）。

大的 216 种颜色块（6x6x6 = 216）代表一个颜色立方体，可以通过 0 到 5 的每个分量颜色的 RGB 值来寻址。值 0 表示此分量颜色的无，值 5 表示最强烈的分量颜色。例如，明亮的红色指定为：

        rgb500

而较暗的红色将是：

        rgb300

请注意，三个数字代表三个分量值，因此在此示例中，5、0 和 0 代表红色 = 5、绿色 = 0、蓝色 = 0。将强烈的红色与没有绿色和蓝色组合会产生红色。类似地，蓝色和绿色是：

        rgb005
        rgb050

另一个示例 - 明亮的黄色 - 是明亮的红色和明亮的绿色的混合，但没有蓝色分量，所以明亮的黄色被寻址为：

        rgb550

柔和的粉红色将被寻址为：

        rgb515

通过运行来查看您是否同意：

        $ task color black on rgb515

您可能会注意到大颜色块被表示为 6 个正方形。第一个正方形中的所有颜色的红色值为 0。第 6 个正方形中的所有颜色的红色值为 5。在每个正方形内，蓝色从左到右范围为 0 到 5，在每个正方形内绿色从上到下范围为 0 到 5。这个方案需要一些时间来习惯。

24 种灰色块也可以访问为 gray0 - gray23，从黑色到白色的连续坡道。

# 混合 16 色和 256 色

如果指定 16 色并在 256 色终端上查看，没有问题。如果尝试相反的操作，指定 256 色并在 16 色终端上查看，您会失望，甚至可能会惊恐。

存在某些有限的颜色映射 - 例如，如果您要指定此组合：

        red on gray3

您混合 16 色和 256 色规范。Taskwarrior 将红色映射到 color1 并继续。注意红色和 color1 的色调并不完全相同。

还要注意，在处理 256 种颜色时没有粗体或亮属性，但仍然有下划线可用。

# 图例

如果您运行此命令，Taskwarrior 将显示 .taskrc 或主题中使用的所有定义的颜色的示例：

        $ task color legend

这为您提供了每种颜色的示例，因此您可以看到效果，而无需必然创建一组满足每个规则条件的任务。

# 规则
that specify a color, and the conditions under which that color is used.
By example, let us add a few tasks:

        $ task add project:Home priority:H pay the bills               (1)
        $ task add project:Home            clean the rug               (2)
        $ task add project:Garden          clean out the garage        (3)

我们可以添加一个颜色规则，对"Home"项目中的所有任务使用蓝色背景：

        $ task config color.project.Home 'on blue'

我们在"on blue"周围使用引号，因为有两个词，但它们在 .taskrc 文件中代表一个值。现在假设我们想对所有清洁工作使用粗体黄色文本颜色：

        $ task config color.keyword.clean 'bold yellow'

现在任务 2 会发生什么，它属于"Home"项目（蓝色背景）并且也是清洁任务（粗体黄色前景色）？颜色被组合，任务显示为"粗体黄色在蓝色上"。

颜色规则可以按项目和描述关键字应用（如图所示），也可以按优先级（或缺乏优先级）、按活动状态、按到期或逾期、按标签或具有特定标签（也许是最有用的规则）或按循环任务应用。

可以创建一个非常丰富多彩的规则混合。通过 256 色支持，这些颜色可以进行精细和互补，但在没有仔细的情况下，这可能是一个视觉混乱。当心！

在这种情况下，请考虑使用"rule.color.merge=no"选项来禁用颜色混合。

颜色规则的优先级由配置变量"rule.precedence.color"确定，默认包含：

        deleted,completed,active,keyword.,tag.,project.,overdue,scheduled,due.today,due,blocked,blocking,recurring,tagged,uda.

这些只是删除了"color."前缀的颜色规则。规则"color.deleted"具有最高优先级，"color.uda."具有最低优先级。

此处显示的关键字规则作为"keyword."对应于通配符模式，意思是"color.keyword.*"，或换句话说所有关键字规则。

也有"color.project.none"、"color.tag.none"和"color.uda.priority.none"来特别代表缺失的数据。

# 主题

Taskwarrior 支持主题。这真的意味着具有能够将其他文件包括在 .taskrc 文件中的能力，可以包括不同的颜色规则集合。

要了解颜色主题外观，请尝试将此条目添加到 .taskrc 文件：

        include dark-256.theme

您可以使用任何标准的 Taskwarrior 主题：

        dark-16.theme
        dark-256.theme
        dark-blue-256.theme
        dark-gray-256.theme
        dark-green-256.theme
        dark-red-256.theme
        dark-violets-256.theme
        dark-yellow-green.theme
        light-16.theme
        light-256.theme
        solarized-dark-256.theme
        solarized-light-256.theme
        dark-default-16.theme
        dark-gray-blue-256.theme
        no-color.theme

请记住，如果您使用的是深色背景的终端，使用深色主题会看到更好的结果。

您还可以看到主题如何使用命令对各种任务进行颜色处理：

        $ task color legend

更好的是，创建自己的并分享。我们很乐意在 &lt;https://taskwarrior.org&gt; 上托管主题文件。

# 版权和信用

Copyright (C) 2006 - 2021 T. Babej, P. Beckingham, F. Hernandez.

Taskwarrior 在 MIT 许可证下分发。有关更多信息，请参见
https://www.opensource.org/licenses/mit-license.php。

# 参见

**task(1),** **taskrc(5),** **task-sync(5)**

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
