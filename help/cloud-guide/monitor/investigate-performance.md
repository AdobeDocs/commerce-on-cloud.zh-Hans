---
title: New Relic监控
description: 了解如何访问New Relic功能板并分析云基础架构项目中的Adobe Commerce数据。
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# New Relic监控

New Relic使用PHP代理连接并监视您的基础结构和[!DNL Commerce]应用程序。 在Cloud环境连接到New Relic后，您可以登录到New Relic帐户以查看代理收集的数据。

在&#x200B;_APM和服务_&#x200B;页面上，选择&#x200B;**摘要**&#x200B;以查看有关应用程序的事务性信息。 此视图可帮助您识别潜在故障，并检查应用程序和服务的整体运行状况。

![Cloud项目New Relic概述页面](../../assets/new-relic/dashboard.png)

从这个视图中，您可以跟踪遇到缓慢响应或瓶颈、应用程序吞吐量、Web错误等问题的事务。

查看跟踪的数据：

- **最耗时** — 通过并行跟踪请求来确定耗时。 例如，在产品和类别视图中，您可能使用了最高的交易时间。 如果客户帐户页面突然在时间消耗方面排名靠前，则您的应用程序可能会受到呼叫或查询拖动性能的影响。

- **最高吞吐量** — 根据传输字节的大小和频率识别点击量最多的页面。

所有收集的数据都详细介绍了传输数据、查询或&#x200B;_Redis_&#x200B;数据的操作所花费的时间。 如果查询导致问题，New Relic会提供信息以跟踪和响应这些问题。

>[!TIP]
>
>有关使用此数据解决应用程序性能问题的详细信息，请参阅&#x200B;_New Relic帮助中心_&#x200B;中的[使用Adobe Commerce解决性能问题](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html?lang=zh-Hans)。

## 使用受管警报监视性能

Adobe为Adobe Commerce _警报策略提供_&#x200B;托管警报，以跟踪性能度量。 该策略包括一组警报，当基础架构或应用程序问题影响站点性能时，这些警报会设置阈值并触发警告和严重通知。 该策略会跟踪生产环境中的以下量度：

| 量度 | 数据收集 | 可用性 |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex]分数 | APM | Pro和Starter |
| CPU使用情况 | NRI | Pro |
| 磁盘空间 | NRI | Pro |
| 错误率 | APM | Pro和Starter |
| 内存使用率 | NRI | Pro |
| MariaDB查询加载 | NRI | Pro |
| Redis内存 | NRI | Pro |

当站点基础设施或应用程序条件触发警报阈值时，New Relic会发送警报通知，以便您能够主动解决问题。 有关警报阈值的详细信息以及解决触发警报的问题的故障排除步骤，请参阅&#x200B;_Adobe Commerce帮助中心_&#x200B;中的[Adobe Commerce托管警报](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=zh-Hans)。

>[!TIP]
>
>对于Pro暂存和集成环境以及入门环境，请使用[运行状况通知](../integrations/health-notifications.md)来监视磁盘空间。

>[!PREREQUISITES]
>
>- **New Relic凭据** — 用于登录到云项目的New Relic帐户的凭据
>- **活动的New Relic集成** — 验证您的云环境是否已连接到New Relic
>- **工作流通知** — 配置至少一个[工作流](#set-up-a-workflow-for-notifications)以接收警报通知

**查看Adobe Commerce的托管警报**：

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)。

1. 找到Adobe Commerce _的_&#x200B;托管警报：

   - 在Explorer导航菜单中，单击&#x200B;**[!UICONTROL Alerts & AI]**。

   - 在&#x200B;_检测_&#x200B;下，单击&#x200B;**[!UICONTROL Alert Conditions & Policies]**。

   - 验证在&#x200B;_警报条件和策略_&#x200B;视图顶部选择了您的帐户。

   - 在&#x200B;_策略_&#x200B;列表中，为Adobe Commerce **策略选择**&#x200B;托管警报。

     ![已生成警报策略](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >如果&#x200B;_Adobe Commerce托管警报_&#x200B;策略不可用，请参阅&#x200B;_Adobe Commerce帮助中心_&#x200B;中的[Adobe Commerce托管警报](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=zh-Hans)。

1. 单击&#x200B;**[!UICONTROL Alert conditions]**&#x200B;选项卡以查看策略中定义的警报条件。

## 创建警报策略

请勿修改“Adobe Commerce的托管警报”策略中包含的任何警报。 Adobe会随着时间的推移更新和改善此策略中的警报条件，这会覆盖您添加到策略的任何自定义设置。

您可以创建警报策略，而不是修改现有警报。 然后，将警报条件复制到新策略中。

>[!TIP]
>
>有关警报、警报策略和工作流的详细信息，请参阅&#x200B;_New Relic_&#x200B;文档中的[警报简介](https://docs.newrelic.com/docs/alerts/overview/)。

## 设置通知工作流

您现在可以设置&#x200B;_工作流_（以前称为通知渠道），以根据过滤的数据（如警报策略）接收有关网站性能的通知。 当应用程序或基础架构上的条件触发警报时，有关性能问题的通知将发送到与警报策略关联的所有工作流。 当问题被确认和关闭时，您也会收到通知。

New Relic提供了用于配置不同类型工作流通知(包括电子邮件、Slack、PagerDuty、Webhook等)的模板。

**配置工作流**：

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)。

1. 创建工作流。

   - 在Explorer导航菜单中，单击&#x200B;**[!UICONTROL Alerts & AI]**。

   - 在左侧导航栏的&#x200B;_扩充和通知_&#x200B;下，单击&#x200B;**[!UICONTROL Workflows]**。

   - 单击右侧的&#x200B;**[!UICONTROL Add a workflow]**。

     ![New Relic添加工作流](../../assets/new-relic/add-a-workflow.png)

   - 在&#x200B;_配置工作流_&#x200B;页面上，输入工作流的名称。

   - 在&#x200B;_筛选数据_&#x200B;部分中，从&#x200B;**[!UICONTROL Policy]**&#x200B;下拉列表中选择&#x200B;**[!UICONTROL Managed Alerts for Adobe Commerce]**。

   - 在&#x200B;_通知_&#x200B;部分中，选择一个渠道并按照说明操作。

   - 单击&#x200B;**[!UICONTROL Test workflow]**&#x200B;验证您的配置。

1. 单击&#x200B;**[!UICONTROL Activate workflow]**。

请参阅有关[工作流](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/)的New Relic文档。

>[!WARNING]
>
>“Adobe Commerce的托管警报”策略中的警报将默认工作流配置为通知在云基础架构客户上支持Adobe Commerce的Adobe团队。 请勿修改这些默认通道的配置，也不要删除分配给它们的警报策略。
