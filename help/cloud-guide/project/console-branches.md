---
title: 使用 [!DNL Cloud Console]管理分支
description: 了解如何使用 [!DNL Cloud Console]在云基础架构上管理Adobe Commerce的环境分支。
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# 使用[!DNL Cloud Console]管理分支

您可以使用[!DNL Cloud Console]或`magento-cloud` CLI管理环境。 您的项目文件存储在Git存储库中。 您可以使用Git命令来管理代码，但`magento-cloud` CLI设计用于与平台功能交互，而Git命令则不然。 请参阅云CLI主题中的[Git命令](../dev-tools/cloud-cli-overview.md#git-commands)。

本主题讨论如何使用[!DNL Cloud Console]来：

- 添加或删除环境
- 从父环境同步(`git pull`)
- 将(`git push`)合并到父环境

>[!TIP]
>
>您无法从Pro暂存环境和生产环境创建分支。 您可以从`master`分支中分支。

## 创建环境

分支策略使用通用的Git工作流，您可以在其中开发代码并在开发分支中添加扩展。 查看[Starter](../architecture/starter-architecture.md)和[Pro](../architecture/starter-develop-deploy-workflow.md)架构概述。

- 首先，从`master`分支创建`staging`分支，然后从`staging`分支进行开发。
- 对于Pro，从`Integration`环境创建一个开发分支。

您的帐户支持有限数量的![活动分支](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} （非活动）开发分支。 通过仅使用[!DNL Cloud Console]或Cloud CLI添加或删除分支来管理活动和不活动分支。 在删除分支之前，请先取消激活该分支，它仍保留在&#x200B;_环境_&#x200B;列表中，作为&#x200B;_不活动_。 您可以稍后重新激活分支，也可以[在环境设置中或使用云CLI删除分支](../dev-tools/cloud-cli-overview.md#)。

如果需要其他活动环境进行开发，请提交[支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

**添加分支**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 选择环境。

   >[!TIP]
   >
   >您的新分支是从此环境中克隆的。 选择与要创建的环境类似的父环境。

1. 单击&#x200B;**[!UICONTROL Branch]**。

   ![创建分支](../../assets/button-branch.png){width="150"}

1. 在&#x200B;_分支自……_&#x200B;表单中，输入分支名称。

   只有在环境名称中使用空格或大写字母时，环境&#x200B;_name_&#x200B;才会与环境&#x200B;_ID_&#x200B;不同。 环境ID由所有小写字母、数字和允许的符号组成。 环境名称中的大写字母在ID中转换为小写；环境名称中的空格转换为破折号。

   环境名称&#x200B;**不能**&#x200B;包含为Linux shell或正则表达式保留的字符。 禁止使用的字符包括大括号(`{ }`)、圆括号、星号(`*`)、尖括号(`>`)、&amp;符号(`&`)、% (<code>%</code>)，以及其他字符。

1. 选择&#x200B;**[!UICONTROL Environment type]**。

1. 单击&#x200B;**[!UICONTROL Create Branch]**。

1. 正在部署环境，请稍候。

   在部署期间，环境状态为&#x200B;**正在进行**。 成功部署后，状态将更改为&#x200B;**success**&#x200B;的绿色复选标记。

## 创建非活动分支

您无法从Adobe Commerce Cloud控制台或CLI创建非活动分支。 如果要创建非活动分支，请在Git存储库中创建它，并使用命令上的`environment.Parent`选项进行推送。

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## 删除环境

在删除环境之前，必须取消激活该环境。 环境处于非活动状态后，您可以将其删除。

**要停用环境**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 从导航栏&#x200B;_环境_&#x200B;列表中选择环境。

1. 单击顶部导航栏右侧的配置图标，打开环境设置。

1. 在&#x200B;_[!UICONTROL General]_&#x200B;选项卡上，向下滚动到&#x200B;_[!UICONTROL Deactivate environment]_&#x200B;部分，然后单击&#x200B;**[!UICONTROL Deactivate environment and delete data]**&#x200B;并按照说明操作。

## 同步环境

同步环境（或分支）与`git pull origin <parent>`相同。 您可以从父环境中同步更新的代码。 您可以通过[!DNL Cloud Console]将此功能用于所有入门和专业环境。

对于Pro计划，您可以从暂存和生产同步到`master`分支。 此同步仅提取和推送代码，而不提取数据。 要同步数据，请转储数据库数据并将其推送到另一个环境的数据库。 请参阅[迁移和部署静态文件和数据](/help/cloud-guide/deploy/staging-production.md#migrate-static-files)。

**同步环境**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 在环境列表中，单击要同步的分支的名称。

1. 单击（同步）。

   ![同步环境](../../assets/button-sync.png){width="150"}

1. 选择要同步的项目。

   - 替换数据 — （数据和文件）同步父分支中数据库和内容文件的更改。
   - 合并 — （代码）同步来自父分支的已更新代码。

   这还会生成一个CLI命令供您复制和使用。

1. 单击&#x200B;**同步**。

## 与父环境合并

合并环境（或分支）与`git push origin`相同。 您可以合并以将更新后的代码从环境推送到其父环境。 您可以将此代码合并到`master`。 您可以使用`merge`命令部署到暂存和生产环境。

**要与父环境合并**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 在环境列表中，单击要合并的分支的名称。

1. 单击（合并）。

   ![合并环境](../../assets/button-merge.png){width="150"}

1. 单击&#x200B;**合并**&#x200B;并确认操作。

## 查看日志

通过[!DNL Cloud Console]，您可以查看环境的各种日志，包括生成、部署和部署历史记录。

对于&#x200B;**Starter**，您可以查看生成和部署日志以及部署历史记录。 这些环境包括`master` （生产）分支以及从中创建的所有分支。

对于&#x200B;**Pro**，您可以在每个环境中查看以下日志：

- 集成 — 构建、部署和部署历史记录
- 暂存 — 构建日志和部署历史记录。 使用SSH登录到服务器以查看部署日志。
- 生产 — 构建日志和部署历史记录。 使用SSH登录到服务器以查看部署日志。

**要在[!DNL Cloud Console]**&#x200B;中查看日志：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 选择环境。

   环境视图提供了[活动列表](activity-stream.md)，该列表显示&#x200B;_最近_&#x200B;个事件，每个尝试的操作有一个条目，包括同步、合并、分支、备份等。 单击&#x200B;**全部**&#x200B;查看完整的部署历史记录。

1. 要查看生成日志，请选择帐户上每个部署记录的成功或失败链接。

>[!TIP]
>
>单击下拉列表的&#x200B;**过滤依据**&#x200B;图标，然后选择要查看的消息类型。

## 从专用Git存储库中提取代码

您在云基础架构上的Adobe Commerce项目可以包含来自私有Git存储库的代码。 例如，您可能拥有专用存储库中自定义模块或主题的代码。 为此，您必须将项目的公共SSH密钥添加到私有Git存储库并更新项目`composer.json`文件。

要向专用GitHub存储库添加部署密钥，您必须是该存储库的管理员。 GitHub允许您仅对一个存储库使用部署密钥。

如果您希望项目访问多个存储库，则可以将SSH密钥附加到自动用户帐户。 由于此帐户不是由用户使用，因此它称为[计算机用户](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)。 将计算机帐户添加为协作者，或者将计算机用户添加到具有存储库访问权限的团队。

>[!INFO]
>
>Adobe建议将此代码添加并合并到项目的Git存储库中。 如果不配置连接，则可能会遇到生成问题。

**要查找您的SSH公钥**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 单击顶部导航栏右侧的配置图标。

1. 在&#x200B;_项目设置_&#x200B;中，单击&#x200B;**[!UICONTROL Deploy Key]**。

1. 将部署密钥复制到剪贴板，以供在以下基于Git的方法之一中使用：

>[!BEGINTABS]

>[!TAB GitHub]

### 输入您的GitHub部署密钥

在GitHub上，部署密钥默认为只读。

**要输入项目公钥作为GitHub部署密钥**，请执行以下操作：

1. 以管理员身份登录到您的GitHub存储库。
1. 单击存储库&#x200B;**[!UICONTROL Settings]**&#x200B;选项卡。

   >[!NOTE]
   >
   >如果没有看到此选项，则表示您不是以存储库管理员的身份登录，并且无法完成此任务。 请咨询GitHub存储库管理员以执行此操作。

1. 在左侧导航栏的&#x200B;_设置_&#x200B;选项卡上，单击&#x200B;**[!UICONTROL Deploy Keys]**。
1. 单击&#x200B;**[!UICONTROL Add deploy key]**。
1. 按照提示操作。

在`composer.json`中，使用`<user>@<host>:<.git</code>`格式，如果使用非标准端口，则使用`ssh://<user>@<host>:<port>/<path>.git`。

>[!TAB 比特桶]

### 输入您的Bitbucket部署密钥

**要输入项目公钥作为Bitbucket部署密钥**，请执行以下操作：

1. 以管理员身份登录到您的Bitbucket存储库。
1. 在左侧导航中，单击&#x200B;**[!UICONTROL Settings]**。
1. 单击“常规”>**[!UICONTROL Deployment Keys]**。
1. 单击&#x200B;**[!UICONTROL Add Key]**。
1. 按照提示操作。

>[!TAB GitLab]

### 输入您的GitLab部署密钥

**要添加项目的公共SSH密钥作为GitLab部署密钥**：

1. 以所有者身份登录到您的GitLab存储库。
1. 验证是否已为您的项目启用&#x200B;_管道_&#x200B;选项：

   1. 在项目设置中，展开&#x200B;**[!UICONTROL Visibility, project, features, permissions]**&#x200B;部分。
   1. 如有必要，请单击&#x200B;**[!UICONTROL Pipelines]**&#x200B;以启用该选项。

1. 将公共SSH密钥添加到CI/CD设置。

   1. 在左侧导航中，单击设置> **[!UICONTROL CI / CD]**。
   1. 单击部署密钥&#x200B;**展开**&#x200B;以配置密钥。
   1. 在&#x200B;_部署密钥_&#x200B;表单中，将部署密钥名称添加到&#x200B;**[!UICONTROL Title]**&#x200B;字段，并将您的公共SSH密钥粘贴到&#x200B;**[!UICONTROL Key]**&#x200B;字段。
   1. 单击&#x200B;**[!UICONTROL Add Key]**&#x200B;保存配置。

>[!ENDTABS]

## 保护环境和分支机构的安全

您可以使用[!DNL Cloud Console]通过Web浏览器从任何位置访问您的项目和环境。 您可能已为生产环境、商店和站点设置了安全性。 本节将帮助您确保集成和暂存环境的安全，严格限制为开发人员、DBA等的安全。

>[!WARNING]
>
>**DO NOT**&#x200B;使用以下方法来保护Pro暂存环境和生产环境。 这破坏了快速缓存。 使用Adobe Commerce的Fastly CDN中提供的[阻止](../cdn/fastly-vcl-blocking.md)功能。

**保护环境**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 选择环境并单击导航栏上的配置图标。

1. 在环境设置&#x200B;_常规_&#x200B;选项卡上，单击&#x200B;**[!UICONTROL HTTP access control enabled]**&#x200B;的&#x200B;**开启**&#x200B;以启用安全访问。 您可以在凭据或IP地址之间进行选择以筛选访问权限。

1. 要按凭据筛选，请单击&#x200B;**[!UICONTROL Add Login]**，输入用户名和密码，然后单击&#x200B;**[!UICONTROL Add Login]**&#x200B;进行添加。

1. 要按IP地址过滤，请在包含`deny`或`allow`的列表中输入IP地址。 例如：

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. 单击&#x200B;**[!UICONTROL Save]**。 这将重新部署环境以更新安全和设置。 Adobe建议在完成安全设置后测试环境。
