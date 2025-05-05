---
title: 访问Commerce管理面板
description: 了解如何访问Commerce管理面板。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# 访问Commerce管理面板

对Commerce管理面板具有管理访问权限的用户可以添加用户、配置商店服务、完成商店设置和自定义工作等。

对于新项目，收到欢迎电子邮件后的第一步是通过更改许可证所有者帐户上的密码来保护管理员对项目的访问。 该帐户的默认用户名是“许可证所有者”电子邮件地址。

您可以使用以下任一方法提交密码更改请求：

- 找到发送到许可证所有者电子邮件地址的欢迎电子邮件，然后按照链接更改密码。

- 将商店URL从[[!DNL Cloud Console]](../cloud-guide/project/overview.md)复制到浏览器中。 然后，将`/admin`附加到URL的末尾以打开登录页面。 单击&#x200B;**忘记密码？**&#x200B;链接，用于将密码更改请求发送到许可证所有者电子邮件地址。

提交密码更改请求后，请查看电子邮件中的密码重置通知。 如果未收到电子邮件，请检查垃圾邮件文件夹。

>[!TIP]
>
>如果密码重置失败或您无法登录到“管理员”面板，则具有管理员访问权限的用户可以使用SSH连接到项目，并使用`admin:user:create` CLI命令添加管理员用户。 请参阅&#x200B;_安装指南_&#x200B;中的[创建、编辑或解锁管理员帐户](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=zh-Hans)。

## 监测站点运行状况

[全站点分析工具](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/site-wide-analysis-tool/intro)是一个主动式自助服务工具和中央存储库，其中包含详细的系统分析和建议，以确保Adobe Commerce安装的安全性和可操作性。 它提供全天候的实时性能监控、报告和建议，以发现潜在问题并更好地了解站点运行状况、安全性和应用程序配置。 它有助于缩短解决时间并提高站点稳定性和性能。 您可以直接从[管理面板](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel)访问站点范围分析工具。
