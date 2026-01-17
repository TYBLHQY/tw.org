---
title: "Taskwarrior - 分类"
---

# 分类

这是对问题的初步评估。
目的是使其朝着正确的方向发展，并确保在正确的背景下注意到它，这意味着确保每个问题都有合适的分类和版本。
新问题一直都在报告中，有时提交者填写字段，有时不填。
这无法控制。
其他一切都可以。

## 初步评估

- 将问题移至正确的项目。
  例如，有时问题被报告为 Taskwarrior 错误，但在 TaskChampion 中处理更好。
- 将问题移至正确的类型，即错误、功能或改进。
  有时很难区分，有些人精于以错误的形式报告功能请求，假设错误比功能请求获得更多关注。
  我们不会被这个愚弄。

## 错误

- 所有错误都应将"目标版本"设置为当前开发工作。
  这反映了需要在版本中修复尽可能多的错误的需要，因为错误是优先事项。
- "To"字段应为空。
  这表示目前没有人在处理它。
- "分类"通常设置为"core"，除非有更相关的分类。
- "优先级"应为"正常"，除非错误导致崩溃、功能缺失或最坏的情况：数据损坏。
  无论提交者设置什么优先级，都重新评估它，因为它不会影响任何东西。
  只有错误的真实性质会影响优先级。
- "Status" should stay as "New".
- "Due date", nope.
  We don't work that way.
- "Target version" has only two alternatives.
  Either it is a Taskwarrior bug in which case the version should be the current development effort ("2.4.0"), or it refers to an external script, and should be "extension".
- If the bug looks like a duplicate, try to find the other bug and link the two.
  It is not necessary to reject the bug.
  Sometimes duplicate bug reports contain complementary information, which is useful.
- If the bug can be replicated, please do so, and add your finding to the issue.
  More evidence is better, and often helps to narrow down the cause.
- If there is a clear workaround to the bug, add the information to the issue, so the submitter can continue to work.
  Helping the submitter work around the problem is important, because the eventual fix will take time, perhaps months.
  Always try to educate the user and don't forget that these issues are permanent and therefore searchable and useful to everyone.
- If a bug looks like a really big problem, tell people, make a noise.

## Features

Same as "Bugs" above , but with these differences:

- "Target version" should always be "BACKLOG".
  This is a non-specific future release.
  Features should be cherry-picked out of the "BACKLOG" version and moved to the current development version.
  They should not default to the current development version, where we have to defer them constantly.
  It gives the wrong impression to have a giant list of features under the current development effort - it makes people think the next release is going to be huge, which is rarely the case.
