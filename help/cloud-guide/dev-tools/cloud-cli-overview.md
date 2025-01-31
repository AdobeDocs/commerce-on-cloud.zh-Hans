---
title: Cloud CLI
description: 了解magento-cloud CLI以及它如何帮助您在云基础架构项目上管理Adobe Commerce的本地开发环境。
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# Cloud CLI

通过`magento-cloud` CLI工具，开发人员和系统管理员能够管理Cloud项目和环境、执行例程并在本地运行自动化任务。 `magento-cloud` CLI扩展了[[!DNL Cloud Console]](../../get-started/cloud-console.md)的特性和功能。 在本地工作站上安装`magento-cloud` CLI后，您可以在云基础架构Starter和Pro集成环境中使用它来管理Adobe Commerce。

>[!NOTE]
>
>这是本地工具，不能使用此方法安装在云环境（只读）上。 您只能通过&#x200B;**部署工作流**&#x200B;在云环境中安装模块
>- [专业部署工作流](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [入门部署工作流](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**安装`magento-cloud` CLI**：

1. 在您的&#x200B;_本地工作站_&#x200B;上，切换到您打算克隆云项目且[文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)具有&#x200B;_写入_&#x200B;访问权限的目录。

1. 安装`magento-cloud` CLI。

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. 将`magento-cloud` CLI添加到bash配置文件。

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 重新加载更新的bash配置文件。

   ```bash
   . ~/.bash_profile
   ```

1. 要启动CLI，请调用`magento-cloud`，并在出现提示时输入您的Cloud帐户凭据。

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. 验证`magento-cloud`命令是否位于您的路径中。 以下示例列出了可用的命令。

   ```bash
   magento-cloud list
   ```

## 常用命令

Adobe设计了这些命令以管理Cloud集成环境，并建议从项目目录运行`magento-cloud` CLI，以便可以省略`-p <project-ID>`参数。

以下常用的`magento-cloud` CLI命令列表仅包含必需选项。 您可以将`--help`选项与任何命令一起使用，以查看更多信息。

| 命令 | 描述 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | 登录到项目。 |
| `magento-cloud list` | 列出CLI工具的可用命令。 |
| `magento-cloud environment:list` | 列出当前项目中的环境。 |
| `magento-cloud environment:checkout` | 查看现有环境。 |
| `magento-cloud environment:merge -e` | 将此环境中的更改与其父级合并。 |
| `magento-cloud variables` | 此环境中的列表变量。 |
| `magento-cloud ssh` | 使用SSH连接到远程环境。 |
| `magento-cloud url` | 在浏览器中打开Adobe Commerce店面。 |
| `magento-cloud web` | 打开[!DNL Cloud Console]。 |

## 环境命令

只有在环境名称中使用空格或大写字母时，环境&#x200B;_name_&#x200B;才会与环境&#x200B;_ID_&#x200B;不同。 环境ID由所有小写字母、数字和允许的符号组成。 环境名称中的大写字母在ID中转换为小写；环境名称中的空格转换为破折号。

环境名称&#x200B;_不能_&#x200B;包含为Linux shell或正则表达式保留的字符。 禁止使用的字符包括大括号(`{ }`)、圆括号(`*`)、尖括号(`< >`)、&amp;符号(`&`)、% (`%`)和其他字符。

`magento-cloud environment:list`命令显示环境层次结构，而`git branch`则不显示。 如果您有任何嵌套环境，请使用以下内容：

```bash
magento-cloud environment:list
```

### 重新部署环境

不使用推送触发重新部署。 验证并确认要重新部署的环境。 如果内部版本处于待处理状态，请勿使用重新部署。

```bash
magento-cloud environment:redeploy
```

示例响应：

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git命令

您可能会注意到其中的一些命令与Git命令类似。 `magento-cloud`命令直接连接到具有其他功能的基于Git的云项目。 如果您在不使用`magento-cloud` CLI的情况下创建分支，则它不会“激活”，并且在将更改推送到远程环境时不会自动生成。 `magento-cloud` CLI命令包含激活。

要创建分支，请使用`magento-cloud`命令以激活该分支。

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

对于分支状态：

- 使用`magento-cloud env`命令查看环境分支的列表及其状态：活动或非活动。
- 使用`magento-cloud environment:activate`命令激活环境分支。

推送空的Git承诺以触发部署。 例如：

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

某些操作（如添加用户）不会导致部署。

### 创建环境分支

以下步骤演示了如何交替使用CLI和Git命令来管理本地环境：

1. 在本地工作站上，转到您的项目目录。

1. 切换到[文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)。

1. 登录到您的项目。

   ```bash
   magento-cloud login
   ```

1. 列出您的项目。

   ```bash
   magento-cloud project:list
   ```

1. 列出项目中的环境。 每个环境都包含一个活动Git分支，其中包含代码、数据库、环境变量、配置和服务。

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >使用`magento-cloud environment:list`命令很重要，因为它显示环境层次结构，而`git branch`命令不显示环境层次结构。

1. 获取来源分支以获取最新代码。

   ```bash
   git fetch origin
   ```

1. 结帐或切换到特定分支和环境。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git命令仅检查Git分支。 `magento-cloud checkout`命令签出分支并切换到活动环境。

   >[!TIP]
   >
   >您可以使用`magento-cloud environment:branch <environment-name> <parent-environment-ID>`命令语法创建环境分支。 创建和激活环境分支可能需要一些额外的时间。

1. 使用环境ID将任何更新的代码拉入到您的本地。 如果环境分支是新的，则不必执行此操作。

   ```bash
   git pull origin <environment-ID>
   ```

1. （_可选_）创建环境的[快照](../storage/snapshots.md)作为备份。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## 更新CLI

`magento-cloud` CLI会在您登录时检查可用更新，但您可以使用`self:update`命令检查更新。 如果有可用的更新，请按照说明更新CLI。

如果`magento-cloud` CLI是最新的，您会看到以下响应：

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
