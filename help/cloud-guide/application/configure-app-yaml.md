---
title: 配置应用程序部署
description: 了解如何在应用程序配置文件中配置属性，这些属性控制 [!DNL Commerce] 应用程序构建和部署到云环境的方式。
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 190
ht-degree: 0%

---

# 配置应用程序部署

`.magento.app.yaml`文件控制应用程序构建和部署的方式。 虽然云基础架构上的Adobe Commerce支持每个项目使用多个应用程序，但通常一个项目具有一个应用程序，其中存储库根目录具有`.magento.app.yaml`文件。

`.magento.app.yaml`有许多默认值，请参阅[示例`.magento.app.yaml`文件](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml)。 始终查看已安装版本的`.magento.app.yaml`。 在云基础架构版本上，此文件可能会在Adobe Commerce中有所不同。

使用`.magento.app.yaml`文件定义以下配置值：

- [属性](properties.md) — 定义应用程序实例的属性值。
- [变量属性](variables-property.md) — 查看[!DNL Commerce]应用程序版本所需的环境变量。
- [PHP设置](php-settings.md) — 配置运行时PHP选项。
- [为静态文件设置缓存](set-cache.md) — 为媒体和静态文件设置缓存TTL。

>[!NOTE]
>
>`.magento.app.yaml`文件在本地或Git存储库中进行管理。 仅出于部署和构建过程的目的读取配置，并在部署完成后删除配置，因此您无法在服务器上找到它。

