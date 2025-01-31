---
title: 配置文件概述
description: 了解如何配置云基础架构环境以支持部署和管理自定义的Adobe Commerce存储。
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 配置文件概述

Adobe Commerce中云基础架构上的环境包括带有应用程序、服务和数据库的容器，可为您的Adobe Commerce应用程序代码库和文件提供完整的系统。

您可以使用以下配置文件配置应用程序设置、路由、生成和部署操作以及通知，以支持您的项目环境：

| 配置 | 文件名 | 描述 |
| ------------- | -------- | ----------- |
| [应用程序](../application/configure-app-yaml.md) | `.magento.app.yaml` | 定义如何构建和部署Adobe Commerce，包括服务、挂接和cron作业。 |
| [环境](configure-env-yaml.md) | `.magento.env.yaml` | 使用环境变量集中管理所有环境（包括专业暂存和生产）中的构建和部署操作。 |
| [路由](../routes/routes-yaml.md) | `.magento/routes.yaml` | 配置缓存、重定向和服务器端包含。 |
| [服务](../services/services-yaml.md) | `.magento/services.yaml` | 按名称和版本定义Adobe Commerce使用的服务。 例如，此文件可能包含MariaDB、PHP扩展名、Redis、RabbitMQ以及Elasticsearch或OpenSearch的版本。 您必须打开支持工单将这些更改推送到专业计划暂存和生产环境。 |
| [PHP设置](../application/php-settings.md#configure-php) | `php.ini` | 可添加到项目的可选文件。 此文件中包含的设置会附加到云基础架构维护的设置中。 |

{style="table-layout:auto"}

## Pro环境的配置更新

对于云基础架构上的Adobe Commerce Pro暂存和生产环境，您可以更新本地开发环境中的许多配置选项，并提交更改以将其应用于这些环境。 但是，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以更新以下配置选项：

- 在`.magento/services.yaml`文件中安装或更新服务。
- 更改`.magento.app.yaml`文件中`mounts`和`disk`属性的配置。

{{pro-self-service-warning}}
