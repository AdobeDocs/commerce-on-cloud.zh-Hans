---
title: 登录到 [!DNL Cloud Console]
description: 了解Adobe Commerce on Cloud基础架构的 [!DNL Cloud Console] 。
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# 登录到[!DNL Cloud Console]

[!DNL Cloud Console]提供交互式方法来生成、管理和部署Commerce代码。 [!DNL Cloud Console]是一种更加新式、用户友好的体验，为将来的界面增强奠定了基础。

[登录 [!DNL Cloud Console]](https://console.adobecommerce.com)以查看您的项目列表。

![项目列表](../assets/ui-allprojects-list.png)

## 功能

新增或改进的功能包括：

- 项目和环境特征的清晰概述
- 具有可排序历史记录的活动流
- 入门级项目的手动备份管理和历史记录
- 增强的日志视图
- 可排序列表
- 添加集成的简单表单和指南
- Web内容无障碍准则(WCAG)合规性

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

新增功能或改进功能如下：

| 功能 | 改进功能 |
| -------------- | ----------------------------------- |
| [活动流](../cloud-guide/project/activity-stream.md) | 与正在运行、待处理或历史操作的可排序列表交互。 选择活动并查看日志或取消正在运行的生成。 |
| [项目和环境概述](../cloud-guide/project/overview.md#project-overview) | 打开您的项目，然后查看项目概述和环境列表。 环境概述提供了有关环境状态、应用程序访问权限和最近活动的更多详细信息。 |
| [集成表单](../cloud-guide/integrations/overview.md) | 使用简单的表单和指南来添加集成，例如比特桶或Slack通知。 |
| [项目列表](../cloud-guide/project/overview.md#cloud-console) | _所有项目_&#x200B;视图列出您有权访问的所有项目。 您可以单击&#x200B;**[!UICONTROL Show filters]**&#x200B;并按类型、地区或计划筛选项目列表。 |
| [变量可见性选项](../cloud-guide/environment/variable-levels.md) | 在生成或运行时限制项目级别或环境级别变量的可见性。 |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## 控制台问题

**_在哪里可以找到快照功能_**？

对于[!DNL Starter]项目，快照功能现在称为&#x200B;_备份_。 您可以从[!DNL Cloud Console]创建[!DNL Starter]环境的手动备份，或从Cloud CLI创建快照。 您必须具有环境的管理员角色。

从项目导航栏中选择一个环境。 环境必须处于活动状态。 选择&#x200B;**[!UICONTROL Backups]**&#x200B;选项卡。 目前，此选项不适用于Pro环境。

**_为环境_**&#x200B;配置的路由列表位于何处？

您可以在&#x200B;_服务_&#x200B;选项卡上找到某个环境的已配置路由列表。

从项目导航栏中选择一个环境。 选择&#x200B;**[!UICONTROL Services]**&#x200B;选项卡。 **路由器**&#x200B;概述显示已配置的路由。 目前，您无法从新[!DNL Cloud Console]添加路由。

## 帐户菜单

右上角是您的帐户菜单。 单击菜单的向下箭头并选择&#x200B;**[!UICONTROL My Profile]**。 在&#x200B;_我的个人资料_&#x200B;视图中，您可以控制用户详细信息和显示设置，管理[安全身份验证](../cloud-guide/project/user-access.md#user-authentication-requirements)、[API令牌](../cloud-guide/project/user-access.md#create-an-api-token)和[SSH密钥](../cloud-guide/development/secure-connections.md)。
