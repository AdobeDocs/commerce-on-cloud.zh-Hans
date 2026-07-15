---
title: Cloud Tools Suite发行说明
description: 了解适用于Adobe Commerce的Cloud Tools套件的最新改进。
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 8470b1b07adc4180f2ed1f07d3987bf33f9813ac
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Commerce Cloud Tools Suite发行说明

此发行信息详细介绍了Commerce软件包的Cloud Tools Suite的最新改进，这些改进旨在部署和管理Cloud平台上的Adobe Commerce安装和升级。

| 发行说明 | 版本 | 描述 | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ece-tools包](ece-tools-package.md) | 2002.2.11 | 一组用于管理和部署云项目的脚本和工具 | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| Commerce的[云修补程序](cloud-patches.md) | 1.1.15 | 一组修补程序，用于改进所有Adobe Commerce版本与Cloud环境的集成。 此软件包包含Adobe Commerce修补程序以及使用`ece-tools`进行部署时应用的可用修补程序 | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.15) |
| 适用于Commerce的[Cloud Docker](cloud-docker.md) | 1.4.8 | Docker映像将Adobe Commerce部署到本地云环境的功能和配置文件 | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| Commerce的[云组件](cloud-components.md) | 1.1.4 | 为部署在云基础架构上的站点扩展了Adobe Commerce核心功能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

当您更新到ECE-Tools 2002.1.0或更高版本时，会自动更新到其他包的最新版本，这些包是`ece-tools`包的依赖项。 有关依赖项列表，请参阅[云中继](../development/overview.md#cloud-metapackage)。
