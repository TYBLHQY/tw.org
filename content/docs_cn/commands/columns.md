---
title: "Taskwarrior - 列命令"
---

# columns

`columns` 命令显示了所有可以列入自定义报告的可用列的列表。
每个列可能有多个格式，每个格式都提供了示例。

例如，`due` 日期可以按各种方式显示。
可以根据 `dateformat` 配置设置格式化日期。
其他格式包括朝代日数、POSIX 时代戳、ISO-8601 格式、「年龄」值或「倒计时」值。

每列都有默认格式，因此如果自定义报告指定：

```
report.my_list.columns=...,due,... 
```

那么使用默认格式。
其他格式可以这样使用：

```
report.my_list.columns=...,due.iso,...
```

以下是 `columns` 命令的示例输出：

```
Columns     Type    Modifiable Supported Formats Example
component   string  Modifiable default*
                               indicator
depends     string  Modifiable list*             1 2 10
                               count             [3]
                               indicator         D
description string  Modifiable combined*         Move your clothes down on to the lower peg
                                                   2015-12-28 Immediately before your lunch
                                                   2015-12-28 If you are playing in the match this afternoon
                                                   2015-12-28 Before you write your letter home
                                                   2015-12-28 If you're not getting your hair cut
                               desc              Move your clothes down on to the lower peg
                               oneline           Move your clothes down on to the lower peg 2015-12-28 Immediately before your lunch 2015-12-28 If you are playing in the match this afternoon 2015-12-28 Before you write your letter home 2015-12-28 If you're not getting your hair cut
                               truncated         Move your clothes do...
                               count             Move your clothes down on to the lower peg [4]
                               truncated_count   Move your clothes do... [4]
due         date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
end         date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
entry       date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
estimate    string  Modifiable default*
                               indicator
id          numeric Read Only  number*           123
imask       numeric Read Only  number*           12
mask        string  Read Only  default*          ++++---
modified    date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
parent      string  Read Only  long*             f30cb9c3-3fc0-483f-bfb2-3bf134f00694
                               short             f30cb9c3
priority    string  Modifiable default*
                               indicator
project     string  Modifiable full*             home.garden
                               parent            home
                               indented            home.garden
recur       string  Modifiable duration*         weekly
                               indicator         R
scheduled   date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
start       date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
                               active            *
status      string  Modifiable long*             Pending
                               short             P
tags        string  Modifiable list*             home @chore next
                               indicator         +
                               count             [2]
tracnumber  numeric Modifiable default*
                               indicator
tracsummary string  Modifiable default*
                               indicator
tracurl     string  Modifiable default*
                               indicator
until       date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
urgency     numeric Modifiable real*             4.6
                               integer           4
uuid        string  Read Only  long*             f30cb9c3-3fc0-483f-bfb2-3bf134f00694
                               short             f30cb9c3
wait        date    Modifiable formatted*        2015-12-28
                               julian            2457385.04894
                               epoch             1451308228
                               iso               20151228T131028Z
                               age               2min
                               relative          -2min
                               remaining
                               countdown         PT2M5S
         Modifiable default*
                               indicator
                               
* 表示默认格式，因此是可选的。例如，"due" 和 "due.formatted" 是等效的。
```
