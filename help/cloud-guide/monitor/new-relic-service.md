---
title: New Relic服务
description: 了解Adobe Commerce在云基础架构项目中提供的New Relic服务。
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# New Relic服务概述

云基础架构上的所有Adobe Commerce项目都包括对New Relic服务的访问权限，以帮助监控性能并调查[!DNL Commerce]应用程序和云基础架构的事件。

以下New Relic功能可用于生产和暂存环境：

- [New Relic APM](#new-relic-apm) （专业版和入门版）
- [New Relic基础架构](#new-relic-infrastructure)（仅限Pro）
- [New Relic日志管理](#new-relic-log-management)（仅限Pro）

>[!INFO]
>
>Adobe Commerce项目上不提供其他New Relic功能。

## NEW RELIC APM

[用于应用程序性能管理(APM)的New Relic](https://docs.newrelic.com/introduction-apm/)是一个软件分析产品，可帮助您分析和改进应用程序交互。 New Relic APM适用于云基础架构项目上的所有Adobe Commerce，并提供以下功能：

- **关注特定交易** — 主动标记并监视网站中的关键客户操作，如添加到购物车、结帐或处理付款。
- **数据库查询监视** — 查找并监视影响性能的数据库查询。
- **应用程序映射** — 查看您的网站、扩展和外部服务中的所有应用程序依赖项。
- **[!DNL Apdex]分数** — 评估绩效并创建警报，以识别问题并在发生问题时通知您，例如受闪售或Web事件影响的网站绩效。 查看[Apdex得分](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/)。
- **Adobe Commerce的托管警报** — 使用此New Relic警报策略，根据行业最佳实践监控应用程序和基础架构性能。 查看[使用Adobe Commerce警报策略的托管警报监视性能](investigate-performance.md/#monitor-performance-with-managed-alerts)。
- **跟踪部署** — 监视部署事件并分析部署对整体性能的影响。 查看[跟踪部署](track-deployments.md)。

您的Adobe Commerce on cloud基础架构项目包括New Relic APM服务的软件以及许可证密钥。 您无需购买或安装任何其他软件。

## New Relic基础架构

Pro项目包括[New Relic基础架构(NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/)服务，该服务会自动与应用程序数据和性能分析连接，以提供动态服务器监控。 此服务在专业生产和暂存环境中可用。

## New Relic日志管理

所有云基础架构项目都包括[New Relic日志管理](log-management.md)。 该服务已预配置为聚合暂存和生产环境中的所有日志数据，并在集中式日志管理功能板中显示这些数据。
