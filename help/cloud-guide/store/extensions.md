---
title: 管理扩展
description: 了解如何在云基础架构上的Adobe Commerce中安装和管理扩展。
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 管理扩展

通过从[Commerce Marketplace](https://marketplace.magento.com)添加扩展，您可以扩展Adobe Commerce应用程序功能。 例如，您可以添加主题以更改店面的外观，或者添加语言包以将店面和管理员本地化。

>[!NOTE]
>
>为了避免安装问题，必须使用拥有云项目的同一帐户(MAGEID)完成所有Marketplace购买。

## 扩展的编辑器名称

尽管本节讨论如何从Commerce Marketplace获取扩展名的编辑器名称和版本，但您可以在模块的编辑器文件中找到&#x200B;_any_&#x200B;模块的名称和版本。 在文本编辑器中打开`composer.json`文件并记下`"name"`和`"version"`值。

**要从Commerce Marketplace**&#x200B;中获取模块的编辑器名称，请执行以下操作：

1. 使用用于购买该组件的用户名和密码登录到[Commerce Marketplace](https://marketplace.magento.com)。

1. 单击右上角的用户名并选择&#x200B;**我的个人资料**。

   ![访问你的Marketplace帐户](../../assets/marketplace/my-profile.png)

1. 在&#x200B;_我的帐户_&#x200B;页面上，单击&#x200B;**我的购买**。

   ![市场购买历史记录](../../assets/marketplace/my-purchases.png)

1. 在&#x200B;_我的购买_&#x200B;页面上，选择您购买的模块，然后单击&#x200B;**技术详细信息**。

1. 单击&#x200B;**复制**&#x200B;以将[!UICONTROL Component name]复制到剪贴板。

1. 打开文本编辑器并粘贴组件名称并附加一个冒号字符(`:`)。

1. 在&#x200B;**技术详细信息**&#x200B;中，单击&#x200B;**复制**&#x200B;以将[!UICONTROL Component version]复制到剪贴板。

1. 在文本编辑器中，将版本号附加到组件名称中冒号的后面。 例如：

   ```text
   extension-name/magento2:1.0.1
   ```

## 安装扩展

在向实施中添加扩展时，Adobe建议在开发分支中工作。 安装扩展时，扩展名(`<VendorName>_<ComponentName>`)会自动插入到[`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html)文件中。 无需直接编辑文件。

**要安装扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 创建或签出开发分支。 查看[分支](../development/cli-branches.md)。

1. 使用编辑器名称和版本，将扩展名添加到`composer.json`文件的`require`部分。

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. 更新项目依赖关系。

   ```bash
   composer update
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >安装扩展时，在将代码更改推送到远程环境时必须包含`composer.lock`文件。 `composer install`命令读取`composer.lock`文件以在远程环境中启用定义的依赖项。

1. 构建和部署完成后，使用SSH登录到远程环境并验证已安装的扩展。

   ```bash
   bin/magento module:status <extension-name>
   ```

   扩展名使用格式： `<VendorName>_<ComponentName>`。

   示例响应：

   ```
   Module is enabled
   ```

   如果遇到部署错误，请参阅[扩展部署失败](../deploy/recover-failed-deployment.md)。

## 管理扩展

使用编辑器添加扩展时，部署过程会自动启用该扩展。 如果已安装扩展，则可以使用CLI启用或禁用该扩展。 管理扩展时，请使用格式： `<VendorName>_<ComponentName>`

在登录到远程环境时，切勿启用或禁用扩展。

**启用或禁用扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 启用或禁用模块。 `module`命令使用请求的模块状态更新`config.php`文件。

   >启用模块。

   ```bash
   bin/magento module:enable <module-name>
   ```

   >禁用模块。

   ```bash
   bin/magento module:disable <module-name>
   ```

1. 如果启用了模块，请使用`ece-tools`刷新配置。

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. 验证模块的状态。

   ```bash
   bin/magento module:status <module-name>
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## 升级扩展

在继续之前，您需要具有扩展的编辑器名称和版本。 此外，请确认该扩展与您的项目和Adobe Commerce版本兼容。 特别是，[在开始之前检查所需的PHP版本](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)。

**要更新扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 创建或签出开发分支。 查看[分支](../development/cli-branches.md)。

1. 在文本编辑器中打开`composer.json`文件。

1. 找到扩展并更新版本。

1. 保存更改并退出文本编辑器。

1. 更新项目依赖关系。

   ```bash
   composer update
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

如果遇到错误，请参阅[从组件故障中恢复](../deploy/recover-failed-deployment.md)。 要了解有关将扩展与Adobe Commerce结合使用的更多信息，请参阅&#x200B;_管理员指南_&#x200B;中的[扩展](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html)。
