---
title: New Relic帐户管理
description: 了解如何访问您的New Relic帐户并管理您的Adobe Commerce在云基础架构项目上的访问权限、集成和工具使用。
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 558c645e353e38ce8455ef17e1d0e9fa99b22c6e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# New Relic帐户管理

Adobe配置云基础架构项目时，许可证所有者会收到New Relic的电子邮件，其中包含用于访问New Relic帐户的凭据和说明。 如果您没有收到电子邮件，请使用许可证所有者电子邮件地址来重置New Relic密码。

如果许可证所有者已更改，而新许可证所有者当前无权访问New Relic，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)。

## 管理用户访问权限（管理员角色）

>[!NOTE]
>
>仅向严格要求访问完整功能集的用户授予完全访问权限。

**在New Relic中访问“用户管理”**：

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)。

1. 从左下方导航中选择您的用户名。

1. 单击&#x200B;**[!UICONTROL Administration]**&#x200B;并从列表中选择以下选项之一：

   - **[!UICONTROL User management]**&#x200B;以添加用户并管理活动用户和待处理邀请。

   - **[!UICONTROL Access management]**&#x200B;以管理用户组、角色和帐户。

请参阅[New Relic](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/)文档中的&#x200B;_用户管理_。

## 为入门环境配置New Relic

>[!NOTE]
>
>**Pro环境**&#x200B;已预配置为使用New Relic服务，可以跳过启用和连接说明。 如果暂存环境和生产环境中未安装New Relic APM，或者New Relic基础架构在生产环境中不可用，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以请求安装。

对于入门环境，必须检查`.magento.app.yaml`文件以验证`runtime`部分是否包含New Relic扩展。 如果尚未配置该扩展，请添加以下内容：

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### 应用许可证密钥

要将Cloud环境连接到New Relic，请将New Relic许可证密钥添加到环境。

- 对于&#x200B;**Pro项目**，Adobe在配置过程中将许可证密钥添加到您的生产和暂存环境。 您可以登录到[New Relic帐户](https://login.newrelic.com/login)，以验证云基础架构网站上的Adobe Commerce与New Relic之间的连接。

- 对于&#x200B;**入门项目**，您拥有最多支持&#x200B;_三个_&#x200B;环境的New Relic许可证密钥。 您必须手动将密钥添加到环境配置。 未预配置入门环境以使用New Relic服务。

对于入门环境，请通过将New Relic许可证密钥添加到环境配置来启用New Relic集成。 将密钥添加到暂存环境和生产环境以及您选择的其他一个环境。 配置只需要使用New Relic许可证密钥。 您可以在[New Relic用户指南](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html?lang=zh-Hans)的&#x200B;_Adobe Commerce报表_&#x200B;主题中找到有关其他配置选项的信息。

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce帐户页面或与项目关联的New Relic许可证的登录凭据
>- [管理员级访问权限](../project/user-access.md)以配置入门环境
>- 用于访问环境的[管理员](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html?lang=zh-Hans)的凭据

**要为入门环境配置New Relic**：

1. 从[!DNL Cloud Console]或Cloud CLI中查找您的New Relic许可证密钥。

   **[!DNL Cloud Console]方法**：

   - 打开云项目[帐户页](https://accounts.magento.cloud/user)。

   - 在&#x200B;_项目_&#x200B;选项卡上，查找您的项目。

   - 单击&#x200B;**查看详细信息**&#x200B;以获取项目基础结构信息。

   - 展开&#x200B;**New Relic服务**&#x200B;部分以查看许可证密钥。

   - 复制许可证密钥。

   **云CLI方法**：

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. 使用`magento-cloud` CLI将New Relic许可证密钥添加到环境中。

   - 更改到需要许可证密钥的环境。
   - 使用以下`magento-cloud` CLI命令更新变量值：

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   或者，您可以从[Commerce管理员](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html?lang=zh-Hans#step-3%3A-configure-your-store)添加它。

1. 登录到您的[New Relic帐户](https://login.newrelic.com/login)以验证您是否可以从Adobe Commerce环境中查看数据。 请参阅[调查性能](investigate-performance.md)。

### 删除许可证密钥

您只能在三个活动环境中使用New Relic许可证密钥。 如果密钥在三个环境中使用，则必须先从其中一个环境中移除密钥，然后才能将其添加到其他环境。

**要从环境中移除许可证密钥**：

1. 列出环境变量。

   ```bash
   magento-cloud variable:list
   ```

   示例响应：

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >如果将许可证密钥添加为&#x200B;_项目_&#x200B;变量，则必须删除该项目级变量。 项目变量将许可证添加到每创建&#x200B;_个_&#x200B;环境分支中，这会占用或超出许可证限制。 要列出项目变量： `magento-cloud variable:list --level project`

1. 删除许可证变量。

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## 更改New Relic on Cloud的帐户所有者

要更改云基础架构项目中Adobe Commerce的New Relic帐户所有者，请执行以下操作：

1. 在New Relic UI中&#x200B;**更改所有者**。 请参阅New Relic文档中的[更改帐户所有者](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/)。

2. **如果用户不在您的帐户中，请先添加用户**。 请参阅New Relic文档中的[添加和更新用户](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users)。

3. **需要帮助？**&#x200B;如果没有现有的所有者或管理员可以提供帮助，则任何有权访问[Adobe Commerce合作伙伴所有者帐户](https://account.newrelic.com/accounts/1311131/users)的Adobe Commerce用户都可以代表您添加用户。

有关详细信息，请参阅[New Relic服务概述](https://experienceleague.adobe.com/zh-hans/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service)。
