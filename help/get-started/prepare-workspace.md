---
title: 准备开发
description: 收集凭据并了解可用于设置开发工作区的工具，以便在云基础架构项目上与Commerce一起使用。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# 准备开发

无论您是Commerce的新手还是现有Commerce所有者（迁移到云基础架构），请按照以下步骤为您的Cloud项目准备开发工作区。 如果您已完成其中的一些步骤或拥有现有的Adobe Commerce开发人员环境，请查看以下各项的预期结果，然后继续下一步骤。 某些配置和工作流程与典型的内部部署安装不同。

## 凭据

在设置工作区之前，请收集以下密钥和帐户访问权限：

- **身份验证密钥（编辑器密钥）**

  身份验证密钥是32个字符的身份验证令牌，它们提供对Adobe Commerce Composer存储库(`repo.magento.com`)以及应用程序开发所需的任何其他Git服务（如GitHub）的安全访问。 您的帐户可以有多个身份验证密钥。 对于工作区设置，请从代码存储库的一个特定键开始。 如果您没有任何密钥，请与项目所有者联系，或自行创建[身份验证密钥](../cloud-guide/development/authentication-keys.md)。

- **云项目帐户**

  项目所有者应邀请您访问Adobe Commerce on cloud infrastructure项目。 收到电子邮件邀请后，单击链接并按照提示创建帐户。 请参阅[入门](onboarding.md)。

- **Adobe Commerce加密密钥**

  仅导入现有系统时，捕获用于保护您对数据库的访问和数据的加密密钥。 有关此密钥的详细信息，请参阅[解决加密密钥的问题](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)

## 开发人员工具

- **安装云CLI**

  安装`magento-cloud` CLI，以便您可以管理云环境和运行自动化任务。 有关安装说明，请参阅[云CLI](../cloud-guide/dev-tools/cloud-cli-overview.md)。

- **安装Docker以进行本地开发和测试**

  或者，使用Docker环境在云基础架构`integration`环境中模拟Commerce以进行本地开发。 有三个基本组件：Adobe Commerce v2模板、Docker撰写和`ece-tools`包。

   - [Docker体系结构和常用命令](../cloud-guide/dev-tools/cloud-docker.md)
   - [Launch Docker开发环境](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [ECE-Tools包](../cloud-guide/dev-tools/package-overview.md)

- **集成基于Git的服务**

  或者，在云基础架构上将基于Git的托管服务（如GitHub或GitLab）与Adobe Commerce集成。 查看[集成](../cloud-guide/integrations/overview.md)。

## 项目代码

安全连接对于与远程环境交互至关重要。 对于新项目，[登录到 [!DNL Cloud Console]](https://console.adobecommerce.com)，然后单击&#x200B;**[!UICONTROL No SSH key]**。 此图标位于命令字段的右侧，当项目不包含SSH密钥时可见。 请参阅[安全连接](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account)。

**要将代码库克隆到本地工作站**：

1. 在[[!DNL Cloud Console]](https://console.adobecommerce.com)中，单击&#x200B;**[!UICONTROL code]**&#x200B;并选择&#x200B;**[!UICONTROL Git]**&#x200B;选项卡。

   ![克隆代码](../assets/ui-git-code.png){width="450"}

1. 复制提供的`git clone ...`命令。

1. 在终端中，创建并更改工作目录。

1. 粘贴并运行`git clone ...`命令。

>[!TIP]
>
>Adobe使用模板存储库来配置初始项目环境，模板存储库中包含适用于特定版本Adobe Commerce的包说明。 查看[项目文件结构](../cloud-guide/project/file-structure.md)主题，并了解有关重要项目文件和云模板的更多信息。
