---
title: "Taskwarrior - 简介"
---

# 简介

欢迎来到 Taskwarrior。
这是众多教程中的第一个，涵盖初次使用。

## 初次使用

作为首次使用者，你需要一个配置文件和一个数据目录。
Taskwarrior 会在你首次运行 Taskwarrior 时在你的主目录中为你创建这两个文件。
下面是一个示例：

```
$ task version
A configuration file could not be found in ~

Would you like a sample /home/alice/.taskrc created, so taskwarrior can
proceed? (yes/no) yes

task 2.4.4 built for linux
Copyright (C) 2006 - 2016 P. Beckingham, F. Hernandez.

Taskwarrior may be copied only under the terms of the MIT license, which may
be found in the taskwarrior source kit.

Documentation for taskwarrior can be found using 'man task', 'man taskrc',
'man task-color', 'man task-sync' or at https://taskwarrior.org
```

对该问题回答 `yes`。
在创建缺失的文件和目录后，你将看到当前版本显示。
刚刚创建的配置文件包含的内容很少。

```
$ cat ~/.taskrc
# [Created by task 2.4.4.dev 7/12/2015 09:09:09]
# Taskwarrior program configuration file.
# For more documentation, see https://taskwarrior.org or try 'man task',
# 'man task-color', 'man task-sync' or 'man taskrc'

# Here is an example of entries that use the default, override and blank values
#   variable=foo   -- By specifying a value, this overrides the default
#   variable=      -- By specifying no value, this means no default
#   #variable=foo  -- By commenting out the line, or deleting it, this uses the default

# Use the command 'task show' to see all defaults and overrides

# Files
data.location=~/.task

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
```

此文件中只有一个 `data.location` 条目。
这是因为 Taskwarrior 有一组内置的合理默认值，而此配置文件只包含对这些默认值的覆盖。

名为 `data.location` 的配置变量指向你的任务数据目录，其中包含一个 SQLite 数据库。
数据库目前是空的，因为还没有任务。

```
$ ls ~/.task
taskchampion.sqlite3
```

通常，你不需要查看该目录。
