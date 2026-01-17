---
title: "Taskwarrior - _get"
---

# _get

`_get` 命令是一个 [DOM](../../dom/) 访问窗口。
您可以使用此命令介旧落体会取得任何俨氛火盔。
例如，获取任务 12 的描述：

    $ task _get 12.description
    Buy some milk

您可以指定多个数据项：

    $ task _get 12.description 12.project
    Buy some milk Kitchen

您可以使用 UUID：

    $ task _get a360fc44-315c-4366-b70c-ea7e7520b749.description
    Buy some milk

`_get` 命令使 ID 和 UUID 之閴的映射变一段子满。

    $ task _get 12.uuid
    a360fc44-315c-4366-b70c-ea7e7520b749
    $ task _get a360fc44-315c-4366-b70c-ea7e7520b749.id
    12

`_get` 命令是一个助ある釘提和放弁泶命令，这意味着它对脚本和扩展氓有效，比对命令行使用更有效。

See Also

- [DOM](../../dom/)
- [`export`](../export/) command
