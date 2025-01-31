---
title: 云基础架构项目
description: 阅读有关Adobe Commerce on cloud infrastructure [!DNL Cloud Console] 的概述，并了解如何访问帐户设置。
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---

# 云基础架构项目

云基础架构上的Adobe Commerce项目包括Git分支中的所有代码、关联的环境和用于部署[!DNL Commerce]应用程序的脚本。 环境包含支持[!DNL Commerce]应用程序的服务，包括数据库、Web服务器和缓存服务器。

Adobe提供了[!DNL Cloud Console]和开发人员工具来全面管理项目的各个方面。 作为帐户所有者，您对所有环境具有完全访问权限。

## [!DNL Cloud Console]

[!DNL Cloud Console]提供了交互方法，用于以用户友好的格式生成、管理和部署Commerce代码。 [登录 [!DNL Cloud Console]](https://console.adobecommerce.com)以查看您的项目列表。 您只能查看您有权以管理员身份访问或访问特定环境类型的项目。 如果您是Adobe解决方案合作伙伴，则可能会看到您支持的客户有多个项目。

>[!TIP]
>
>如果您没有看到任何项目，则必须联系与项目关联的[帐户所有者或项目管理员](../project/user-access.md)并请求访问权限。 对于首次登录的用户，请参阅&#x200B;_入门_&#x200B;指南中的[入门主题](../../get-started/onboarding.md#cloud-console)。

_所有项目_&#x200B;视图列出您有权访问的所有项目。 您可以单击&#x200B;**[!UICONTROL Show filters]**&#x200B;并按类型、地区或计划筛选项目列表。

![项目列表](../../assets/ui-allprojects-list.png)

### 项目概述

从&#x200B;_所有项目_&#x200B;列表中选择一个项目将打开项目概述。 项目概述始终显示项目导航栏，其中包括环境选择器和配置按钮：

![项目导航](../../assets/project-nav.png)

项目概述（只要未选择环境）在预览区域显示项目详细信息的摘要：

- 项目名称
- 地区、项目ID
- 规划、分配的存储、环境、用户
- 带&#x200B;**[!UICONTROL Set a custom domain]**&#x200B;按钮的店面URL

在主项目概述中：

- 环境视图显示![活动分支](../../assets/icon-active.png){width="32"} (active) and ![inactive branch](../../assets/icon-inactive.png){width="32"} （非活动）环境的列表或树视图。
- [活动流](activity-stream.md)显示项目的正在运行、挂起和最近活动。
<!-- - Apps & Services—Shows a topology of service containers -->

对于&#x200B;**Starter**&#x200B;项目，存在从`master` （生产）开始的分支层次结构。 您创建的任何分支都会显示为来自`master`分支的子级。 Adobe建议创建一个`staging`分支，然后创建一个`integration`分支用于开发。 请参阅[入门体系结构](../architecture/starter-architecture.md)。

对于&#x200B;**Pro**，存在从`production`到`staging`到`integration`的分支层次结构。 ![专用图标](../../assets/icon-dedicated.png){width="32"}图标表示分支部署到专用环境。 您创建的任何分支都会显示为`integration`分支的子级。 请参阅[专业体系结构](../architecture/pro-architecture.md)。

![专业环境列表](../../assets/pro-environments.png)

### 环境概述

从项目导航栏中选择环境会将概述和导航栏更改为专注于选定的环境。 导航栏包括分支控件（“分支”、“合并”和“同步”）和一个配置按钮：

![选定的环境](../../assets/environment-selected.png)

环境概述在预览区域显示环境详细信息的摘要：

- 环境名称，类型
- 地区、项目ID
- 上次活动的日期和时间，包括备份
- HTTP访问和搜索引擎状态
- 分配给环境的计算机名称
- 环境状态（活动或非活动）
- 带&#x200B;**[!UICONTROL Set a custom domain]**&#x200B;按钮的店面URL

在主环境概述中：

- [活动流](activity-stream.md)构成了主环境概述，并显示选定环境的正在运行、挂起和最近活动。
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- [备份选项卡](../storage/snapshots.md#create-a-manual-backup)提供了存储备份的列表、备份操作的历史记录以及“备份”按钮。

### 访问店面

每个活动环境都有一个店面。 从顶部导航中选择一个环境，然后单击环境概述中的URL。 此外，活动列表上方的右侧还有&#x200B;**[!UICONTROL URLs]**&#x200B;列表。

Web访问URL可能包括以下内容：

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **唯一ID** = 7个随机字母数字字符
- **项目ID** = 13个字符的项目ID
- **区域** = AWS或Azure区域名称，请参阅[区域IP地址](regional-ip-addresses.md)

Pro生产和暂存环境包括三个节点，您可以使用以下链接访问它们：

- 负载平衡器URL：

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- 直接访问以下三个冗余服务器之一：

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  生产URL由内容交付网络(CDN)使用。

## 设置

单击项目导航右侧的![配置项目图标](../../assets/icon-configure.png){width="36"}（配置）图标，打开&#x200B;_设置_&#x200B;面板。

### 项目设置

**[!UICONTROL Project Settings]**&#x200B;扩展了项目级控件菜单以管理用户、变量等：

| 选项 | 描述 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| 常规 | 管理用于计划备份或维护的时区。 |
| 访问 | 管理[用户对项目和环境类型的访问权限](user-access.md)。 |
| 证书 | 查看与项目关联的SSL证书的列表。 |
| 部署密钥 | 添加并查看项目代码存储库的公共密钥。 |
| 域 | 向项目添加域名。 请参阅[管理域](../cdn/fastly-custom-cache-configuration.md#manage-domains)。 |
| 集成 | 添加和管理[集成](../integrations/overview.md)，如运行状况通知和Webhook。 |
| 变量 | 添加在所有环境中生成和运行时可用的[项目级变量](../environment/variable-levels.md)。 |

{style="table-layout:auto"}

### 环境设置

单击&#x200B;**[!UICONTROL Environments]**&#x200B;并从列表中选择特定环境，以便管理站点设置、环境变量等：

| 选项 | 描述 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 常规 | 配置显示名称、环境类型和父环境。<br>切换不同的环境设置： |
|           | **启用传出电子邮件**：使用SMTP协议从环境发送[传出电子邮件](outgoing-emails.md)。 |
|           | **对搜索引擎隐藏**：网站中的块搜索引擎索引器和爬网程序。 |
|           | **HTTP访问控制**：使用登录和IP地址访问控制为[!DNL Cloud Console]启用安全配置。 |
|           | 状态为`active`或`inactive`。 您的大多数工作都是在活动环境中进行的。 您可以停用或删除环境。 |
| 变量 | 查看、创建和管理运行时可用的[环境级变量](../environment/variable-levels.md)。 |
| 域 | 查看[已配置路由](../routes/routes-yaml.md)的列表。 |

{style="table-layout:auto"}

>[!WARNING]
>
>**不要**&#x200B;使用HTTP访问控制方法来保护Pro暂存环境和生产环境。 这破坏了快速缓存。 请改用Adobe Commerce的Fastly CDN中提供的[阻止](../cdn/fastly-vcl-blocking.md)功能。

## Fastly和New Relic凭据

您的项目包括[Fastly](../cdn/fastly.md)和[New Relic](../monitor/new-relic-service.md)。 项目详细信息显示项目计划的信息以及这些集成的重要许可证和令牌。 只有许可证所有者才具有对凭据和服务的初始访问权限。 根据需要向技术和开发人员资源提供这些凭据。

- [Fastly](https://www.fastly.com/)为云基础架构项目上的Adobe Commerce提供内容交付(CDN)、图像优化和安全服务(DDoS和WAF)。 查看[获取Fastly凭据](../cdn/fastly-configuration.md#get-fastly-credentials)。

- [New Relic](../monitor/new-relic-service.md)为暂存环境和生产环境提供应用程序指标和性能信息。

使用[Cloud CLI](../dev-tools/cloud-cli-overview.md)查看您的集成令牌、ID等：

```bash
magento-cloud subscription:info services
```
