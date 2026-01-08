---
title: 云基础架构上的Commerce
description: 了解如何在云基础架构上构建、部署与管理 Commerce。
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
source-git-commit: 8cbda8ca194c5e5865073c9eb08e061cfecb5ace
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---


# 云基础架构上的Commerce

云基础架构上的Adobe Commerce提供了一个自动托管平台，该平台提供了一种用于在云原生环境中构建、部署和管理&#x200B;**应用程序的**&#x200B;自助服务[!DNL Commerce]方法。 云基础架构上的Adobe Commerce提供了其他功能，使其与内部部署Adobe Commerce和Magento Open Source平台有所不同：

- 预配置的基础结构，包括PHP、MySQL (MariaDB)、Redis、消息队列服务（[!DNL RabbitMQ]或[!DNL ActiveMQ]）以及支持的搜索引擎技术。
- 基于Git的工作流，具有自动构建和部署功能，可在您每次推送Platform as a Service (PaaS)环境中的代码更改时实现高效的快速开发和连续部署。
- 高度可定制的环境配置文件和命令行界面(CLI)管理和部署工具。
- Amazon Web Services (AWS)托管，为在线销售和零售提供可扩展且安全的环境。

![云优势](../assets/CloudBenefits.svg)

>[!NOTE]
>
>有关安全性的更多信息，请参阅[安全性启动项核对清单](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration)。

查看[技术栈栈](architecture/tech-stack.md)的详细信息，或了解有关Commerce [的](architecture/cloud-architecture.md)云架构中的特定功能和支持的产品的更多信息。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## 云区域

以下部分提供了在云基础架构上可用于Adobe Commerce的各种AWS和Azure区域的详细信息。

## AWS地区

![显示AWS地区的图表](../assets/aws-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 仅在中国和俄罗斯进行内部部署。

## Azure区域

显示Azure区域的![图表](../assets/azure-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 仅在中国和俄罗斯进行内部部署。 所有需要集成环境的商家都必须使用美国地区。

## Adobe Commerce文档

Commerce on cloud infrastructure指南假定您对Adobe Commerce应用程序有一定的工作知识和了解。 您可以参阅下面的[!DNL Commerce]开发人员和用户指南：

- [Adobe Commerce开发人员文档](https://developer.adobe.com/commerce/docs/) (Adobe Developer站点) — 开发、自定义、集成、扩展和使用高级功能

- [Adobe Commerce文档](https://experienceleague.adobe.com/docs/commerce.html?lang=zh-Hans) (Adobe Experience League) — 规划、实施、运营、升级和维护您的[!DNL Commerce]项目

{{$include /help/_includes/templated/whats-new.md}}


<!-- Last updated from includes: 2026-01-05 17:03:22 -->
