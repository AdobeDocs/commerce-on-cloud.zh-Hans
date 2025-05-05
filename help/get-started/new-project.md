---
title: 在Cloud上配置Commerce
description: 了解如何准备Adobe客户技术顾问以在云基础架构项目上配置您的Adobe Commerce。
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Commerce on Cloud配置先决条件

让我们开始并初始化您在云基础架构上的Commerce项目！

在Adobe上预配Commerce项目环境之前，建议您考虑以下策略并准备答案，以首次咨询Adobe客户团队。 将以下部分用作核对清单，帮助您准备好与客户技术顾问进行对话以配置云项目：

## 域定义

**问题1**：_您打算使用哪些域或域进行站点启动？_

通过为Pro暂存和生产环境定义顶级域和子域，准备集成Fastly和nginx服务。 在初始设置后，您只能通过提交Adobe Commerce支持票证来添加域，因此建议您准备好域信息。

生产和暂存域示例：

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

有关多个域或唯一域的进一步指导，请参阅&#x200B;_Commerce on Cloud Infrastructure_&#x200B;指南中的[设置多个网站或商店](../cloud-guide/store/multiple-sites.md)。

如果您现有的Fastly帐户链接了您Adobe Commerce网站上使用的相同顶点和子域，请参阅[多个Fastly帐户和分配的域](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"}。

## 事务性电子邮件域

**问题2**：_您打算使用哪些域或域来发送事务性电子邮件？_

Adobe Commerce on Cloud使用SendGrid简单邮件传输协议(SMTP)代理服务，该服务提供出站电子邮件身份验证和信誉监控服务。 SendGrid代表您发送事务性电子邮件，因此需要域信息。

SendGrid域示例： `example@your-store.com`

有关事务性电子邮件和域设置的进一步指导，请参阅&#x200B;_Commerce on Cloud Infrastructure_&#x200B;指南中的[SendGrid邮件服务](../cloud-guide/project/sendgrid.md)。

## 存储分配

**问题3**： _您计划为文件上传和数据库分配多少合同存储空间？_

云基础架构上的Adobe Commerce使用存储来构建文件结构、搜索索引和数据库。 您可以根据需要从每个分区的10 GB开始细分存储。

您为云项目合同的存储量表示每个环境的总存储量。 例如，如果您购买了50 GB的存储空间，则每个环境有50 GB的存储空间。 生产、暂存和每个集成环境分别有50 GB的单独存储。

您可以随时增加合同存储空间。 对于Pro生产和暂存环境，您必须提交Adobe Commerce支持票证以更改磁盘空间分配。 Pro生产和暂存环境的大小只能按特定的时间间隔增加。 根据您当前的磁盘空间使用情况，支持团队可能建议将磁盘空间分配至少增加10 GB。 分配后，Pro Staging和Production的存储增加无法&#x200B;**还原**。

请参阅&#x200B;_云基础架构上的Commerce_&#x200B;指南中的[管理磁盘空间](../cloud-guide/storage/manage-disk-space.md)。

## 云服务区域

**问题4**：_哪个云服务区域最适合您的邻近地区？_

选择Amazon Web Services (AWS)或Microsoft Azure作为您的Adobe Commerce on cloud infrastructure Pro项目上的基础架构即服务(IaaS)基础。 每个服务提供商都在多个区域运营，并提供多个可用区。 选择适合您所在位置的区域，降低延迟和成本提升的可能性。

查看[Adobe Commerce云区域地图](../cloud-guide/overview.md)。

## 连接服务

**问题5**：_是否需要PrivateLink服务？ 如果是，PrivateLink连接位于哪个区域？_

云基础架构上的Adobe Commerce支持与AWS PrivateLink或Azure Private Link服务的集成。 尽管此服务是可选的，但是PrivateLink用于在云基础架构环境与托管在外部系统上的服务和应用程序之间建立安全的私有通信。

请务必考虑您的云服务策略(AWS或Azure)，以便Adobe Commerce实例可在同一区域内访问。 有关区域可访问性的进一步说明，请参阅&#x200B;_Commerce on Cloud Infrastructure_&#x200B;指南中的[PrivateLink服务](../cloud-guide/development/privatelink-service.md)。

## Target站点启动

**问题6**：_您预计的目标启动日期是多久？_

启动站点需要反复进行配置和测试，以确保站点启动成功。 设置目标日期可帮助您和您的Adobe客户团队为启动前的最终活动做好准备，其中包括用于协调最终步骤的调用。

请参阅&#x200B;_云基础架构上的Commerce_&#x200B;指南中的[启动站点概述](../cloud-guide/launch/overview.md)，以查看完整过程并下载启动清单副本。

>[!TIP]
>
> 快速查看云门户并访问您的新云项目。
>
>**下一步**：[加入Commerce](onboarding.md)
