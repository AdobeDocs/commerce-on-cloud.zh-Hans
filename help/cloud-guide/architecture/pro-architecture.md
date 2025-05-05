---
title: 专业体系结构
description: 了解Pro架构支持的环境。
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 0%

---

# 专业体系结构

您的Adobe Commerce on cloud infrastructure Pro架构支持多种环境，您可以使用这些环境开发、测试和启动您的商店。

- **Master** — 提供部署到Platform as a Service (PaaS)容器的`master`分支。
- **集成** — 提供单个`integration`分支用于开发，但您可以创建一个其他分支。 这允许将最多两个&#x200B;_活动_&#x200B;分支部署到Platform as a Service (PaaS)容器。
- **暂存** — 提供部署到专用基础架构即服务(IaaS)容器的单个`staging`分支。
- **生产** — 提供部署到专用基础架构即服务(IaaS)容器的单个`production`分支。

下表总结了环境之间的差异：

|                                        | 集成 | 暂存 | 生产 |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| 支持[!DNL Cloud Console]中的设置管理 | 是 | 有限 | 有限 |
| 支持多个分支 | 是 | 否（仅限暂存） | 否（仅限生产） |
| 使用YAML文件进行配置 | 是 | 否 | 否 |
| 在专用IaaS硬件上运行 | 否 | 是 | 是 |
| 包括Fastly CDN | 否 | 是 | 是 |
| 包括New Relic服务 | 否 | APM | APM + NRI |
| 自动备份 | 否 | 是 | 是 |

>[!NOTE]
>
>Adobe提供了Cloud Docker for Commerce工具，可用于部署到本地Cloud Docker环境，以便您能够开发和测试Adobe Commerce项目。 请参阅[Docker开发](../dev-tools/cloud-docker.md)。

## 环境架构

您的项目是一个包含三个主要环境分支的Git存储库： `integration`、`staging`和`production`。 下图显示了Pro环境的层次关系：

![专业环境体系结构的高级视图](../../assets/pro-branch-architecture.png)

### 主环境

在Pro项目中，`master`分支会为生产环境提供一个活动的PaaS环境。 始终将生产代码的副本推送到`master`环境，以便您可以在不中断服务的情况下调试生产环境。

**注意事项：**

- 请&#x200B;**不**&#x200B;根据`master`分支创建分支。 使用集成环境创建用于开发的活动分支。

- 请勿使用`master`环境进行开发、UAT或性能测试

### 集成环境

集成环境在Linux容器(LXC)中运行，该容器位于称为PaaS的服务器网格上。 每个环境都包含一个Web服务器和数据库，用于测试您的站点。 有关AWS和Azure IP地址的列表，请参阅[区域IP地址](../project/regional-ip-addresses.md)。

**推荐的用例：**

集成环境是为在将更改移动到暂存和生产环境之前进行有限的测试和开发而设计的。 例如，您可以使用集成环境完成以下任务：

- 确保对持续集成(CI)流程所做的更改与云兼容

- 在主页、类别、产品详细信息页面(PDP)、结账和管理员等关键页面上测试关键工作流

要在集成环境中获得最佳性能，请遵循以下最佳实践：

- 限制目录大小 — 作为参考，示例数据包含约2,048个产品。 尝试将目录大小缩减到4,000-5,000个产品左右。
要检查目录中的产品数，请运行以下MySQL查询：

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- 减少客户组的数量 — 拥有过多的客户组可能会影响索引性能和整体性能。

- 仅限使用一位或两位并发用户

- 禁用cron作业并根据需要手动运行

**注意事项：**

- 在集成环境中无法访问Fastly CDN和New Relic服务

- 集成环境体系结构与暂存和生产体系结构不匹配

- 请勿使用`integration`环境进行开发测试、性能测试或用户验收测试(UAT)

- 请勿使用`integration`环境来测试B2B的Adobe Commerce功能

- 无法从数据库生产或暂存还原集成环境中的数据库

{{enhanced-integration-envs}}

### 暂存环境

暂存环境提供了一个用于测试站点的准生产环境。 此环境托管在专用的IaaS硬件上，包括所有服务，如Fastly CDN、New Relic APM和搜索。

**推荐的用例：**

该环境与生产架构匹配，设计用于UAT、内容暂存和最终审阅，然后再将功能推送到`production`环境。 例如，您可以使用`staging`环境完成以下任务：

- 针对生产数据的回归测试

- 启用了Fastly缓存的性能测试

- 测试新内部版本，而不是在生产环境中打补丁

- 新内部版本的UAT测试

- 测试Adobe Commerce的B2B

- 自定义cron配置和测试cron作业

请参阅[部署工作流](pro-develop-deploy-workflow.md#deployment-workflow)和[测试部署](../test/staging-and-production.md)。

**注意事项：**

- 启动生产站点后，请使用暂存环境主要测试用于生产关键错误修复的修补程序。

- 无法从`staging`分支创建分支。 而是将代码更改从`integration`分支推送到`staging`分支。

{{second-staging}}

### 生产环境

生产环境运行面向公众的单个和多站点店面。 此环境在专用IaaS硬件上运行，具有冗余、高可用性节点，可为客户提供持续访问和故障转移保护。 生产环境包括暂存环境中的所有服务，以及[New Relic基础架构(NRI)](../monitor/new-relic-service.md#new-relic-infrastructure)服务，该服务会自动与应用程序数据和性能分析连接，以提供动态服务器监控。

**注意事项：**

无法从`production`分支创建分支。 而是将代码更改从`staging`分支推送到`production`分支。

### 生产技术栈栈

生产环境在由HAProxy管理的每个虚拟机的Elastic Load Balancer后面有三个虚拟机(VM)。 每个虚拟机都包括以下技术：

- **Fastly CDN**—HTTP缓存和CDN

- **NGINX** — 使用PHP-FPM的Web服务器，一个具有多个工作线程的实例

- **GlusterFS** — 用于管理所有静态文件部署并通过四个目录装载进行同步的文件服务器：

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis** — 每个虚拟机一个服务器，只有一个处于活动状态，另外两个作为副本

- **Elasticsearch** — 在Cloud Infrastructure 2.2到2.4.3-p2上搜索Adobe Commerce

- **OpenSearch** — 在云基础架构2.3.7-p3、2.4.3-p2、2.4.4及更高版本上搜索Adobe Commerce

- **Galera** — 数据库群集，每个节点有一个MariaDB MySQL数据库，每个数据库的唯一ID的自动增量设置为3

下图显示了生产环境中使用的技术：

![生产技术栈栈](../../assets/az-stack-diagram.png)

## 冗余硬件

云基础架构上的Adobe Commerce运行的是&#x200B;_冗余架构_，其中所有三个实例都接受读取和写入，而不是运行传统的、主动 — 被动`master`或主 — 辅助设置。 此架构在扩展时提供零停机时间，并确保事务完整性。

由于独特的冗余硬件，Adobe可以提供三台网关服务器。 大多数外部服务都支持向允许列表添加多个IP地址，因此拥有多个固定IP地址不成问题。 三个网关映射到生产环境群集中的三个服务器，并保留静态IP地址。 它完全冗余，并且在每个级别都具有高可用性：

- DNS
- 内容交付网络(CDN)
- 弹性负载平衡器(ELB)
- 包含所有Adobe Commerce服务（包括数据库和Web服务器）的三服务器群集

## 备份和灾难恢复

云基础架构上的Adobe Commerce使用高可用性体系结构，该体系结构在三个单独的AWS或Azure可用区上复制每个Pro项目，每个区都有一个单独的数据中心。 除此冗余外，专业级暂存和生产环境还会接收常规实时备份，这些备份专为在&#x200B;_灾难性故障_&#x200B;的情况下使用而设计。

**自动备份**&#x200B;包含来自所有正在运行的服务（如MySQL数据库和装载的卷上存储的文件）的永久数据。 备份将保存到与生产环境位于同一区域的加密弹性块存储(EBS)中。 不能公开访问自动备份，因为它们存储在单独的系统中。

>[!NOTE]
>
>装入的卷仅包含/引用[可写装入](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/configure/app/properties/properties#mounts)，将不包含所有`app/`目录。 至于其他文件，它们由[生成和部署过程](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)创建/生成，您还必须检查Git存储库中是否有剩余文件。

{{pro-backups}}

您可以使用CLI命令为暂存环境和生产环境创建数据库的&#x200B;**手动备份**。 请参阅[备份数据库](../storage/database-dump.md)。 对于`integration`环境，Adobe建议在访问云基础架构项目上的Adobe Commerce之后和应用任何重大更改之前，首先创建备份。 请参阅[备份管理](../storage/snapshots.md)。

### 恢复点目标

请联系您的Adobe客户成功经理，以了解有关恢复点目标上次备份时间的详细信息。 备份的频率取决于计划的备份计划以及写入存储服务的更改量。

### 保留策略

Adobe根据以下数据保留策略保留自动备份：

| 时间段 | 备份保留策略 |
| ------------------ | ----------------------- |
| 第1天至第3天 | 每小时一次备份 |
| 第4天至第7天 | 每天备份一次 |
| 第2周至第6周 | 每周一次备份 |
| 第8周至第12周 | 双周备份 |
| 第3个月至第5个月 | 每月一次备份 |

此策略可能因您的云基础架构计划而异。

### 恢复时间目标

RTO取决于存储的大小。 大型EBS卷需要更多时间来恢复。 恢复时间可能因数据库的大小而异。 请联系您的Adobe客户成功经理以了解详细信息。

## 专业群集扩展

Pro群集大小和&#x200B;_计算_&#x200B;配置因所选的云提供商(AWS、Azure)、地区和服务依赖项而异。 Adobe云基础架构可以扩展Pro群集，以随着需求的变化满足流量期望和服务要求。

冗余体系结构使Adobe云基础架构能够在不停机的情况下进行扩展。 在升级时，这三个实例中的每一个都会轮换以升级容量，而不会影响站点操作。 例如，如果约束位于PHP级别而不是数据库级别，则可以将额外的Web服务器添加到现有群集。 这提供了&#x200B;_水平缩放_，以补充数据库级别上额外CPU提供的垂直缩放。 请参阅[缩放的体系结构](scaled-architecture.md)。

如果您预计某个事件或其他原因会导致流量显着增加，则可以请求临时增加容量。 请参阅[如何在&#x200B;_Commerce帮助中心_&#x200B;中请求临时扩展](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=zh-Hans)。
