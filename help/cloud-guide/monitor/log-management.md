---
title: New Relic日志管理
description: 了解如何使用New Relic日志
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# New Relic日志管理

所有云基础架构项目都包括[New Relic日志管理](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)。 该服务已预配置为聚合暂存和生产环境中的所有日志数据，并在集中式日志管理功能板中显示这些数据。

聚合数据包括以下日志中的信息：

- `~/var/log`目录中的所有`ece-tools`和应用程序日志
- `var/log/platform/<project-ID>`目录中的云服务日志
- Fastly CDN和WAF

当项目连接到New Relic时，您可以使用New Relic日志服务完成以下任务：

- 使用New Relic查询搜索聚合日志数据
- 通过New Relic Logs应用程序可视化日志数据
- 创建自定义图表、功能板和警报
- 从单个功能板排除性能问题

## 查看和分析日志数据

使用New Relic Logs应用程序在聚合的日志数据中搜索，并排查应用程序、基础架构、CDN和WAF错误。 您可以使用从New Relic APM和基础架构服务中收集的日志数据来创建图表、功能板和警报。

**要使用New Relic Logs应用程序**：

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)。

1. 从Explorer导航菜单中选择&#x200B;**日志**。

1. 验证是否在&#x200B;_所有日志_&#x200B;视图的顶部选择了您的帐户。

1. 选择日志查询的时间范围。

1. 若要查看云服务（来自`~/var/log/`的日志）的基础结构日志数据，请在&#x200B;_搜索日志_&#x200B;字段中输入查询字符串`has: "filePath"`。 然后，单击&#x200B;**[!UICONTROL Query logs]**。

   日志文件的名称存储在`filePath`列中，包含日志文件的完整路径。

   ![云项目New Relic服务日志数据](../../assets/new-relic/var-log-query.png)

1. 要查看Fastly日志数据，请在&#x200B;_搜索日志_&#x200B;字段中输入查询字符串`has: "client_ip"`。 然后，单击&#x200B;**[!UICONTROL Query logs]**。

1. 要按国家/地区代码筛选Fastly日志结果，请单击&#x200B;**[!UICONTROL Add column]**，然后选择&#x200B;**[!UICONTROL geo_country_code]**。

   ![云项目New Relic CDN日志属性筛选器](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>您可以从&#x200B;_保存的视图_&#x200B;下拉列表中保存查询视图。 单击&#x200B;**[!UICONTROL Create new]**，提供名称，选择选项，然后单击&#x200B;**[!UICONTROL Save view]**。
>
>请参阅&#x200B;_New Relic文档_&#x200B;网站上的[日志管理入门](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)和[New Relic查询语言简介](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/)。
