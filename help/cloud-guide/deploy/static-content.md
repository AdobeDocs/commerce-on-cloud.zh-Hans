---
title: 静态内容部署
description: 了解在Adobe Commerce上针对云基础架构项目部署静态内容（如图像、脚本和CSS）的策略。
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 静态内容部署策略

静态内容部署(SCD)对存储部署过程有显着影响，这取决于要生成多少内容（如图像、脚本、CSS、视频、主题、区域设置和网页）以及何时生成内容。 例如，当站点处于维护模式时，默认策略会在[部署阶段](process.md#deploy-phase-deploy-phase)期间生成静态内容；但是，此部署策略需要一些时间才能将内容直接写入装入的`pub/static`目录。 您可以通过多种选项或策略来帮助您根据自己的需求缩短部署时间。

## 优化JavaScript和HTML内容

您可以在静态内容部署期间使用捆绑和缩小功能构建优化的JavaScript和HTML内容。

### 缩小内容

如果您跳过复制`var/view_preprocessed`目录中的静态视图文件并在请求时生成&#x200B;_缩小的_ HTML，则可以缩短部署过程中的SCD加载时间。 您可以通过在`.magento.env.yaml`文件中将[SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification)全局环境变量设置为`true`来激活此设置。

>[!NOTE]
>
>从`ece-tools`包版本2002.0.13开始，SKIP_HTML_MINIFICATION变量的默认值设置为`true`。

通过减少不必要的主题文件数，可以节省&#x200B;**更多**&#x200B;部署时间和磁盘空间。 例如，您可以部署英语版的`magento/backend`主题以及其他语言的自定义主题。 您可以使用[SCD_MATRIX](../environment/variables-deploy.md#scdmatrix)环境变量配置这些主题设置。

## 选择部署策略

根据您选择在&#x200B;_生成_&#x200B;阶段、_部署_&#x200B;阶段还是&#x200B;_按需_&#x200B;阶段生成静态内容，部署策略有所不同。 如下图所示，在部署阶段生成静态内容是最不理想的选择。 即使使用缩小的HTML，每个内容文件也必须复制到已装载的`~/pub/static`目录，这可能需要较长时间。 按需生成静态内容似乎是最佳选择。 但是，如果内容文件不存在于请求时生成的缓存中，则会增加用户体验的加载时间。 因此，在构建阶段生成静态内容是最理想的情况。

![SCD加载比较](../../assets/scd-load-times.png)

### 在生成时设置SCD

使用缩小的HTML在构建阶段生成静态内容是&#x200B;[**零停机时间**&#x200B;部署](reduce-downtime.md)的最佳配置，也称为&#x200B;**理想状态**。 它将从`./init/pub/static`目录创建符号链接，而不是将文件复制到已装入的驱动器。

生成静态内容需要访问主题和区域设置。 Adobe Commerce将主题存储在文件系统中（可在构建阶段访问）；但是，Adobe Commerce将区域设置存储在数据库中。 在生成阶段，数据库&#x200B;_不可用_。 为了在生成阶段生成静态内容，您必须使用`ece-tools`包中的`config:dump`命令将区域设置移动到文件系统。 它读取区域设置并将它们保存在`app/etc/config.php`文件中。

>[!NOTE]
>运行`ece-tools`包中的`config:dump`命令后，转储到`config.php`文件[的配置在管理员仪表板](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin)中将被锁定（灰显）。 在管理员中更新这些配置的唯一方法是从本地文件删除它们，然后重新部署项目。
>>此外，每次向实例添加新的商店/商店组/网站时，都应记得运行`config:dump`命令以确保数据库同步。 您还可以选择应将哪些配置](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en)转储到`config.php`文件中的[。
>>如果由于字段灰显而忽略执行此步骤而从`config.php`文件中删除商店/商店组/网站配置，则下次部署时将从数据库中删除未转储的新实体。

**要将项目配置为在生成**&#x200B;时生成SCD，请执行以下操作：

1. 在本地工作站上，转到您的项目目录。
1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 将语言环境移动到文件系统，然后更新[`config.php`文件](../development/commerce-version.md#create-a-configphp-file)。

1. `.magento.env.yaml`配置文件应包含以下值：

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)为`true`
   - 生成阶段中的[SKIP_SCD](../environment/variables-build.md#skip_scd)为`false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy)为`compact`

1. 验证`.magento.app.yaml`文件中[部署后挂接](../application/hooks-property.md)的配置。

1. 通过运行[智能向导来验证您的设置，以获得理想的状态](smart-wizards.md)。

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### 按需设置SCD

对于集成环境中的开发工作流，按需生成SCD是最佳选择。 它缩短了部署时间，以便您可以快速审查实施并运行集成测试。 在`.magento.env.yaml`文件的全局阶段中启用[SCD_ON_DEMAND](../environment/variables-global.md#scdondemand)环境变量。 SCD_ON_DEMAND变量将覆盖与SCD相关的所有其他配置，并清除`~/pub/static`目录中的现有内容。

使用SCD按需策略时，有助于使用预期请求的页面（例如主页）预加载缓存。 在`.magento.env.yaml`文件的部署后阶段的[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages)环境变量中添加预期页面列表。

>[!WARNING]
>
>请勿在生产环境中使用SCD on-demand策略。

### 跳过SCD

有时，您可以选择完全跳过生成静态内容。 您可以在全局阶段中设置[SKIP_SCD](../environment/variables-build.md#skipscd)环境变量以忽略与SCD相关的其他配置。 这不会影响`~/pub/static`目录中的现有内容。
