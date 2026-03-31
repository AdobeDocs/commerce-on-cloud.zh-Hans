---
title: 管理员变量
description: 请参阅在云基础架构上安装Adobe Commerce时使用的环境变量列表。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: ac1b2001294ba72304fc7ad3c760872dbd73e44f
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 管理员变量

对云基础架构项目上的Adobe Commerce具有管理访问权限的用户可以使用以下项目环境变量覆盖管理用户帐户的配置设置以访问管理UI。

## 管理员凭据

您可以在安装Commerce期间使用下表中的管理员变量覆盖管理员用户凭据。

如果要在安装后更改值，请使用SSH连接到您的环境，并使用Adobe Commerce CLI [`admin:user`命令](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=zh-Hans)创建或编辑Admin用户凭据。

| 变量 | 默认 | 描述 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | 许可证所有者用户名 | 能够创建其他用户（包括管理用户）的管理用户的用户名。 |
| `ADMIN_FIRSTNAME` | 许可证所有者名字 | 管理用户的名字。 |
| `ADMIN_LASTNAME` | 许可证所有者姓氏 | 管理用户的姓氏。 |
| `ADMIN_EMAIL` | 许可证所有者电子邮件 | 管理用户的电子邮件地址。 此地址用于发送密码重置通知。 |
| `ADMIN_PASSWORD` | 许可证所有者密码 | 管理用户的密码。 创建项目后，将生成随机密码并向许可证所有者发送电子邮件。 在项目创建期间，许可证所有者应该已经更改了密码。 请与许可证所有者联系以获取更新的密码。 |
| `ADMIN_LOCALE` | `en_US` | 管理员使用的默认区域设置。 |

## 管理员URL

使用以下环境变量保护对管理员UI的访问。 如果指定，此值将在安装期间覆盖默认URL。 在云基础架构的[!DNL Adobe Commerce]中，您必须使用（[!DNL Cloud Console]或[!DNL Cloud CLI]）中的`ADMIN_URL`变量设置或更改管理员URL。 从[!DNL Admin]修改设置仅适用于内部部署。

`ADMIN_URL` — 用于访问管理员UI的相对URL。 默认URL为`/admin`。

### 更改管理员URL

默认情况下，[Commerce管理员](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html?lang=zh-Hans) URL设置为&#x200B;*&lt;域名>/管理员*。 出于安全原因，Adobe建议将其更改为不容易猜测的唯一自定义管理员URL。

**在云基础架构的[!DNL Adobe Commerce]中**，您必须使用（[!DNL Cloud Console]或[!DNL Cloud CLI]）中的`ADMIN_URL`环境变量更改管理员URL。 从[!DNL Admin]修改设置仅适用于内部部署。 对于内部部署，请遵循[使用自定义管理员URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=zh-Hans#use-a-custom-admin-url)。

Adobe建议在安装后更改管理员URL的环境级变量。 在从克隆的`master`环境进行分支之前，出于安全原因配置此设置。 从`master`分支创建的所有分支都会继承环境级变量及其值，除非您将继承设置为false。

使用[!DNL Cloud Console]或[!DNL Cloud CLI]设置或更新`ADMIN_URL`。

#### 选项A：使用[!DNL Cloud Console]更改管理员URL

##### 集成环境

从[Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=zh-Hans)，添加新的变量，其包含：

- **名称：** `ADMIN_URL`
- **值：**&#x200B;您的新管理员URL（例如，`magento_A8v10`）

- 有关详细步骤，请参阅我们的开发人员文档中的[添加环境变量](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=zh-Hans#configure-environment)或[环境变量](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html?lang=zh-Hans)。

##### 在[!DNL Cloud Console]中设置管理员URL

1. 登录到[云控制台](https://console.adobecommerce.com/)。
2. 从&#x200B;**[!UICONTROL All projects]**&#x200B;列表中选择一个项目。
3. 在项目概述中，选择环境并单击配置图标。
4. 选择&#x200B;**[!UICONTROL Variables]**&#x200B;选项卡。
5. 单击&#x200B;**[!UICONTROL Create Variable]**(或编辑现有的`ADMIN_URL`变量（如果存在）。
6. 输入以下内容：
   - **变量名称：** `ADMIN_URL`
   - **值：**&#x200B;您的新管理员路径（例如，`magento_A8v10`）。

   默认情况下，已选择&#x200B;**[!UICONTROL Available during runtime]**&#x200B;和&#x200B;**[!UICONTROL Make inheritable]**。 要防止子环境继承此值，请清除此变量的&#x200B;**[!UICONTROL Make inheritable]**。
7. 单击&#x200B;**[!UICONTROL Create variable]**（或&#x200B;**[!UICONTROL Save]**）并等待部署完成。 仅当必填字段包含值时，按钮才可见。

##### 当暂存和生产在[!DNL Cloud Console]中不可用时

[提交支持票证](https://experienceleague.adobe.com/zh-hans/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)，请求为暂存或生产环境添加`ADMIN_URL`变量。 如果可从[!DNL Cloud Console]访问暂存和生产环境，请按照[集成环境](#integration-environment)中的说明添加变量。

#### 选项B：使用[!DNL Cloud CLI]更改管理员URL

使用`magento-cloud variable:update`命令更新变量。 （`variable:set`命令已弃用，不可用。）

以下示例将`master`环境`ADMIN_URL`更新为`newAdmin_A8v10`并阻止子环境继承该值：

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **重新部署：**&#x200B;更改[!DNL Cloud CLI]中的`ADMIN_URL`变量会触发环境的重新部署。
- **继承：**&#x200B;变量默认可继承。 要防止子环境继承该值，请使用所示的`--inheritable false`选项。 有关详细信息，请参阅[变量级别可见性](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html?lang=zh-Hans#visibility)。

>[!NOTE]
>
>`ADMIN_URL`值接受字母(a-z、A-Z)、数字(0-9)和下划线字符(_)。 不接受空格或其他字符。
