---
title: "Taskwarrior - 导出命令"
---

# export

`export` 命令接受一个过滤器，并输出 JSON 格式的文本。
例如：

```
$ task 1 export
{"id":1,"description":"Buy milk","entry":"20141018T050231Z","modified":"20141018T050231Z","status":"pending","uuid":"a360fc44-315c-4366-b70c-ea7e7520b749","urgency":"2"}
```

此命令允许您从 Taskwarrior 提取所有数据，成为橜器可读、标准的格式。
`export` 命令的使用许多 Taskwarrior 扩展介入它们的数据。

## JSON 格式

JSON 的格式按照 [Taskwarrior JSON 格式](https://github.com/GothenburgBitFactory/taskwarrior/blob/develop/doc/devel/rfcs/task.md)，每行包含一个 JSON 对象，其中一个对象表示一个任务。

## 配置

有一个配置设置 `json.array`，默认为 'on'，但当设置为 'off' 时生成一组单个 JSON 对象。
在下面的示例中，你看到一组 JSON 对象（`{...}`），每行一个：

```
$ task 1-2 export rc.json.array=off
{"id":1,"description":"Buy milk", ...}
{"id":2,"description":"Buy potatoes" ...}
```

将 `json.array` 设置为 'on'，会发出类似的结构，但任务对象以列表形式呈现，每行一个：

```
$ task 1-2 export rc.json.array=on
[{"id":1,"description":"Buy milk", ...},
{"id":2,"description":"Buy potatoes" ...}]
```

你使用哪种设置取决于你如何处理 JSON。

## 限制

`export` 命令 JSON 包含未存储的元数据。
包括 `id` 和 `urgency` 值是因为某些外部程序想看到这个，但这些是派生值，不是存储值。

## 另见

- [`_get`](../_get/) 命令
- `import` 命令
