---
title: "Taskwarrior - 命令"
layout: single
---

# 命令

## 命令

这里是按字母顺序排列的命令。
特定于版本的功能标有首次提供该功能的版本。

  * [`add` - 添加新任务](add/)
  * `annotate` - 为任务添加注释
  * [`append` - 为任务描述附加单词](append/)
  * [`calc` {{< label >}}2.4.0{{< /label >}} - 表达式计算器](calc/)
  * `config` - 修改配置设置
  * `context` - 管理上下文
  * [`count` - 计算与过滤器匹配的任务数](count/)
  * `delete` - 将任务标记为已删除
  * `denotate` - 从任务中移除注释
  * [`done` - 完成任务](done/)
  * `duplicate` - 克隆现有任务
  * `edit` - 启动文本编辑器以修改任务
  * `execute` - 执行外部命令
  * [`export` - 以 JSON 格式导出任务](export/)
  * `help` - 显示高级帮助，速查表
  * `import` - 导入 JSON 格式的任务
  * [`log` - 记录已完成的任务](log/)
  * `logo` - 显示 Taskwarrior 徽标
  * [`modify` - 修改一个或多个任务](modify/)
  * [`prepend` - 为任务描述前缀单词](prepend/)
  * `purge` {{< label >}}2.6.0{{< /label >}} - 完全删除任务，而不是改变状态为 `deleted`
  * `start` - 开始处理任务，使其激活
  * `stop` - 停止处理任务，不再活跃
  * [`synchronize` - 与其他实例同步任务](synchronize/)
  * `undo` - 撤销上次更改
  * `version` - 版本详情和版权

## 可自定义报告

[可自定义报告](../report/) 可以根据您的需要进行修改。
它们都以相同的方式工作，主要区别是显示的 [列](columns/)、应用的 [过滤器](../filter/) 和执行的排序。

因为所有可自定义报告都以相同方式工作，[`list` 报告](list/) 是唯一讨论的报告。

  * `active` - 已启动的任务
  * `all` - 待处理、已完成和已删除的任务
  * `blocked` - 被其他任务阻止的任务
  * `blocking` - 阻止其他任务的任务
  * `completed` - 已完成的任务
  * [`list` - 待处理任务](list/)
  * `long` - 待处理任务，长形式
  * `ls` - 待处理任务，简短形式
  * `minimal` - 待处理任务，最小形式
  * `newest` - 最新的待处理任务
  * `next` - 最紧急的任务
  * `oldest` - 最旧的待处理任务
  * `overdue` - 逾期任务
  * `ready` - 待处理、不受阻、已安排的任务
  * `recurring` - 待处理的循环任务
  * `unblocked` - 不被阻止的任务
  * `waiting` - 隐藏的、等待中的任务

## 固定报告

固定报告的自定义选项很少，通常仅限于颜色。

  * [`burndown.daily` - 燃尽图，按天](burndown/)
  * [`burndown.monthly` - 燃尽图，按月](burndown/)
  * [`burndown.weekly` - 燃尽图，按周](burndown/)
  * `calendar` - 日历和假日
  * `colors` - 演示所有支持的颜色
  * [`columns` - 报告列列表和支持的格式](columns/)
  * `commands` {{< label >}}2.5.0{{< /label >}} - 命令列表，带有它们的行为
  * `diagnostics` - 显示诊断信息，用于故障排除
  * `ghistory.annual` - 历史图表，按年
  * `ghistory.monthly` - 历史图表，按月
  * `ghistory.weekly` {{< label >}}2.6.0{{< /label >}} - 历史图表，按周
  * `ghistory.daily` {{< label >}}2.6.0{{< /label >}} - 历史图表，按天
  * `history.annual` - 历史报告，按年
  * `history.monthly` {{< label >}}2.6.0{{< /label >}} - 历史报告，按月
  * `history.weekly` {{< label >}}2.6.0{{< /label >}} - 历史报告，按周
  * `history.daily` - 历史报告，按天
  * `ids` - 过滤的任务 ID 列表
  * [`information` - 显示所有属性](info/)
  * `projects` - 过滤的项目列表，带有任务计数
  * `reports` - 可用报告列表
  * `show` - 过滤的配置设置列表
  * `stats` - 过滤的统计数据
  * `summary` - 过滤的项目摘要
  * `tags` - 过滤的标签列表
  * `timesheet` - 周时间表报告
  * `udas` - 所有定义的 UDA 的详情
  * `uuids` - 过滤的 UUID 列表

## 帮助程序命令

帮助程序命令都以下划线开头，它们的存在是为了为第三方附加组件（例如 shell 完成脚本）提供支持。

帮助程序命令不生成多余的输出，因此易于解析。
帮助程序命令不适合常规使用，但这并不能阻止您使用它们。

  * `_aliases` - 活跃别名列表
  * `_columns` - 支持的列列表
  * `_commands` - 支持的命令列表
  * `_config` - 配置设置名称列表
  * `_context` - 定义的上下文名称列表
  * [`_get` - DOM 访问器](_get/)
  * `_ids` - 过滤的任务 ID 列表
  * `_projects` - 过滤的项目名称列表
  * `_show` - `name=value` 配置设置列表
  * `_tags` - 使用中的过滤标签列表
  * `_udas` - 配置的 UDA 名称列表
  * [`_unique` - {{< label >}}2.5.0{{< /label >}} 指定属性的唯一值列表](_unique/)
  * `_urgency` - 过滤的任务紧急程度列表
  * `_uuids` - 过滤的待处理 UUID 列表
  * `_version` - 任务版本（和可选的 git 提交）
  * `_zshattributes` - Zsh 格式的任务属性列表
  * `_zshcommands` - Zsh 格式的命令列表
  * `_zshids` - Zsh 格式的 ID 列表
  * `_zshuuids` - Zsh 格式的 UUID 列表
Theme swatch