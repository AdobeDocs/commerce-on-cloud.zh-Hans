---
title: 将请求重新路由到CMS后端
description: 了解如何使用Fastly Edge模块将来自Adobe Commerce存储区的传入请求重新路由到单独的WordPress网站。
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
TQID: https://experienceleague.adobe.com/zRM-iTFGNPgSmT5xu1B9Lo3-onUtCHh-tVY-WPPiVC8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 322
ht-degree: 0%

---

# 将请求重新路由到CMS后端

使用Fastly Edge Module _Other CMS/后端集成_&#x200B;和Edge Dictionary将来自Adobe Commerce存储区的传入请求重新路由到单独的WordPress站点。 您可以按照类似的流程，将请求重新路由到其他CMS后端。

使用Fastly Edge模块从管理员创建和上传自定义VCL代码，而不是手动编写VCL代码并使用Fastly API上传它。

>[!NOTE]
>
>我们建议将自定义VCL配置添加到暂存环境中，您可以在其中测试它们，然后再在生产环境中更新Fastly服务配置。

**先决条件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**要将请求从Adobe Commerce重新路由到WordPress**：

1. 在暂存或生产环境中启用Fastly Edge模块。

   - 登录到管理员。

   - 导航到&#x200B;**存储** >设置> **配置** > **高级** > **系统** > **全页缓存** > **快速配置** > **高级配置**。

   - 将&#x200B;**Fastly Edge模块**&#x200B;的值设置为&#x200B;**是**。

   - 保存配置。

1. 标识要重路由到WordPress后端的URL路径。

1. 完成以下任务以配置Fastly服务并创建自定义VCL代码，以便将请求重新路由到WordPress后端。

   - 创建一个Edge词典，该词典指定要从Adobe Commerce存储区重新路由到后端的路径。

   - 将WordPress后端添加到Fastly服务配置并附加URL重写的请求条件。

   - 配置&#x200B;_其他CMS/后端集成_ Edge模块，以便处理从Adobe Commerce到WordPress后端的URL重写。

     有关详细说明，请参阅Magento 2 _的_ Fastly CDN模块文档中的[Fastly Edge模块 — 其他CMS/后端集成](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md)。

1. 更新Fastly服务配置后，请测试您的Adobe Commerce存储，以确保为WordPress指定的URL请求正确重路由。

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
