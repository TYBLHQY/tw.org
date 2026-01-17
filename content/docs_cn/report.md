---
title: "Taskwarrior - 报告"
---

# 报告

Taskwarrior 有三种类型的报告。
有些内置报告无法修改，例如 `info` 和 `summary`。有些内置报告可以完全重新定义或消除，例如 `list` 和 `next`。最后，还有您自己的自定义报告。
要生成*所有*报告的列表，请使用 `reports` 命令：

    $ task reports

    Report           Description
    active           活跃的任务
    all              所有任务
    blocked          被阻止的任务
    blocking         阻止其他任务的任务
    burndown.daily   显示按日期的图形化燃尽图表
    burndown.monthly 显示按月份的图形化燃尽图表
    burndown.weekly  显示按周的图形化燃尽图表
    completed        完成的任务
    ghistory.annual  显示按年份的任务历史图形化报告
    ghistory.monthly 显示按月份的任务历史图形化报告
    history.annual   显示按年份的任务历史报告
    history.monthly  显示按月份的任务历史报告
    information      显示所有数据和元数据
    list             任务的大部分详情
    long             任务的所有详情
    ls               任务的少量详情
    minimal          任务的最少详情
    newest           最新的任务
    next             最紧急的任务
    oldest           最旧的任务
    overdue          超期的任务
    projects         显示使用的所有项目名称
    ready            最紧急的可行任务
    recurring        循环任务
    summary          显示按项目的任务状态报告
    tags             显示使用的所有标签列表
    unblocked        未被阻止的任务
    waiting          等待中（隐藏）的任务

    28 个报告

## 内置静态报告

通常，报告由一个数据表组成，其中一行数据对应一个任务，任务属性作为列表示。
但还有其他报告不符合这种结构。
这些是内置的静态报告，它们是不可修改的，因为它们很特殊并需要自定义代码。
这些报告是：

    burndown.daily
    burndown.monthly
    burndown.weekly
    calendar
    colors
    export
    ghistory.annual
    ghistory.monthly
    history.annual
    history.monthly
    information
    summary
    timesheet

每个报告都接受非标准参数，可能支持也可能不支持过滤器，并生成特定的输出。

## 内置可修改报告

这些报告是标准格式，使用标准参数，也是可修改的。

    active
    all
    blocked
    blocking
    completed
    list
    long
    ls
    minimal
    newest
    next
    oldest
    overdue
    ready
    recurring
    unblocked
    waiting

假设您想从这些报告之一中删除一列，例如从 `minimal` 报告中删除 `tags`。
首先查看 `minimal` 报告的现有列和标签：

    $ task show report.minimal.labels

    Config Variable       Value
    --------------------- ---------------------------
    report.minimal.labels ID,Project,Tags,Description

    $ task show report.minimal.columns

    Config Variable        Value
    ---------------------- ---------------------------------------
    report.minimal.columns id,project,tags.count,description.count

确定当前值后，只需使用新值进行覆盖：

    $ task config report.minimal.labels  'ID,Project,Description'
    ...
    $ task config report.minimal.columns 'id,project,description.count'

您可以将内置可修改报告视为一组默认自定义报告。

## 自定义报告

定义自定义报告很简单。
您可以控制显示的数据、排序顺序和列标签。
自定义报告只是一组配置值，这些值包括：

- 描述
- 列
- 标签
- 排序
- 过滤

让我们快速创建一个自定义报告，将其命名为 `simple`。这个报告将显示任务 ID、项目名称和描述。
我们需要收集上面列出的五个设置。

描述是最简单的，在这种情况下报告将被描述为：

    按项目的开放任务简单列表

这只是一个描述性标签，将在列出报告时使用。
接下来，我们需要指定报告中的列及其显示顺序。
这里 `_columns` 助手命令将显示可用的列：

    $ task _columns
    depends
    description
    due
    end
    entry
    foo
    id
    imask
    mask
    modified
    parent
    priority
    project
    recur
    scheduled
    start
    status
    tags
    until
    urgency
    uuid
    wait

这代表 Taskwarrior 存储的所有数据，因此代表可能显示在报告中的所有数据。
我们的 `simple` 报告将显示 id、project 和 description，这些都在列表中。
这意味着我们的列列表很简单：

    id,project,description

但每列也都有格式，`columns` 命令显示它们和示例。
以下是我们三列的格式：

    $ task columns id

    Columns Supported Formats Example
    ------- ----------------- ------------------------------------
    id      number*           123
    uuid    long*             f30cb9c3-3fc0-483f-bfb2-3bf134f00694
            short             f30cb9c3

这很容易，因为只有一种 `id` 格式。

    $ task columns project

    Columns Supported Formats Example
    ------- ----------------- -------------
    project full*             home.garden
            parent            home
            indented            home.garden

`project` 列有三种格式，默认的 `full` 是我们想要的。

    $ task columns description

    Columns     Supported Formats Example
    ----------- ----------------- -----------------------------------------------------------------------------
    description combined*         Move your clothes down on to the lower peg
                                    2014-02-08 Immediately before your lunch
                                    2014-02-08 If you are playing in the match this afternoon
                                    2014-02-08 Before you write your letter home
                                    2014-02-08 If you're not getting your hair cut
                desc              Move your clothes down on to the lower peg
                oneline           Move your clothes down on to the lower peg 2014-02-08 Immediately before ...
                                  this afternoon 2014-02-08 Before you write your letter home 2014-02-08 If ...
                truncated         Move your clothes do...
                count             Move your clothes down on to the lower peg [4]

description 有五种格式。
这次我们更喜欢 `count` 格式，所以我们的列列表现在是：

    id,project,description.count

标签是报告中的列标题标签。
有默认值，但我们希望像这样指定这些：

    ID,Proj,Desc

排序也很简单，我们希望任务按项目排序，然后按输入日期排序，即任务的创建日期。
这说明了一个任务属性，即使不可见也可以用在排序中。
排序顺序是：

    project+/,entry+

`+` 表示升序排列，但我们可以使用 `-` 表示降序。
`/` 斜线表示 `project` 是一个分隔列，这意味着在唯一值之间插入一个空白行，以实现视觉分组效果。 {{< label >}}2.4.0{{< /label >}}

最后，我们需要一个过滤器，否则我们的报告将只显示所有任务，这很少是想要的。
这里我们只想看到待处理的任务，这意味着过滤器是：

    status:pending

Now we have our report definition, so we just create the five configuration entries like this:

    $ task config report.simple.description 'Simple list of open tasks by project'
    $ task config report.simple.columns     'id,project,description.count'
    $ task config report.simple.labels      'ID,Proj,Desc'
    $ task config report.simple.sort        'project+/,entry+'
    $ task config report.simple.filter      'status:pending'

Note the equivalent report directly from the config file would look like that:

    report.simple.description=Simple list of open tasks by project
    report.simple.columns=id,project,description.count
    report.simple.labels=ID,Proj,Desc
    report.simple.sort=project+\/,entry+
    report.simple.filter=status:pending

And it is finished.
Run the report like this:

    $ task simple

    ID Proj Desc
    -- ---- -----------------
     1 Home Wash the windows
     3 Home Vacuum the floors

     2      Food shopping

Custom reports also show up in the help output.

    $ task help | grep simple
    task  simple              Simple list of open tasks by project

I can inspect the configuration.

    $ task show report.simple

    Config Variable           Value
    ------------------------- ------------------------------------
    report.simple.columns     id,project,description.count
    report.simple.description Simple list of open tasks by project
    report.simple.filter      status:pending
    report.simple.labels      ID,Proj,Desc
    report.simple.sort        project+/,entry+

Now the report is fully configured, it joins the others and is used in the same way.
