---
title: 启动步骤
description: 了解如何完成站点启动。
exl-id: e7a3cd6b-32de-4fd0-9fbd-da8299e77114
TQID: https://experienceleague.adobe.com/Nl8YFJUZUkxtsm0eqBgJklsTysKuUE31PkEDsJmvBMU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 0%

---

# 启动步骤

测试并完成启动项核对清单后，即可开始启动项的最后步骤。 这些步骤包括输入票证、传输站点访问权限，以及最后在商店上线时对其进行测试。

Adobe支持人员将与您一起完成该过程，检查状态，并在出现任何问题时提供帮助。 应使用票证跟踪所有问题，以便最好地捕获所发生的情况及其解决方式。 在将更新部署到启动的存储区后，如果开始不断迭代，则可能会再次出现类似问题。 这些工单有助于查明问题并帮助调整部署任务。

## 联系Adobe以启动您的网站

联系Adobe Commerce支持部门并更新任何站点启动（上线）票证，指定其交换和启动商店的日期和时间。

## 将DNS切换到新站点

生存时间已更改值对于检查已更改的域很重要。 当您修改A和CNAME记录时，更新会花费TTL配置的时间正确更新。 有关DNS设置的详细信息，请参阅[DNS配置](checklist.md#update-dns-configuration-with-production-settings)。

### 要更改为新站点，请执行以下操作：

1. 访问您的DNS服务。

1. 更新域和主机名的A和CNAME记录。

1. 等待TTL时间传递并重新启动Web浏览器。

1. 使用店面域访问您的商店。

## 测试实时商店

在实时商店中完成几个UAT测试，以确认所有内容都在加载且操作已正确完成。 有关测试的列表，请参阅[测试部署](../test/staging-and-production.md#complete-uat-testing)。

## 启动后

Adobe检查并监控站点，确保所有服务和访问均呈绿色显示。 我们将根据需要随时为您提供帮助，以逐步了解并检查所有系统日志、服务、缓存和功能是否都能根据您和客户的需要工作。

如果发生任何问题，请使用支持创建并跟踪问题。 包括尽可能多的信息，如日期/时间、存在问题的特定功能、症状和异常行为、扩展等。 我们会调查日志和问题，并与您合作以尽快解决。
