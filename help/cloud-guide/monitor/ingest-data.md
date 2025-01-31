---
title: 数据摄取
description: 了解如何在New Relic中查看和管理Commerce数据摄取。
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 数据摄取

New Relic依靠丰富的数据提供有效的监测和分析，但大型数据集可能会影响及时的结果、性能和合规性。 本主题提供有关管理数据摄取的一些指南，以及优化数据以使其最有效的策略。

New Relic提供了一个&#x200B;_数据管理_&#x200B;视图，该视图按数据源汇总了您的计划使用情况。

**要查看您的引入数据和源**：

1. 从您的New Relic用户菜单中，单击&#x200B;**[!UICONTROL Manage your data]**。
1. 在&#x200B;_管理_&#x200B;列表中单击&#x200B;**[!UICONTROL Data management]**。

   ![数据管理](../../assets/new-relic/data-ingestion.png)

   **[!UICONTROL Data ingestion]**选项卡显示当天摄取的数据以及数据源。
“数据保留”选项卡显示并控制数据的存储时间。

1. 选择&#x200B;**[!UICONTROL Limits]**&#x200B;选项卡并查看您帐户的限制。

Adobe Commerce的数据源包括：

- **APM事件** — 在图表和仪表板中使用的事件数据
- **基础架构** — 进程和主机指标，如CPU、存储、网络
- **日志记录** - CDN、APM和应用程序服务器的日志

日志数据在摄取中占很大比例。 了解如何[查看和分析日志数据](log-management.md#view-and-analyze-log-data)，并与您的Adobe代表合作，针对数据摄取和保留需求制定策略。 有关[管理数据摄取](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/)的更多信息，请参阅&#x200B;_New Relic文档_。
