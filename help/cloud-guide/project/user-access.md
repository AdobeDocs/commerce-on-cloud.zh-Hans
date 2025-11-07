---
title: 管理用户访问权限
description: 了解如何在云基础架构项目和环境中管理用户对Adobe Commerce的访问权限。
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 953593de-f675-49fd-988f-f11306f67fbd
source-git-commit: c972d9f2029499cf53edc334c1d9a40b155a991d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# 管理用户访问权限

云基础架构上的Adobe Commerce项目使用基于角色的访问。 在项目级别有两个可用的角色：

- **项目管理员** — 写入对所有项目环境的访问权限，并可管理用户、推送代码和更新项目设置。 （以前称为&#x200B;**超级管理员**）
- **项目查看器** — 对所有项目环境的仅查看访问权限。

项目查看者不能在任何环境中执行任务；但是，您可以授予项目查看者对特定环境类型的写入权限。

环境级访问基于环境类型：生产、暂存和开发。 向用户&#x200B;_查看器_&#x200B;授予&#x200B;_开发_&#x200B;环境的权限意味着他们可以查看项目中的&#x200B;**所有**&#x200B;开发环境。 下表说明了授予各个权限级别的功能：

| 权限级别 | 访问 | SSH访问 |
| ------------------ | ----------- | :----------: |
| **管理员** | 执行管理员任务，如更改设置、推送代码、执行任务和分支管理（包括与父环境合并） | 是 |
| **参与者** | 推送代码并分支环境；无法更改设置或执行操作 | 是 |
| **查看器** | 对环境类型的仅查看访问权限 | 否 |
| **无访问权限** | 无法访问环境类型 | 否 |

{style="table-layout:auto"}

您可以使用`magento-cloud` CLI或[!DNL Cloud Console]添加用户和分配角色。

>[!BEGINSHADEBOX]

**先决条件：**

- Adobe ID的注册用户。 用户必须[注册Adobe帐户](https://account.adobe.com)，然后通过访问[https://console.adobecommerce.com](https://console.adobecommerce.com)初始化其[Cloud帐户](https://console.adobecommerce.com)，然后才能将其添加到Cloud项目。
- 分配了&#x200B;**管理员**&#x200B;角色的用户无法使用`magento-cloud` CLI管理用户。 只有被授予&#x200B;**帐户所有者**&#x200B;角色的用户才能管理用户。

>[!ENDSHADEBOX]

## 使用CLI管理用户

使用`magento-cloud` CLI管理用户并与自动化系统集成：

- `magento-cloud user:add` — 添加用户到项目
- `magento-cloud user:delete` — 删除用户
- `magento-cloud user:list [users]`列出项目用户
- `magento-cloud user:role` — 查看或更改用户角色
- `magento-cloud user:update` — 更新项目中的用户角色

以下示例使用`magento-cloud` CLI添加用户、配置角色、修改项目分配以及分配用户角色。

**要添加用户并分配角色**：

1. 使用`magento-cloud` CLI添加用户。

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >用户必须具有Adobe ID；请参阅[先决条件](#add-users-and-manage-access)。

1. 按照提示操作：指定用户电子邮件地址，设置项目和环境类型角色，并添加用户。

   > 示例提示

   ```
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   添加用户后，Adobe会向指定地址发送一封电子邮件，说明如何访问Adobe Commerce on cloud infrastructure项目。

### 查看用户的项目角色

```bash
magento-cloud user:get alice@example.com
```

>示例响应：

```
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 将用户添加到多个环境

要在`viewer`环境中将用户添加为`Production`，并在`contributor`环境中将用户添加为`Integration`，请执行以下操作：

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### 更新用户环境权限

要在`admin`环境中将用户环境权限更新为`Production`，请执行以下操作：

```bash
magento-cloud user:update alice@example.com -r production:a
```

## 从[!DNL Cloud Console]管理用户

您可以使用[[!DNL Cloud Console]](../../get-started/cloud-console.md)添加权限并使用&#x200B;_编辑_&#x200B;功能修改现有用户的权限。

>[!IMPORTANT]
>
>用户必须具有Adobe ID；请参阅[先决条件](#add-users-and-manage-access)。

### 在项目中添加用户

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com/)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 在项目仪表板上，单击右上角的配置图标。

1. 在&#x200B;_项目设置_&#x200B;下，单击&#x200B;**[!UICONTROL Access]**。

1. 在&#x200B;_访问_&#x200B;视图中，单击&#x200B;**[!UICONTROL Add]**。

1. 完成&#x200B;_[!UICONTROL Add User]_表单：

   - 输入用户电子邮件地址。

   - **[!UICONTROL Project admin]** — 向所有设置和环境类型授予管理员权限。

   - **[!UICONTROL Environment types and permissions]** — 向特定环境类型授予访问权和特定权限级别。 _无访问权限_、_管理员_（更改设置、执行操作、合并代码）、_参与者_（推送代码）或&#x200B;_查看器_（仅查看）。

   >[!TIP]
   >
   >只有&#x200B;**项目管理员**&#x200B;可以在任何环境中管理用户。 要授予用户访问&#x200B;**访问**&#x200B;选项卡的权限，其他&#x200B;**项目管理员**&#x200B;或&#x200B;**帐户所有者**&#x200B;必须为该用户分配&#x200B;**项目管理员**&#x200B;角色。

1. 单击&#x200B;**[!UICONTROL Add User]**。

   >[!IMPORTANT]
   >
   >添加用户不会自动触发部署。

1. 添加用户后，重新部署所有环境以应用更改。 添加用户不会自动触发部署。 重新部署是确保用户可以使用SSH访问环境或执行管理员任务的重要步骤。

添加用户后，Adobe会向指定地址发送一封电子邮件，说明如何访问Adobe Commerce on cloud infrastructure项目。

## 用户身份验证要求

为了提高安全性，Adobe提供了项目级别的多因素身份验证(MFA)实施，要求对云基础架构项目源代码和环境上的Adobe Commerce的SSH访问进行双重身份验证(TFA)。 请参阅[为SSH启用MFA](multi-factor-authentication.md)。

在云基础架构项目上的Adobe Commerce上启用MFA强制执行后，所有对该项目中的环境具有SSH访问权限的用户都必须在其云基础架构帐户上的Adobe Commerce上启用TFA。 对于自动化进程，您可以创建计算机用户和API令牌，以通过命令行进行身份验证。

将用户添加到云项目后，请要求用户查看其帐户安全设置并根据需要添加以下安全配置：

- **启用TFA** — 通过配置双重身份验证满足安全和合规性标准。 使用[MFA强制](multi-factor-authentication.md)配置的项目需要使用SSH访问项目的帐户上的TFA。

- **启用SSH密钥** — 需要访问Cloud Infrastructure源代码存储库上的Adobe Commerce的用户必须在其帐户上启用SSH密钥。 请参阅[安全连接](../development/secure-connections.md)。

- **创建API令牌** — 用户必须生成用于环境SSH访问的API令牌。 您需要令牌以启用自动化流程的身份验证工作流。

  在启用了MFA强制的项目中，您必须使用API令牌来验证来自自动帐户的SSH访问请求。 令牌允许自动化进程绕过需要TFA的身份验证工作流。

### 为云帐户启用TFA

云基础架构上的Adobe Commerce支持使用以下任意应用程序进行TFA：

- [Google Authenticator (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [授权(Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth身份验证器（Firefox OS、桌面、其他）](https://github.com/gbraad-apps/gauth)

有关安装验证器应用程序和启用TFA的说明，请参阅&#x200B;_中的_&#x200B;帐户设置[!DNL Cloud Console]页。

**要在您的用户帐户上启用TFA**：

1. 登录到[您的帐户](https://console.adobecommerce.com)。

1. 在右上角帐户菜单中，单击&#x200B;**[!UICONTROL My Profile]**。

1. 在&#x200B;_安全性_&#x200B;选项卡上，单击&#x200B;**[!UICONTROL Set up application]**。

1. 如果您的移动设备上没有经过批准的验证器应用程序，请使用链接说明安装该应用程序。

1. 将您的Adobe Commerce on cloud infrastructure帐户添加到验证器应用程序。

   - 在移动设备上，打开验证器应用程序。 然后，将设置代码添加到应用程序中。

   - 在[!UICONTROL **[!UICONTROL TFA set up - Application]**]页面的&#x200B;**[!UICONTROL Application verification code]**&#x200B;字段中键入移动设备中的TFA代码。

   - 单击&#x200B;**[!UICONTROL Verify and save]**。

     如果代码有效，Adobe会向帐户电子邮件地址发送通知，确认帐户现在具有TFA。

1. 可选。 启用&#x200B;_受信任的浏览器_&#x200B;设置以将浏览器中的身份验证代码缓存30天。

   此配置减少了项目登录期间的身份验证挑战。

1. 单击&#x200B;**保存**&#x200B;或&#x200B;**跳过**。

1. 保存恢复代码。

   - 在&#x200B;_TFA设置 — 恢复_&#x200B;代码页面上，复制并保存恢复代码，以便在无法访问移动设备或身份验证应用程序时登录到Adobe Commerce上的云基础架构项目。

   - 将恢复代码复制到其他位置，或者在您无法访问设备或身份验证应用程序时将其写下。

   - 单击&#x200B;**保存**&#x200B;以将代码保存到您的帐户，以便您可从帐户安全设置查看和管理这些代码。

     >[!WARNING]
     >
     >如果您无法访问具有TFA的帐户，并且没有恢复代码列表，则必须联系项目管理员，或[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以重置TFA应用程序。

1. 完成TFA设置后，单击&#x200B;**保存**&#x200B;以更新您的帐户。

1. 通过TFA验证当前会话。

   - 注销您的帐户。
   - 使用您的用户名和密码登录。
   - 出现提示时，从移动设备上的验证器应用程序输入`accounts.magento.cloud`条目的TFA代码。

### 管理TFA配置和恢复代码

您可以从&#x200B;_我的个人资料_&#x200B;页面上的&#x200B;_安全性_&#x200B;部分管理Adobe Commerce on cloud infrastructure帐户的TFA配置。

1. 登录到[您的帐户](https://console.adobecommerce.com)。

1. 在右上角帐户菜单中，单击&#x200B;**[!UICONTROL My Profile]**。

1. 在&#x200B;_我的个人资料_&#x200B;页面上，单击&#x200B;**[!UICONTROL Security]**&#x200B;选项卡。

1. 使用可用链接更新云基础架构帐户上Adobe Commerce的TFA设置：

   - 禁用TFA
   - 重置验证程序应用程序
   - 添加或删除受信任的浏览器
   - 查看或刷新您帐户上的TFA恢复代码

### 创建API令牌

可以将API令牌交换为OAuth 2访问令牌，然后该令牌可用于验证请求。

在已启用MFA强制的项目中，您必须具有API令牌才能为计算机用户和自动化流程启用SSH访问。

>[!IMPORTANT]
>
>保护帐户的API令牌值。 不要在代码示例、屏幕捕获或不安全的客户端 — 服务器通信中公开值。 此外，不要公开存储在公共存储库中的源代码中的值。

**要创建API令牌**：

1. 登录到[您的帐户](https://console.adobecommerce.com)。

1. 在右上角帐户菜单中，单击&#x200B;**[!UICONTROL My Profile]**。

1. 在&#x200B;_我的个人资料_&#x200B;页面上，单击&#x200B;**[!UICONTROL API tokens]**&#x200B;选项卡。

1. 单击&#x200B;**[!UICONTROL Create API token]**&#x200B;并输入名称，例如，指定与计算机用户或使用API令牌的自动化进程匹配的名称。

   ![API令牌](../../assets/api-token-name.png)

1. 单击&#x200B;**[!UICONTROL Create API token]**。
