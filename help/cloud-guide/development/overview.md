---
title: 开发概述
description: 使用Adobe Commerce on cloud基础架构项目为本地开发做准备。
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 1cf1f9097f9897591fe59a390b0e73921f2300fa
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 开发概述

云基础架构远程环境上的Adobe Commerce是&#x200B;**只读**，包括所有入门级环境和所有Pro集成、暂存和生产环境。 在本地开发环境中，您可以先编写和测试代码，然后再将其推送到集成环境，以便进一步测试并部署到暂存和生产环境。

在准备本地工作区之前，请确保您拥有[凭据](../../get-started/prepare-workspace.md)。 本地开发要求安装PHP和Composer，除非您选择使用[Cloud Docker for Commerce](#docker-environment)。

## 必需的包

云基础架构上的Adobe Commerce使用编辑器管理项目的依赖项和升级。 对于本地开发，必须安装与云项目兼容的PHP和Composer版本。 例如，如果您使用[!DNL Commerce] 2.4.8云模板，则可以看到[`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml)配置文件使用&#x200B;**PHP 8.4**&#x200B;和&#x200B;**Composer 2.8.4**。

Composer将项目所需的库和依赖项安装在`vendor`目录中。 以下必需的编辑器文件位于项目根目录中：

- `composer.json` — 使用`composer.json`文件管理产品安装和升级。
- `composer.lock` - `composer.lock`文件存储一组精确版本依赖项，这些依赖项满足项目依赖项树中每个包的版本约束。

**常用命令：**

| 命令 | 描述 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | 更新到`composer.json`文件中反映的最新依赖项版本。 这将更新`composer.lock`文件。 |
| `composer install` | 读取`composer.lock`文件以下载依赖项。 最佳做法是在您的项目存储库中保留`composer.lock`的最新副本。 |

{style="table-layout:auto"}

添加、提交和推送更新的代码后，部署进程将在`composer install`构建阶段[期间自动运行](../deploy/process.md#build-phase-build-phase)命令。

### 云中继

云基础架构上的Adobe Commerce使用需要`magento/product-enterprise-edition`的中继包。 要获取最新版本的Commerce的最新更新，请使用以下约束语法：

```text
>=current_version <next_version
```

例如，要使用最新的Adobe Commerce版本2.4.9，请在`2.4.8`文件中将`2.4.9`设置为“当前”版本，将`composer.json`设置为“下一个”版本：

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

此中继包的主要包如下：

- **vendor/magento/ece-tools** — `ece-tools`包与Adobe Commerce版本2.1.4及更高版本兼容，以提供一组丰富的功能，可用于在云基础架构项目上管理Adobe Commerce。 它包含脚本和云基础架构上的Adobe Commerce命令，旨在帮助管理代码并自动构建和部署项目。 查看[`ece-tools`包概述](../dev-tools/package-overview.md)。
- **vendor/magento/product-enterprise-edition** — 此中继需要应用程序组件，包括模块、框架、主题等。
- **vendor/fastly2/magento2** — 此模块管理Pro暂存环境和生产环境以及入门级生产环境的Fastly CDN和服务。 查看[Fastly服务](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2)。
- **vendor/magento/module-paypal-on-boarding** — 此模块通过连接到您的PayPal商家帐户来提供PayPal付款网关结帐。 查看[PayPal入门培训工具](../store/paypal.md)。
- **供应商/aem/rum** — 此模块管理[操作遥测](../monitor/operational-telemetry.md)数据收集工具。

>[!TIP]
>
>有关依赖项和第三方许可证的列表，请参阅[Adobe Commerce发行说明](/help/cloud-guide/release-notes/cloud-packages.md)中的&#x200B;_Commerce云包_。

## Docker环境

您可以使用Cloud Docker for Commerce工具在云基础架构生产和开发环境中模拟Adobe Commerce以进行本地开发。 适用于Commerce的Cloud Docker不需要在本地安装PHP和编辑器。

- 在Adobe Developer站点中使用Cloud Docker [进行本地开发](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
- [Docker体系结构和常用命令](../dev-tools/cloud-docker.md)
- [Cloud Docker发行说明](../release-notes/cloud-docker.md)

>[!TIP]
>
>有关在云基础架构上使用基于Git的托管服务与Adobe Commerce的信息，请参阅[集成](../integrations/overview.md)。
