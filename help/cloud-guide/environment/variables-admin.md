---
title: 管理员变量
description: 请参阅在云基础架构上安装Adobe Commerce时使用的环境变量列表。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# 管理员变量

对云基础架构项目上的Adobe Commerce具有管理访问权限的用户可以使用以下项目环境变量覆盖管理用户帐户的配置设置以访问管理UI。

## 管理员凭据

您可以在安装Commerce期间使用下表中的管理员变量覆盖管理员用户凭据。

如果要在安装后更改值，请使用SSH连接到您的环境，并使用Adobe Commerce CLI [`admin:user`命令](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html)创建或编辑Admin用户凭据。

| 变量 | 默认 | 描述 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | 许可证所有者电子邮件地址 | 能够创建其他用户（包括管理用户）的管理用户的用户名。 |
| `ADMIN_EMAIL` |                             | 管理用户的电子邮件地址。 此地址用于发送密码重置通知。 |
| `ADMIN_PASSWORD` |                             | 管理用户的密码。 创建项目后，将生成随机密码并向许可证所有者发送电子邮件。 在项目创建期间，许可证所有者应该已经更改了密码。 请与许可证所有者联系以获取更新的密码。 |
| `ADMIN_LOCALE` | `en_US` | 管理员使用的默认区域设置。 |

## 管理员URL

使用以下环境变量保护对管理员UI的访问。 如果指定，此值将在安装期间覆盖默认URL。

`ADMIN_URL` — 用于访问管理员UI的相对URL。 默认URL为`/admin`。 出于安全原因，Adobe建议您将默认设置更改为不容易猜测的唯一自定义管理员URL。

### 更改管理员URL

Adobe建议在安装后更改管理员URL的环境级变量。 在从克隆的`master`环境进行分支之前，出于安全原因配置此设置。 从`master`分支创建的所有分支都将继承环境级变量及其值。

使用`magento-cloud variable:update`命令更新变量值。 （`variable:set`命令已弃用，不可用。） 以下示例将ADMIN_URL更新为`newAdmin_A8v10`：

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>`ADMIN_URL`值接受自定义管理路径中的字母（a-z或A-Z）、数字(0-9)和下划线字符(_)。 **不接受空格或其他字符**。

**要使用[!DNL Cloud Console]**&#x200B;更改URL：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 在项目概述中，选择环境并单击配置图标。

   ![项目配置](../../assets/icon-configure.png){width="36"}

1. 选择&#x200B;**变量**&#x200B;选项卡。

1. 单击&#x200B;**创建变量**。

1. 输入以下内容：

   - **变量名称** = `ADMIN_URL`
   - **value** =新URL。 例如，将管理员URL设置为`magento_A8v10`。

   默认情况下，已选择`Available during runtime`和`Make inheritable`。

1. 单击&#x200B;**创建变量**&#x200B;并等待部署完成。 仅当必填字段包含值时，此按钮才可见。
