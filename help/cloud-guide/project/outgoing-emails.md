---
title: 配置传出电子邮件
description: 了解如何在云基础架构上为Adobe Commerce启用传出电子邮件。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# 配置传出电子邮件

您可以从[!DNL Cloud Console]或命令行为集成（和暂存仅用于入门）环境启用和禁用传出电子邮件。 启用传出电子邮件以向Cloud项目用户发送双重身份验证或重置密码电子邮件。

默认情况下，传出电子邮件在“生产”和“暂存”（仅限Pro）环境中启用。 但是，在您通过[命令行](#enable-emails-in-the-cli)或[Cloud Console](outgoing-emails.md#enable-emails-in-the-cloud-console)设置`enable_smtp`属性之前，**[!UICONTROL Enable outgoing emails]**&#x200B;设置可能在环境设置中显示为禁用，而不管状态如何。

通过[命令行](#enable-emails-in-the-cli)更新`enable_smtp`属性值也会在Cloud Console上更改此环境的[!UICONTROL Enable outgoing emails]设置值。

>[!NOTE]
>
>启用/禁用&#x200B;**[!UICONTROL Enable outgoing emails]**&#x200B;设置将不会在Pro暂存或生产环境中启用/禁用电子邮件。

{{redeploy-warning}}

## 在Cloud Console启用电子邮件

使用&#x200B;_配置环境_&#x200B;视图中的&#x200B;**[!UICONTROL Outgoing emails]**&#x200B;切换启用或禁用电子邮件支持。

如果必须在专业生产或暂存环境中禁用或重新启用传出电子邮件，则可以提交[Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)。

>[!TIP]
>
>在Cloud Console中，Pro暂存或生产环境可能不会反映出站电子邮件状态。

**要管理来自[!DNL Cloud Console]**&#x200B;的电子邮件支持，请执行以下操作：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。
1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。
1. 在项目仪表板上，单击右上角的配置图标。
1. 单击&#x200B;**[!UICONTROL Environments]**&#x200B;并从列表中选择特定环境（Staging和Production for Pro除外）。
1. 要启用或禁用传出电子邮件，请切换&#x200B;_启用传出电子邮件_ **打开**&#x200B;或&#x200B;**关闭**。

   ![启用传出电子邮件配置](../../assets/outgoing-emails.png)

更改设置后，环境将使用新配置进行构建和部署。

## 在CLI中启用电子邮件

您可以使用`magento-cloud` CLI `environment:info`命令更改活动环境的电子邮件配置以设置`enable_smtp`属性。 启用SMTP将使用SMTP主机的IP地址更新`MAGENTO_CLOUD_SMTP_HOST`环境变量以发送邮件。

**要从命令行管理电子邮件支持**：

1. 在本地工作站上，转到您的项目目录。

1. 检查环境的传出电子邮件设置。

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. 通过将`enable_smtp`环境变量设置为`true`或`false`更改电子邮件支持配置。

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   等待环境构建和部署。

1. 使用SSH登录到远程环境。

1. 验证电子邮件是否有效；向可检查的地址发送测试电子邮件。

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. 验证SendGrid是否拾取电子邮件。

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
