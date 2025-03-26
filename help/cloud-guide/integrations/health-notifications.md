---
title: 运行状况通知
description: 了解如何在Adobe Commerce on cloud infrastructure项目中为Slack的磁盘空间使用情况配置PagerDuty、电子邮件和PagerDuty通知。
feature: Cloud, Observability, Integration
exl-id: 5a7f37e9-e8f9-4b6b-b628-60dcaa60cc64
source-git-commit: c3c708656e3d79c0893d1c02e60dcdf2ad8d7c7c
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# 运行状况通知

云基础架构上的Adobe Commerce可监控您的入门环境或专业集成环境中所有应用程序和服务的磁盘空间使用情况。 空间不足的数据库磁盘可能会导致数据损坏。 运行状况检查每5分钟进行一次，可通过电子邮件或其他外部服务通知您。 运行状况通知出现三个低磁盘警告：

- **警告** — 可用磁盘空间降到20%以下
- **关键** — 可用磁盘空间降至10%以下
- **完全清除** — 发生低磁盘事件后，可用磁盘空间返回到20%以上

>[!NOTE]
>
>在Pro Production环境中，您可以使用New Relic的Managed Alerts for Adobe Commerce警报策略来监视磁盘空间。 查看[监视托管警报的性能](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)。

## 电子邮件通知

健康电子邮件集成需要一个源地址和至少一个收件人地址。 您可以为`from-address`和`recipients`地址使用相同的电子邮件地址。 以下示例向两个收件人注册健康电子邮件集成：

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slack渠道通知

Slack是一种外部服务，它使用称为机器人的交互式应用程序在聊天室中发布消息。 在Slack中接收运行状况通知之前，必须为您的Slack组创建自定义[机器人用户](https://api.slack.com/bot-users)。 为您的渠道或渠道配置机器人用户后，保存Slack提供的[机器人令牌](https://api.slack.com/docs/token-types#bot)以注册您的集成。 以下示例在Slack渠道中注册运行状况通知：

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuty通知

PagerDuty是一项外部服务，可通知待命团队成员存在重要问题。 您必须先创建使用Events API版本2的[PagerDuty集成](https://developer.pagerduty.com/v2/docs/integrating)，然后才能在PagerDuty中接收运行状况通知。 使用集成密钥或&#x200B;_路由密钥_&#x200B;注册您的集成。 以下示例使用路由键为PagerDuty注册通知：

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```

## 日志管理

要增加可用磁盘空间，可以截断或删除环境中的日志文件。 如果启用了logrotate，请首先下载日志的备份副本，然后删除它们：

```bash
rm -rf some-log-file.log.gz
```

或者，您可以截断单个日志文件以减小其大小。 有关日志文件截断的详细示例，请参阅视频教程截断日志文件{target="_blank"}。
