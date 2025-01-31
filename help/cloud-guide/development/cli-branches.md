---
title: 使用CLI管理分支
description: 了解如何使用Cloud CLI在云基础架构上管理Adobe Commerce的环境分支。
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# 使用CLI管理分支

要安装`magento-cloud` CLI，请参阅[云CLI引用](../dev-tools/cloud-cli-overview.md)。 安装`magento-cloud` CLI并设置SSH密钥以远程访问云基础架构后，可以使用`magento-cloud` CLI命令管理项目的环境。 有关环境架构的信息，请参阅[入门架构](../architecture/starter-architecture.md)或[专业架构](../architecture/pro-architecture.md)。

要使用[!DNL Cloud Console]管理分支和环境，请参阅[使用 [!DNL Cloud Console]](../project/console-branches.md)管理分支。

## 使用CLI命令

`magento-cloud` CLI命令与Git命令类似。 您可以使用它们连接到您的项目并管理环境。 虽然可以从任何目录运行命令，但建议从项目目录运行这些命令。 从项目目录运行时，可以省略`-p <project-ID>`参数。 请参阅[云CLI引用](../dev-tools/cloud-cli-overview.md)。

## 克隆项目

以下说明使用`magento-cloud` CLI命令和Git命令的组合将项目克隆到本地工作站。 要查看`magento-cloud` CLI命令的完整列表，请使用`magento-cloud list`命令。

>[!IMPORTANT]
>
>某些Git命令无法在Adobe Commerce中完成云基础架构项目上的操作。 例如，您可以使用Git命令创建分支，但无法创建和激活新环境。 必须使用`magento-cloud environment:branch <branch-name>`命令创建环境，才能使环境变为&#x200B;_活动_。 或者，您可以使用[!DNL Cloud Console]创建活动环境。 请参阅[云CLI引用](../dev-tools/cloud-cli-overview.md#git-commands)。

**克隆项目`master`环境**：

1. 使用[文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)帐户登录到本地工作站。

1. 更改为Web服务器或虚拟主机&#x200B;_docroot_&#x200B;目录。

1. 使用`magento-cloud` CLI登录。

   ```bash
   magento-cloud login
   ```

1. 列出您的项目。

   ```bash
   magento-cloud project:list
   ```

1. 克隆项目。

   ```bash
   magento-cloud project:get <project-ID>
   ```

   出现提示时，提供目录名称。

1. 更改为`magento2`目录。

1. 列出项目的可用环境。

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >`magento-cloud environment:list`命令显示环境层次结构，而`git branch`命令不显示环境层次结构。

1. 获取远程分支。

   ```bash
   git fetch origin
   ```

1. 提取更新的代码。

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>有关在云基础架构上使用基于Git的托管服务与Adobe Commerce的信息，请参阅[集成](../integrations/overview.md)。

## 创建用于开发的分支

在克隆项目并更新Adobe Commerce管理员帐户配置后，可以分支进行开发。 如前所述，您必须使用`magento-cloud environment:branch <branch-name>`命令或[!DNL Cloud Console]创建环境，以使环境变为&#x200B;_活动_。

- 对于[简易版](../architecture/starter-develop-deploy-workflow.md#clone-and-branch)，请考虑为`staging`创建分支，然后基于`staging`分支创建开发分支。
- 对于[Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow)，根据`Integration`分支创建开发分支。

**要创建开发分支**：

1. 在本地工作站上，转到您的项目目录。

1. 根据为您的项目工作流推荐的分支创建环境。

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 更新依赖关系。

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_可选_]&#x200B;创建环境的[备份](../storage/snapshots.md)。

### 合并分支

完成开发后，将此分支合并到父级：

1. 提交和推送代码更改：

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 与父环境合并：

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 删除环境

仅当您确定不再需要某个环境时，才将其删除。 删除环境后无法恢复该环境。

>[!WARNING]
>
>您无法删除任何项目的`master`分支。

您必须是项目管理员、环境管理员或帐户所有者才能执行此任务。 请参阅[管理用户对云项目的访问权限](../project/user-access.md)。

删除环境时，该环境设置为&#x200B;_不活动_。 该代码仍可在Git分支中使用，但不再包含服务或数据库。 要完全删除环境，您还必须删除相应的远程Git分支。

**要删除环境**：

1. 在本地工作站上，转到您的项目目录。

1. 从远程服务器获取更新。

   ```bash
   git fetch
   ```

1. 删除环境分支。

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   或者，您也可以通过将多个环境ID添加到delete命令来一次删除多个环境。

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. 响应提示删除本地环境和相应的远程环境。

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   删除环境会使其处于&#x200B;_不活动_&#x200B;状态。

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   删除远程Git分支会从项目中删除环境。

1. 等待环境删除。

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>要激活非活动环境，请使用`magento-cloud environment:activate`命令。

## 与远程环境交互

在您[设置SSH密钥](../development/secure-connections.md)后，您可以[从本地工作区连接到远程环境](../development/secure-connections.md#connect-to-a-remote-environment)，并与项目服务交互并修改设置。
