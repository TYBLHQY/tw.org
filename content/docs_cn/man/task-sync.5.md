---
title: 'Taskwarrior - task-sync.5 for task-3.4.2 (中文)'
viewport: 'width=device-width, initial-scale=1'
layout: single
---
# 名称

task-sync - 关于 **task**(1) 数据同步能力的讨论和说明。

# 介绍

Taskwarrior 能够向一个主机同步您的任务。
这有几个优点：  
- 您的任务可以从多台机器上访问。  
- 您的任务得到服务器端的备份和同步。  
- 文件冲突得到解决。

例如，您可能想要在笔记本电脑和手机上拥有任务的副本。

注意：同步的副作用是，一旦更改已同步，就无法撤销。这意味着每次运行同步时，都不再可能撤销以前的操作。

# 管理同步

## 添加副本

要添加新副本，请将新的空副本配置为与现有副本相同，然后运行 `task sync`。

## 何时同步

对于服务器同步，常见的解决方案是运行

        $ task sync

定期，例如通过 **cron**(8)**.**

# 配置

Taskwarrior 为同步任务提供了几个选项：

\- 同步到专门设计用于处理 Taskwarrior 数据的服务器。 + 同步到云存储提供商。目前支持 AWS 和 GCP。 - 同步到本地的磁盘文件。

对于大多数这些，您需要用于加密和解密任务的加密密钥。这可以是任何秘密字符串，且必须与所有共享任务的副本相匹配。

        $ task config sync.encryption_secret <encryption_secret>

诸如 **pwgen**(1) 的工具可以生成合适的密钥值。

## 同步服务器

要将任务同步到同步服务器，您需要从服务器管理员处获得以下信息：

  
- 服务器的 URL（如"https://tw.example.com/path"）  
- 标识您的任务的客户端 ID（"client_id"）

使用以下详细信息配置 Taskwarrior：

        $ task config sync.server.url               <url>
        $ task config sync.server.client_id         <client_id>

请注意，URL 必须包括方案，如"http://"或"https://"。

$ task config sync.server.origin &lt;origin&gt;

是"sync.server.url"的已弃用同义词。

## Google Cloud Platform

要将任务同步到 GCP，请使用 GCP 控制台创建新项目，并在该项目中创建新的 Cloud Storage 存储桶。存储桶的默认设置就可以了。

使用以下方式向项目进行身份验证：

        $ gcloud config set project $PROJECT_NAME
        $ gcloud auth application-default login

然后配置 Taskwarrior：

        $ task config sync.gcp.bucket               <bucket-name>

但是，如果您的 `application-default` 已被其他应用程序使用，您可以使用您自己的服务帐户凭据

首先，在导航菜单中导航到"IAM 和管理"部分，然后选择"角色"。

在"角色"部分的顶部菜单栏中，单击"创建角色"。为新角色提供适当的名称和描述。

使用筛选器"Service:storage"（而不是"按角色筛选权限"输入框）为新角色添加权限。选择以下权限：

\- storage.buckets.create - storage.buckets.get -
storage.buckets.update - storage.objects.create -
storage.objects.delete - storage.objects.get - storage.objects.list -
storage.objects.update

创建新角色。

在左侧边栏中，导航到"服务帐户"。

在"服务帐户"部分的顶部菜单栏中，单击"创建服务帐户"。为新服务帐户提供适当的名称和描述。选择您刚刚创建的角色并完成服务帐户创建过程。

现在，在服务帐户仪表板中，单击新服务帐户，然后在顶部菜单栏中选择"密钥"。单击"添加密钥"以创建和下载新密钥（JSON 密钥）。

然后配置 Taskwarrior：

        $ task config sync.gcp.bucket               <bucket-name>
        $ task config sync.gcp.credential_path      <absolute-path-to-downloaded-credentials>

## Amazon Web Services

要将任务同步到 AWS，请选择靠近您的区域，并使用 AWS 控制台创建新的 S3 存储桶。存储桶的默认设置就可以了。特别是，确保未启用生命周期策略，因为它们可能会自动删除或转换对象，从而影响数据可用性。

您还需要一个具有以下策略的 AWS IAM 用户，其中 BUCKETNAME 是存储桶的名称。可以为多个 Taskwarrior 客户端配置相同的用户。

        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "TaskChampion",
                    "Effect": "Allow",
                    "Action": [
                        "s3:PutObject",
                        "s3:GetObject",
                        "s3:ListBucket",
                        "s3:DeleteObject"
                    ],
                    "Resource": [
                        "arn:aws:s3:::BUCKETNAME",
                        "arn:aws:s3:::BUCKETNAME/*"
                    ]
                }
            ]
        }

要创建这样的用户，请在 IAM 控制台中创建新策略，在策略编辑器中选择 JSON 选项，并粘贴策略。单击"下一步"并为策略指定名称，如"TaskwarriorSync"。接下来，创建新用户，使用您选择的名称，选择"直接附加策略"，然后选择新创建的策略。

您需要为新用户配置访问密钥。在用户列表中查找用户，打开"安全凭证"标签，然后单击"创建访问密钥"并按照步骤操作。

此时，您可以选择如何将这些凭据提供给 Taskwarrior。最简单的方法是在 Taskwarrior 配置中包括它们：

        $ task config sync.aws.region              <region>
        $ task config sync.aws.bucket              <bucket-name>
        $ task config sync.aws.access_key_id       <access-key-id>
        $ task config sync.aws.secret_access_key   <secret-access-key>

或者，您可以设置 AWS CLI 配置文件，使用您选择的配置文件名称，如"taskwarrior-creds"：

        $ aws configure --profile taskwarrior-creds

输入访问密钥 ID 和秘密访问密钥。默认区域和格式不重要。然后配置 Taskwarrior：

        $ task config sync.aws.region              <region>
        $ task config sync.aws.bucket              <bucket-name>
        $ task config sync.aws.profile             taskwarrior-creds

要使用 AWS 的默认凭据源（如环境变量、默认配置文件或实例配置文件），请设置

        $ task config sync.aws.region              <region>
        $ task config sync.aws.bucket              <bucket-name>
        $ task config sync.aws.default_credentials true

## 本地同步

## 本地同步

为了利用同步节省磁盘空间的副作用而不设置远程服务器，可以本地同步任务。要配置本地同步：

        $ task config sync.local.server_dir /path/to/sync

默认配置是同步到任务目录中的数据库（"data.location"）。

# 运行 TASKCHAMPION-SYNC-SERVER

TaskChampion 同步服务器是支持多个用户的 HTTP 服务器。用户由客户端 ID 标识，不同客户端 ID 的用户完全独立。任务数据由 Taskwarrior 加密，同步服务器永远不会看到未加密的数据。

该服务器的开发位于
https://github.com/GothenburgBitFactory/taskchampion-sync-server。

## 添加新用户

要向服务器添加新用户，请使用 `uuidgen` 或在线 UUID 生成器之类的工具创建新的客户端 ID。无需为该新客户端 ID 配置服务器：每当显示新客户端 ID 时，同步服务器将自动为其创建新用户。将 ID 与 URL 一起提供给用户以包括在其 Taskwarrior 配置中。用户应创建自己的"encryption_secret"。

# 避免重复循环任务

如果运行多个同步到同一服务器的客户端，您需要在主客户端（最常使用的客户端）上运行此命令：

        $ task config recurrence on

在其他客户端上，运行：

        $ task config recurrence off

这可保护您免受同步/重复错误的影响。

# 替代方案：文件共享服务

有许多文件共享服务，例如 DropBox、Amazon S3、Google Drive、SkyDrive 等。此技术涉及在文件托管服务控制的共享目录中存储 .task 目录。

同步发生迅速，尽管当没有网络连接且在两个不同位置修改任务时，可能会遇到冲突情况。这是因为文件托管服务只知道文件，不知道如何合并任务。通过从不在两台机器上修改相同任务而不进行干预同步来避免此问题。

设置只需创建目录并修改您的 data.location 配置变量，如下所示：

        $ task config data.location /path/to/shared/directory

优势：  
- 良好的客户端支持  
- 易于设置  
- 透明使用

劣势：  
- 任务不能正确合并

# 版权和信用

Copyright (C) 2006 - 2021 T. Babej, P. Beckingham, F. Hernandez.

Taskwarrior 在 MIT 许可证下分发。有关更多信息，请参见
https://www.opensource.org/licenses/mit-license.php。

# 参见

**task(1),** **taskrc(5),** **task-color(5),**

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
