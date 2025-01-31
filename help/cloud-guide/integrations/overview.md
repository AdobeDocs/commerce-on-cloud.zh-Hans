---
title: 集成概述
description: 了解适用于您的Adobe Commerce on cloud基础架构项目的第三方集成选项。
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# 集成概述

集成对于使用外部服务(如Git托管或Slack机器人)和维护当前开发流程（如使用GitHub中的代码审核拉取请求函数）非常有用。 您可以在云基础架构项目上向Adobe Commerce添加以下集成：

![集成](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**使用Cloud CLI添加集成**：

以下命令开始交互式提示，以选择新集成的类型和选项。

```bash
magento-cloud integration:add
```

**列出为您的项目配置的集成**：

```bash
magento-cloud integration:list
```

示例响应：

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB 控制台]

**要使用[!DNL Cloud Console]**&#x200B;添加集成：

1. 在&#x200B;_项目设置_&#x200B;中，单击&#x200B;**[!UICONTROL Integrations]**。

1. 单击集成类型或单击&#x200B;**[!UICONTROL Add integration]**。

1. 逐步完成集成类型选择和配置步骤。

1. 添加集成后，该集成将显示在集成视图上的列表中。

>[!ENDTABS]

## Commerce webhook

您可以使用[ENABLE_WEBHOOKS全局变量](../environment/variables-global.md#enable_webhooks)在Cloud项目中配置Commerce Webhooks。 Commerce webhook将请求发送到外部服务器，以响应Commerce生成的事件。 [_Webhooks指南_](https://developer.adobe.com/commerce/extensibility/webhooks)详细描述了此功能。

## 通用Webhook

您可以使用对`POST` JSON消息的自定义webhook集成来捕获云基础架构和存储库事件并报告到&#x200B;_webhook_ URL。

**要添加webhook URL，请使用以下语法**：

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` — 指定`webhook`集成类型。
- `url` — 提供可接收JSON消息的webhook URL。

示例响应显示了一系列提示，这些提示提供了自定义集成的机会。 使用默认（空白）响应，可发送有关项目中所有环境的所有事件的消息。

您可以自定义集成以报告特定的[事件](#events-to-report)，例如将代码推送到分支。 例如，您可以指定`environment.push`事件在用户将代码推送到分支时发送消息：

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

您可以选择报告处于`pending`、`in_progress`或`complete`状态的事件：

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

对于特定环境，您可以&#x200B;_包含_&#x200B;或&#x200B;_排除_&#x200B;消息：

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

集成完成后，您将收到值的摘要：

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 更新现有集成

您可以更新现有集成。 例如，使用以下方式将状态从`complete`更改为`pending`：

```bash
magento-cloud integration:update --states=pending <int-id>
```

示例响应：

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 要报告的事件

| 事件 | 描述 |
| ----- | :-----------|
| `environment.access.add` | 已授予用户访问环境的权限 |
| `environment.access.remove` | 已从环境中移除用户 |
| `environment.activate` | 已使用环境“激活”分支 |
| `environment.backup` | 用户触发了快照 |
| `environment.branch` | 已使用管理控制台创建分支 |
| `environment.deactivate` | 已“停用”分支。 代码仍然存在，但环境被破坏了 |
| `environment.delete` | 已删除一个分支 |
| `environment.initialize` | 使用首次提交初始化的项目的`master`分支 |
| `environment.merge` | 已使用管理控制台或API合并活动分支 |
| `environment.push` | 用户将代码推送到分支 |
| `environment.restore` | 用户还原了快照 |
| `environment.route.create` | 已使用管理控制台创建路由 |
| `environment.route.delete` | 已使用管理控制台删除路由 |
| `environment.route.update` | 已使用管理控制台修改了路由 |
| `environment.subscription.update` | 由于订阅已更改，`master`环境已调整大小，但此处没有内容更改 |
| `environment.synchronize` | 某个环境已从其父环境中重新复制了数据或代码 |
| `environment.update.http_access` | 环境的HTTP访问规则已修改 |
| `environment.update.restrict_robots` | 已启用或禁用了所有机器人块功能 |
| `environment.update.smtp` | 已针对环境启用或禁用电子邮件发送 |
| `environment.variable.create` | 已创建变量 |
| `environment.variable.delete` | 已删除变量 |
| `environment.variable.update` | 已修改变量 |
| `project.domain.create` | 已创建域并将其添加到项目中 |
| `project.domain.delete` | 已删除与项目关联的域 |
| `project.domain.update` | 与项目关联的域已更新 |
