---
title: 自动缩放
description: 了解云基础架构上的Adobe Commerce如何进行扩展以满足资源需求。
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 605
ht-degree: 0%

---

# 自动缩放

自动缩放功能可自动向云基础架构添加或删除资源，以保持最佳性能和合理的成本。 目前，此功能仅适用于配置了[缩放体系结构](scaled-architecture.md)的项目。

## Web服务器节点

[Web层](scaled-architecture.md#web-tier)可以扩展以适应进程请求的增加和更高的流量要求。 目前，自动缩放功能只能通过添加或删除Web服务器节点来水平缩放。

当CPU使用情况和流量达到预定义的阈值时，会发生自动缩放事件：

- 已添加&#x200B;**个节点** — 所有活动Web节点的CPU/核心在1分钟内都以75%的容量递增，流量在连续5分钟内增加20%。
- **节点已删除** — 所有活动Web节点上的CPU/核心以60%的加载速度加载20分钟。 节点会按照其添加顺序进行删除。

最小和最大阈值根据每个商户的合同资源限制确定和设置；这降低了无限扩展的风险。

## 使用New Relic监控阈值

您可以使用[New Relic服务](../monitor/new-relic-service.md)来监视某些阈值，如主机数和CPU使用情况。 以下New Relic查询对`cluster-id`使用变量表示法仅用于示例目的。

>[!TIP]
>
>有关生成查询的参考，请参阅&#x200B;_New Relic_&#x200B;文档中的[NRQL语法、子句和函数](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/)。
>使用您的查询构建[New Relic仪表板](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/)。

### 主机计数

以下示例New Relic查询显示环境中的主机计数：

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

在以下屏幕截图中，**看到的APM主机**&#x200B;是指在选定期间记录事务的主机数。

![New Relic主机计数](../../assets/new-relic/host-count.png)

### CPU使用情况

以下示例New Relic查询显示了CPU在Web节点的使用情况：

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic Web节点CPU使用情况](../../assets/new-relic/web-node-cpu-usage.png)

## 启用自动缩放

要在云基础架构项目上启用或禁用Adobe Commerce的自动缩放，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 在票证中选择以下原因：

- **联系原因**：基础架构更改请求
- **Adobe Commerce基础架构联系原因**：其他基础架构更改请求

>[!IMPORTANT]
>
>自动缩放功能会捕获意外事件。 即使您启用了自动缩放，如果您预计即将发生事件，Adobe仍建议您继续[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

### 负载测试

Adobe首先在云项目&#x200B;_暂存_&#x200B;群集上启用自动缩放。 在环境中执行并完成负载测试后，Adobe会在生产群集上启用自动缩放。 有关负载测试的指导，请参阅[性能测试](../launch/checklist.md#performance-testing)。

### IP允许列表

启用自动缩放后，出站Web节点流量源自服务节点的IP地址。 如果您使用的允许列表与云基础架构项目上的Adobe Commerce未捆绑的第三方服务一起使用，请验证第三方服务中的IP地址。

例如：

- 如果允许列表包含服务节点（1、2和3）的IP地址，则无需执行任何操作。
- 如果允许列表包含服务节点（1、2和3）和Web节点（4、5和6）的IP地址（本例中是全部六个节点），则无需执行任何操作。
- 如果允许列表仅包含Web节点（4、5和6）的IP地址&#x200B;_仅_，则必须更新以包含服务节点的IP地址。

