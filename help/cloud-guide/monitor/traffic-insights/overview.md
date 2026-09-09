---
title: Adobe Commerce流量分析
description: 了解Adobe Commerce流量分析工具，以及它如何帮助您了解Adobe Commerce上的云基础架构项目流量。
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 流量分析

Adobe Commerce流量分析是一个New Relic One应用程序，可可视化[!DNL Adobe Commerce on Cloud Infrastructure] Fastly CDN流量。 它将读取已发送到New Relic的Fastly CDN访问日志行作为`Log`事件，并呈现一组精选的图表，其范围涵盖您选择的New Relic帐户和平台时间范围。 这无需手动编写New Relic的查询语言NRQL ，即可将商店的边缘流量可视化。

## 它有助于您调查

流量分析旨在帮助您解决三个常见问题：

- **CDN带宽超额** — 流量趋势高于合同允许。 将卷归因到重介质、大型文件、不可缓存的404页或缓存效率低下，进而归因到特定域、内容类型、URL或项目。
- **搜索机器人和爬虫负载** — 搜索引擎或AI爬虫产生的请求份额不成比例，从而影响缓存效率和原始负载。 查看哪些命名机器人最活跃，以及它们获取的确切内容。
- **恶意脚本和擦除程序** — 擦除、凭据填充、卡片测试、虚假帐户创建或第7层滥用。 显示Fastly Next-Gen WAF信号以及可疑流量背后的IP、子网和国家/地区。

在每种情况下，应用程序都会识别流量的&#x200B;*谁、什么、在哪里*。 通过Commerce和Fastly配置中的Fastly VCL规则、映像优化、缓存调整、速率限制或Adobe的[高级安全性](../../cdn/advanced-security.md)加载项来操作该信息。 [调查行动手册](investigation-playbook.md)涵盖了所有这些问题。

## 访问应用程序

- **直接链接：** [Adobe Commerce流量分析](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47)。
- **从New Relic One主屏幕** (one.newrelic.com) — 帐户订阅应用程序后，它将在主页上显示为自己的磁贴&#x200B;**Adobe Commerce流量分析**。
- **从顶部搜索栏（快速查找）** — 搜索`Adobe Commerce Traffic Insights`并从结果中选择它。
- **若要固定它以加快访问速度** — 使用应用程序磁贴或页眉上的星形或pin控件将其添加到收藏夹或左侧导航。 此控件的确切位置取决于帐户使用的New Relic UI版本。

## 本指南内容

- **[了解应用程序](understanding-the-app.md)** — 什么是“流量分析”，如何使用过滤器驱动，如何测量数字，以及数据可以告诉您哪些内容，哪些内容不能告诉您。
- **[调查行动手册](investigation-playbook.md)** — 针对应用程序构建要解决的三个问题（带宽超量、爬虫负载和恶意流量）推荐的方法。 其中每个参数都引用确认它的图表，并指定在手动缓解不足时Adobe的本机[高级安全性](../../cdn/advanced-security.md)提升路径。