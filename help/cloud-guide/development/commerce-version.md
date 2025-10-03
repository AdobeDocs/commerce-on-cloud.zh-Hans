---
title: 升级Commerce版本
description: 了解如何在云基础架构环境中升级Adobe Commerce版本。
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 7f9aac358effdf200b59678098e6a1635612301b
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 0%

---

# 升级Commerce版本

您可以将Adobe Commerce代码库升级到较新版本。 在升级环境之前，请查看[安装](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)指南中的&#x200B;_系统要求_&#x200B;以了解最新的软件版本要求。

根据环境类型（“开发”、“暂存”或“生产”），您的升级任务可能包括：

- 使用MariaDB (MySQL)、OpenSearch、RabbitMQ和Redis的新版本更新`.magento/services.yaml`文件，以便与新的Adobe Commerce版本兼容。
- 使用挂接和环境变量的新设置更新`.magento.app.yaml`文件。
- 将第三方扩展升级到支持的最新版本。

{{upgrade-tip}}

{{pro-update-service}}

## 配置文件

在升级应用程序之前，必须更新项目配置文件，以便考虑对云基础架构或应用程序上Adobe Commerce的默认配置设置所做的更改。 可以在[magento-cloud GitHub存储库](https://github.com/magento/magento-cloud)中找到最新的默认值。

### composer.json

升级之前，请始终检查`composer.json`文件中的依赖项是否与Adobe Commerce版本兼容。

要更新Adobe Commerce版本2.4.4及更高版本的`composer.json`文件**：

1. 将以下`allow-plugins`添加到`config`部分：

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 将以下插件添加到`require`部分：

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 将以下组件添加到`extra:component_paths`部分：

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. 保存文件。 尚未提交更改或将更改推送到分支。

1. 继续升级过程。

## 环境备份

我们建议在升级之前创建实例的备份。 使用以下步骤可备份集成、暂存和生产环境。

**要备份集成环境数据库和代码**：

1. 创建远程数据库的本地备份。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump`命令运行带有[标志的](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)mysqldump`--single-transaction`命令，该标志允许您在不锁定表的情况下备份数据库。

1. 备份代码和介质。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   或者，如果您有大量静态文件已在源代码管理中，则可以省略`[--media]`。

**要在部署**&#x200B;之前备份暂存或生产环境数据库，请执行以下操作：

1. 使用SSH登录到远程环境。

1. 创建[数据库转储](../storage/database-dump.md)。 要为数据库转储选择目标目录，请使用`--dump-directory`选项。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   转储操作将在远程项目目录中创建`dump-<timestamp>.sql.gz`存档文件。 请参阅[备份数据库](../storage/database-dump.md)。

## 应用程序升级

在升级应用程序之前，请查看[服务版本](../services/services-yaml.md#service-versions)信息以了解最新的软件版本要求。

**升级应用程序版本**：

1. 在本地工作站上，转到您的项目目录。

1. 为目标升级版本设置[版本约束](overview.md#cloud-metapackage)。 仅当目标版本位于现有约束之外时，才需要执行此步骤。

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >您必须使用版本约束语法才能成功更新`ece-tools`包。 您可以在`composer.json`文件中找到用于升级的[应用程序模板](https://github.com/magento/magento-cloud/blob/master/composer.json)的版本限制。

1. 使用核心Commerce升级版本更新您的`composer.json`文件。

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. 如果您使用的是B2B，请将您的`composer.json`文件更新为Commerce的[支持的版本](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions)。

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. 更新项目依赖关系。

   ```bash
   composer update
   ```

1. 查看当前应用的修补程序：

   - 如果`m2-hotfixes`目录中安装了任何修补程序，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)，并与Adobe Commerce支持部门合作，验证哪些修补程序仍可以应用于新版本。 从`m2-hotfixes`目录中删除不适用的修补程序。

   - 如果[文件中应用了任何]质量修补程序`.magento.env.yaml`，请验证它们是否仍可应用于新版本。 从`QUALITY_PATCHES`文件的`.magento.env.yaml`部分删除不适用的修补程序。

   **方法1**： [验证Quality Patches发行说明中的适用版本](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法2**： [查看可用的修补程序和状态](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法3**： [搜索修补程序](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   由于Composer封送基础包的方式，将所有更改的文件添加到源代码管理时需要`git add -A`。 `composer install`和`composer update`将基包（`magento/magento2-base`和`magento/magento2-ee-base`）中的文件封送到包根中。

   Composer封送的文件属于新版本的Adobe Commerce，用于覆盖这些相同文件的过时版本。 目前，Adobe Commerce中已禁用封送处理，因此您必须将封送处理文件添加到源代码管理。

1. 等待部署完成。

1. 通过使用SSH登录并检查版本，在集成、暂存或生产环境中验证升级。

   ```bash
   php bin/magento --version
   ```

### 升级扩展

在Marketplace或其他公司站点中查看您的第三方扩展和模块页面，并验证在云基础架构上对Adobe Commerce和Adobe Commerce的支持。 如果必须升级任何第三方扩展和模块，Adobe建议在禁用扩展的情况下使用新的集成分支。

**验证并升级扩展**：

1. 在本地工作站上创建分支。

1. 根据需要禁用扩展。

1. 可用时，下载扩展升级。

1. 按照第三方文档的说明安装升级。

1. 启用并测试扩展。

1. 添加、提交代码更改并将其推送到远程。

1. 推送到并在您的集成环境中测试。

1. 推送到暂存环境以在预生产环境中测试。

Adobe强烈建议在&#x200B;_之前升级您的生产环境_，包括在您的站点启动过程中升级的扩展。

>[!NOTE]
>
>当您升级应用程序版本时，升级过程将自动更新到[Fastly CDN模块](../cdn/fastly.md#fastly-cdn-module-for-magento-2)的最新版本。

## 升级疑难解答

如果升级失败，您会在浏览器中收到一条错误消息，指示您无法访问店面或管理面板：

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**要解决错误**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 打开`./app/var/report/<error number>`文件。

1. [检查日志](../test/log-locations.md)并确定问题的来源。

1. 添加、提交和推送代码更改。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
