---
title: 设置通知
description: 了解如何在云基础架构环境中为Adobe Commerce配置通知。
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# 设置通知

默认情况下，云基础架构上的Adobe Commerce将生成和部署操作写入Adobe Commerce根应用程序目录中的`app/var/log/cloud.log`文件。 或者，您也可以将日志发送到消息传送系统(如Slack和电子邮件)以接收实时通知。

例如，您可以发送一条Slack消息，在部署失败时提醒一组人员，并提示调查哪里出了问题。

## 计划通知

在配置通知之前，请考虑以下事项：

- 您要接收哪种通知(Slack消息、电子邮件和两者)？
- 您希望在日志中查看多少详细信息？
- 您想在何处设置通知（集成、暂存、生产）？

例如，在初始开发期间，您可能希望电子邮件通知显示有关集成环境的详细日志，以帮助您在部署到暂存环境之前调试问题。 当您准备好部署到暂存或生产环境时，您可能希望使用包含较少详细信息的Slack消息。

>[!NOTE]
>
>用于设置通知的配置文件位于项目目录的根目录下，因此当您将更改推送到任何环境时，该文件均适用。 如果要按环境自定义通知，则必须先修改配置文件，然后再将其推送到该环境。

## 配置通知

要配置通知，请执行以下操作：

1. 在本地工作站上，转到您的项目目录。
1. 在项目根目录中的`.magento.env.yaml`文件中，添加消息系统设置，包括首选通知[日志级别](log-handlers.md#log-levels)。

   例如，要配置Slack _和_&#x200B;电子邮件配置，请使用以下内容：

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >云基础架构上的Adobe Commerce仅在部署阶段发送电子邮件。

1. 提交更改并将其推送到远程服务器。

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### 示例Slack配置

以下示例显示了仅限该Slack的配置：

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` — 您的Slack[用户令牌](https://api.slack.com/docs/token-types#user)。 您的用户令牌授权云基础架构上的Adobe Commerce发送消息。
- `channel` — 云基础架构上的Slack渠道Adobe Commerce的名称发送通知。
- `username` — 云基础架构上的Adobe Commerce用户名用于以Slack发送通知消息。
- `min_level` — 通知消息的最小日志级别。 我们建议使用`info`。

### 示例电子邮件配置

以下示例显示了仅用于电子邮件的配置：

>[!NOTE]
>
>云基础架构上的Adobe Commerce仅在部署阶段发送电子邮件。

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` — 云基础架构上的Adobe Commerce电子邮件地址将发送通知消息。
- `from` — 用于向收件人发送通知消息的电子邮件地址。
- `subject` — 电子邮件的说明。
- `min_level` — 通知消息的最小日志级别。 我们建议使用`notice`或`warning`。
