---
title: 了解应用程序
description: 了解Adobe Commerce流量分析的工作原理、如何使用过滤器推动流量、如何测量其数据以及其数据限制和性能。
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# 了解应用程序

[!DNL Adobe Commerce Traffic Insights]应用程序将原始Fastly内容分发网络(CDN)访问日志可视化为商店边缘流量的图片。 图表分为以下选项卡：

- **带宽** — 在一段时间内跨域、内容和资源类型以及云项目分配流量带宽的方式。
- **全页缓存性能** — 在边缘缓存产品详细信息页面(PDP)、产品列表页面(PLP)和内容管理系统(CMS)页面的动态店面HTML的效率如何。
- **机器人活动和请求分析** — 按已知机器人代理、地理位置、IP/子网、URL和Fastly Next-Gen Web Application Firewall (WAF)信号划分的流量。

第四个应用程序内&#x200B;**文档**&#x200B;选项卡包含概念注释和[调查行动手册](investigation-playbook.md)。

## 本指南面向谁？

- **站点操作员和站点可靠性工程(SRE)**&#x200B;正在调查CDN带宽过量、流量尖峰或源负载。
- **开发人员**&#x200B;调整全页缓存(FPC)覆盖率和命中率，或实施快速清漆配置语言(VCL)规则。
- **管理员和安全工程师**&#x200B;识别并缓解不需要的机器人、刮刀和恶意的自动化流量。

假定您熟悉[!DNL Adobe Commerce on Cloud Infrastructure]、Fastly CDN概念以及基本的New Relic导航。

## 工作原理

从页面顶部的平台控件中选择帐户和时间范围。 可选的&#x200B;**项目ID**&#x200B;可以将图表进一步缩小到特定云项目。 在主帐户或伙伴关系设置中，能够在下拉列表中查看帐户并不意味着您可以查询该帐户。 如果图表报告权限错误，请切换到您拥有New Relic查询语言(NRQL)访问权限的帐户。

您可以继续应用过滤器，将广泛的概述转变为重点调查。 单击Facet列中的值（如机器人、IP、子网、国家或内容类型）以添加[全局筛选器](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use)。 活动筛选器显示在网格顶部，并同时应用于每个选项卡中的每个小组件。 要扩大范围，请移除过滤器。

**演练** — 考虑以下情况：*总带宽*&#x200B;的趋势高于合同限额，您希望了解谁在推动它：

1. 打开&#x200B;**机器人活动和请求分析**&#x200B;选项卡并读取&#x200B;**带宽结构**，查看有多少流量是自动流量还是自然流量。
1. 如果机器人似乎具有更多流量，请打开&#x200B;**按带宽划分的已知机器人**，然后单击最重的命名机器人，例如刮刀。 这添加了一个新过滤器，这意味着现在每个小部件的范围都限制在该机器人上。
1. 阅读&#x200B;**已知机器人影响详细信息**&#x200B;以了解其请求率、状态组合和FPC命中率。
1. 若要查看机器人的来源位置，请检查&#x200B;**按国家/地区划分的带宽**。 要查看机器人正在获取的内容，请参阅&#x200B;**按带宽划分的URL**。
1. 如果流量集中在一个网络中，请单击&#x200B;**按IP子网统计**&#x200B;以确认操作者在一个块中跨地址旋转。
1. 现在，您掌握了撰写有针对性的缓解措施所需的人员、内容和地点。 继续查看[调查行动手册](investigation-playbook.md)，了解如何继续。

相同的过滤方法适用于任何起始方面：可疑国家/地区、单个IP、内容类型或URL路径区段。

## 如何测量数据

了解一些衡量标准选择有助于更轻松地信任和解读这些数字。

- **Bandwidth (BW)**&#x200B;是CDN为匹配请求提供服务的总字节数，计数&#x200B;**响应标头和正文**。 它是衡量合同拨备的总体成本指标。
- **请求（需要）** 是不同请求的数量，但是，如果启用了Fastly [屏蔽](https://www.fastly.com/documentation/guides/concepts/shielding/)，则单个请求将记录&#x200B;**两次**，每次以下记录一次：
  - 内部屏蔽[存在点(POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - EDGE POP
    除非响应直接来自本地POP缓存，或者屏蔽本身充当发件人位置的POP，否则会发生这种情况。 为避免重复计算这些`HIT,MISS`和`MISS,MISS`情况，应用查询在`request_id`字段中聚合为[`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount)。 这将返回一个接近&#x200B;**的近似值**，其预期误差为&#x200B;**~5%**，而不是精确计数。
- **CDN网络段**&#x200B;的压缩方式不同。 发送到客户端的响应已压缩，但shield-to-POP通信是[未压缩](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge)以保留[Edge Side Include (ESI)](https://www.fastly.com/documentation/reference/vcl/statements/esi/)支持。 因此，低缓存命中率比面向客户端的命中率会夸大内部区段，因为未缓存的内容必须以完整、未压缩的大小重复通过屏蔽提取。 此压缩是为什么&#x200B;**CDN网络段带宽**&#x200B;构件和FPC命中率是基础成本相同的两个视图。

## 数据限制和性能

- **30天保留** — 根据订阅计划，Fastly CDN日志在New Relic中保留&#x200B;**30天**。 您选择的任何窗口都必须位于过去30天内。 对于较长期的&#x200B;**总计**&#x200B;带宽，请使用[!DNL Adobe Commerce admin]面板中的直接Fastly集成，**仪表板> Fastly >带宽> Total**，但请考虑它报告每个服务ID，因此必须按环境收集数据并汇总以与合同允许量进行比较。
- **60秒的查询限制** — 每个图表的NRQL都有[60秒的执行限制](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration)。 对于流量非常高的帐户，小组件在扫描过多日志记录时可能会超时。 如果发生这种情况，请缩短时间范围并重新加载图表。 对于较轻的选项卡，可再次展开它。
