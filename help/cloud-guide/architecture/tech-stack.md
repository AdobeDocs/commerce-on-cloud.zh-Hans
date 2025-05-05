---
title: 技术栈栈
description: 请参阅在云基础架构上形成Commerce的技术栈栈。
feature: Cloud, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# 技术栈栈

将云基础架构上的Adobe Commerce视为五个功能层，如下所示：

![云栈叠](../../assets/CloudStack.svg)

1. [**云基础架构**](pro-architecture.md)：选择Amazon Web Services (AWS)或Microsoft Azure作为您的Adobe Commerce on cloud infrastructure Pro项目的基础架构(IaaS)。

   Adobe会定期分析您的虚拟计算资源(vCPU)使用情况，并自动分配资源以优化长期使用情况，并降低超出最大年度vCPU日允许量的风险。 如果预计特定时间段的网站流量会增加，则必须继续打开支持票证以[请求临时扩展](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=zh-Hans)。

1. [**Platform as a Service**](cloud-architecture.md)：云基础架构项目上的每个Adobe Commerce都提供了一个用于开发、测试和集成服务的Platform as a Service (PaaS)集成环境。
1. [**Adobe Commerce**](../project/overview.md)：云基础架构上的Adobe Commerce提供了预配置的基础架构，其中包括PHP、MySQL (MariaDB)、Redis、[!DNL RabbitMQ]和支持的搜索引擎技术。
1. [**性能工具**](../monitor/new-relic-service.md)： New Relic性能工具允许您通过收集、分析和显示来自Adobe Commerce的关于云基础架构项目的数据，来调试、监视和管理应用程序和基础架构。
1. [**内容交付网络(CDN)、Web应用程序防火墙([!DNL WAF])和图像优化(IO)**](../cdn/fastly.md)：

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) — 提供安全CDN服务，该服务具有内置保护，可抵御分布式拒绝服务(DDoS)攻击（如[!DNL Ping of Death]、[!DNL Smurf]攻击）和其他基于Internet控制消息协议(ICMP)的洪水攻击。
   * [Web应用程序防火墙(WAF)](../cdn/fastly-waf-service.md) — WAF服务确保生产环境和WAF策略中Adobe Commerce店面的PCI合规性，这些策略可保护Adobe Commerce Web应用程序免受注入攻击、恶意输入、跨站点脚本、数据过滤、HTTP协议违规和其他[[!DNL OWASP] 十大安全威胁](https://owasp.org/www-project-top-ten/)。
   * [图像优化(IO)](../cdn/fastly-image-optimization.md) — 提供实时图像处理和优化，以加快图像交付并简化响应式Web应用程序的图像源集的维护。 Fastly IO减轻了图像处理和调整负载大小的负担，使服务器能够高效地处理订单和转换。

单一应用程序需要大量资源，难以快速扩展和提供服务。 借助云基础架构，Commerce客户可获得对基于SaaS的丰富、智能和高性能微服务的无与伦比的访问权限。 请参阅[支持的软件和服务](cloud-architecture.md#supported-software-and-services)。

使用[Commerce入门指南](../../get-started/overview.md)设置新的Cloud项目并开始在云原生环境中管理您的[!DNL Commerce]应用程序。
