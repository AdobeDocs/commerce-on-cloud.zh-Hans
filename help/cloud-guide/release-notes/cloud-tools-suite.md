---
title: Cloud Tools Suite发行说明
description: 了解适用于Adobe Commerce的Cloud Tools套件的最新改进。
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: fa3c52baea5f1c0805c82de937e95f6e487f16be
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 1%

---

# Commerce Cloud Tools Suite发行说明

此发行信息详细介绍了Commerce软件包的Cloud Tools Suite的最新改进，这些改进旨在部署和管理Cloud平台上的Adobe Commerce安装和升级。

| 发行说明 | 版本 | 描述 | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [`ece-tools`包](ece-tools-package.md) | 2002.2.5 | 一组用于管理和部署云项目的脚本和工具 | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.4) |
| Commerce的[云修补程序](cloud-patches.md) | 1.1.7 | 一组修补程序，用于改进所有Adobe Commerce版本与Cloud环境的集成。 此软件包包含Adobe Commerce修补程序以及使用`ece-tools`进行部署时应用的可用修补程序 | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.6) |
| 适用于Commerce的[Cloud Docker](cloud-docker.md) | 1.4.2 | Docker映像将Adobe Commerce部署到本地云环境的功能和配置文件 | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.1) |
| Commerce的[云组件](cloud-components.md) | 1.1.1 | 为部署在云基础架构上的站点扩展了Adobe Commerce核心功能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.1) |

当您更新到ECE-Tools 2002.1.0或更高版本时，会自动更新到其他包的最新版本，这些包是`ece-tools`包的依赖项。 有关依赖项列表，请参阅[云中继](../development/overview.md#cloud-metapackage)。

新版本的`ece-tools` (2002.2.0)仅在PHP版本8.1及更高版本(8.2、8.3)中可用。 已弃用旧版PHP (7.4、7.3、7.2)。 您可以将`ece-tools`的早期版本与旧PHP版本一起使用。
