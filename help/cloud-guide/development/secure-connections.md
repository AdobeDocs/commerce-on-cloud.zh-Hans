---
title: 安全连接
description: 了解如何将SSH密钥应用于云基础架构项目上的Adobe Commerce并登录到远程环境。
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: 9c0b4bea11abb2ce5644556ab3dadd361f8ff449
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 0%

---

# 与远程环境的安全连接

Secure Shell (SSH)是用于安全登录到远程服务器和系统的常用协议。 您可以使用SSH访问远程环境，以便管理Adobe Commerce应用程序和访问远程环境日志。 Adobe仅支持使用SSH公钥的安全FTP (sFTP)连接。 不支持FTP连接。

## 生成SSH密钥对

在需要访问项目源代码和环境的每台计算机和工作区上创建一个SSH密钥对。 SSH密钥允许您连接到GitHub来管理源代码并连接到云服务器，而无需持续提供用户名和密码。 有关创建SSH密钥对的进一步说明，请参阅[使用SSH连接到GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)。

- _公钥_&#x200B;可安全用于访问站点、SSH和sFTP。
- _私钥_&#x200B;在工作站上保持私有。

>[!CAUTION]
>
>**从不共享您的私钥。**&#x200B;请勿将其添加到票证、复制到聊天或附加到电子邮件。

## 向帐户添加SSH公钥

在向Adobe Commerce on cloud infrastructure帐户中添加或更新SSH公钥后，在您的帐户上[重新部署所有活动环境](https://experienceleague.adobe.com/zh-hans/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy)以安装密钥。

您可以使用以下方法之一将SSH密钥添加到您的帐户： Cloud CLI或[!DNL Cloud Console]。

>[!BEGINTABS]

>[!TAB CLI]

### 使用云CLI添加SSH密钥

1. 在本地工作站上，转到您的项目目录。

1. 登录项目：

   ```bash
   magento-cloud login
   ```

1. 添加公钥。

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>您可以使用Cloud CLI命令`ssh-key:list`和`ssh-key:delete`列出并删除SSH密钥。

>[!TAB 控制台]

### 使用[!DNL Cloud Console]添加SSH密钥

**要将SSH密钥添加到新项目**，请执行以下操作：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 单击&#x200B;**[!UICONTROL No SSH key]**。 此图标位于命令字段的右侧，当项目不包含SSH密钥时可见。

1. 在&#x200B;**公钥**&#x200B;字段中复制并粘贴公共SSH密钥的内容。

1. 按照其余提示操作。

**要向云配置文件添加SSH密钥**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 在右上角帐户菜单中，单击&#x200B;**我的个人资料**。

1. 在&#x200B;_SSH密钥_&#x200B;视图中，单击&#x200B;**添加公钥**。

1. 在&#x200B;_添加SSH密钥_&#x200B;表单中，为您的密钥提供&#x200B;**标题**，并将公共SSH密钥粘贴到&#x200B;**密钥**&#x200B;字段中。

1. 单击&#x200B;**保存**。

>[!ENDTABS]

## 连接到远程环境

您可以使用`magento-cloud` CLI或SSH命令连接到远程环境。 `magento-cloud` CLI命令只能在Starter和Pro集成环境中使用。

### 使用Cloud CLI

**登录到远程集成环境**：

1. 在本地工作站上，转到您的项目目录。

1. 列出该项目中的环境。

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### 使用SSH命令

[!DNL Cloud Console]包含每个环境的Web和SSH访问命令列表。

**复制SSH命令**：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 从&#x200B;_所有项目_&#x200B;列表中选择一个项目。

1. 选择环境。

1. 单击&#x200B;**[!UICONTROL SSH]**。

1. 在&#x200B;_SSH_&#x200B;选项卡中，单击“复制”按钮以将完整的SSH命令复制到剪贴板。

1. 打开终端并粘贴SSH命令以创建连接。

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>对于Pro暂存和生产环境，SSH命令可能如下所示：
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

云基础架构上的Adobe Commerce支持使用采用SSH身份验证的sFTP（安全FTP）访问您的环境。 使用支持sFTP的SSH密钥身份验证的客户端并使用公共SSH密钥。 必须将公共SSH密钥添加到目标环境中。 对于入门级环境和专业级集成环境，您可以[通过 [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface)添加它。

只读sFTP连接&#x200B;_不支持_；默认情况下，为sFTP访问权限提供了&#x200B;_写入_&#x200B;权限。

配置sFTP时，请使用SSH访问环境命令中的信息： `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **用户名**： SSH访问目标中`@`之前的所有内容。
- **密码**：您不需要sFTP密码。 sFTP访问使用SSH密钥身份验证。
- **主机**： SSH访问中`@`之后的所有内容。
- **端口**： 22，这是默认的SSH端口。
- **SSH**&#x200B;私钥：如有必要，请向sFTP客户端提供私钥的位置。 默认情况下，私钥存储在`~/.ssh`目录中。

根据客户端的不同，可能需要其他选项来完成sFTP的SSH身份验证。 查看所选客户端的文档。

对于&#x200B;**入门环境和Pro集成环境**，您可能还想考虑[添加`mount`](../application/properties.md#mounts)以访问特定目录。 您要将装载添加到`.magento.app.yaml`文件中。 有关可写目录的列表，请参阅[项目结构](../project/file-structure.md)。 此挂载点仅适用于这些环境。

对于&#x200B;**Pro暂存和生产环境**，如果您没有该环境的SSH访问权限，则必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以请求sFTP访问权限和装入点以访问特定文件夹，例如`pub/media`。

>[!NOTE]
>对于Pro暂存和生产环境，如果sFTP连接针对的是&#x200B;_通用_、**非**&#x200B;的用户，需要将[添加到云项目](../project/user-access.md)，则您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)，并附加其&#x200B;**公共**&#x200B;密钥。 **从不提供您的私有SSH密钥。**

## SSH隧道

您可以使用SSH隧道从本地开发环境连接到服务，就像该服务在本地一样。 在隧道之前，请配置[SSH](#add-an-ssh-public-key-to-your-account)。

使用终端应用程序登录并发出命令。

```bash
magento-cloud login
```

使用验证是否打开了任何通道。

```bash
magento-cloud tunnel:list
```

要建立通道，您必须知道[应用程序名称](../application/properties.md#name)。 您可以使用CLI检查应用程序名称：

```bash
magento-cloud apps
```

### 设置SSH通道

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

例如，要在具有名为`sprint5`的应用程序的项目中打开到`mymagento`分支的通道，请输入

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

示例响应：

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**要显示有关通道的信息**：

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### 连接到服务

建立SSH隧道后，您可以像在本地运行一样连接到服务。 例如，要连接到数据库，请使用以下命令：

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```

#### 获取MySQL凭据

从`database`环境变量中的`$MAGENTO_CLOUD_RELATIONSHIPS`属性检索MySQL登录凭据。 有关在本地或远程环境中检索信息的说明，请参阅[服务关系](../services/services-yaml.md#service-relationships)。
