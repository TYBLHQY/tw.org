---
title: "Taskwarrior - 详细程度"
---

# 详细程度

运行命令时，Taskwarrior 提供反馈，其数量可以控制。
有一个名为 `verbose` 的配置变量，可以设置为不同的值来控制详细程度。
默认情况下，值为：

    verbose=yes

这个值导致 Taskwarrior 显示所有反馈。
相反，它可以设置为：

    verbose=no

这限制了反馈的数量，但仍然保留有用的信息。
要删除所有反馈，请使用设置：

    verbose=nothing

## 细节控制

可以采用以下形式指定单个详细程度设置：

    verbose=footnote,label

在这种情况下，值由两个标记组成，它们在报告中的任务之后显示脚注，以及表列标签。

## 详细程度标记

* blank      - 在输出中插入空行以将任务列表与其他反馈分开。
* header     - 显示任务列表前的信息，涉及别名或默认命令。
* footnote   - 显示任务列表后的信息，如配置覆盖。
* label      - 显示表格输出的列标签。
* new-id     - 提供任何新任务的反馈与 ID（以及新删除或完成任务的 UUID）。
* new-uuid   - 提供任何新任务的反馈与 UUID。覆盖 new-id。用于自动化。
* affected   - 显示批量修改影响了多少任务。
* edit       - 使用 `edit` 命令时在模板中显示前言和附加文本。
* special    - 使用特殊标签时显示反馈。
* project    - 项目状态变更时显示反馈。
* sync       - 同步期间显示反馈。
* filter     - 显示使用的完整过滤器。
* override   - 如果 TASKDATA 或 TASKRC 被覆盖，在标题中显示。
* unwait     - 当任务离开 `waiting` 状态时的通知。在脚注中显示。
* context    - 显示当前上下文。在脚注中显示。
