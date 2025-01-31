---
title: 身份验证密钥
description: 了解如何将身份验证密钥应用于云基础架构上Adobe Commerce中的开发项目。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 身份验证密钥

您必须拥有身份验证密钥才能访问Adobe Commerce存储库，并在云基础架构项目上为Adobe Commerce启用安装和更新命令。 有两种方法可指定Composer授权凭据。

- **身份验证文件** — 在云基础架构根目录上的Adobe Commerce中包含您的Adobe Commerce [授权凭据](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html)的文件。
- **环境变量** — 一个环境变量，用于在Adobe Commerce on cloud infrastructure项目中设置身份验证密钥，以防止意外泄露。

>[!BEGINSHADEBOX]

**安全说明**

Adobe建议在云项目中使用[环境变量](#composer-auth-environment-variable)方法，以防止授权凭据意外泄露。

将Cloud Docker for Commerce用作本地开发工具时，身份验证文件方法非常理想，但请注意，不要将`auth.json`文件上传到基于Git的公用存储库。 您可以将`auth.json`文件添加到[`.gitignore`文件](../project/file-structure.md#ignoring-files)。

>[!ENDSHADEBOX]

## 身份验证文件

**要创建`auth.json`文件**：

1. 如果您的项目根目录中没有`auth.json`文件，请创建一个。

   - 使用文本编辑器，在项目根目录中创建`auth.json`文件。
   - 将[示例`auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample)的内容复制到新的`auth.json`文件中。

1. 将`<public-key>`和`<private-key>`替换为您的Adobe Commerce身份验证凭据。

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 保存更改并退出文本编辑器。

## Composer身份验证环境变量

以下方法是防止在基于Git的公共存储库中意外泄露敏感凭据的最佳方法。

**要使用环境变量**&#x200B;添加身份验证密钥：

1. 在&#x200B;_[!DNL Cloud Console]_中，单击项目导航右侧的配置图标。

   ![配置项目](../../assets/icon-configure.png){width="36"}

1. 在&#x200B;_项目设置_&#x200B;列表中，单击&#x200B;**[!UICONTROL Variables]**。

1. 单击&#x200B;**[!UICONTROL Create variable]**。

1. 在&#x200B;**[!UICONTROL Variable name]**&#x200B;字段中，输入`env:COMPOSER_AUTH`。

1. 在&#x200B;_值_&#x200B;字段中，添加以下内容，并将`<public-key>`和`<private-key>`替换为您的Adobe Commerce身份验证凭据：

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 选择&#x200B;**[!UICONTROL Available during buildtime]**&#x200B;并取消选择&#x200B;**[!UICONTROL Available during runtime]**。

1. 单击&#x200B;**[!UICONTROL Create variable]**。

1. 从每个环境中删除`auth.json`文件。
