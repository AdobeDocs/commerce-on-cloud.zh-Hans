---
title: 备份管理
description: 了解如何在Cloud Infrastructure项目中手动创建和恢复Adobe Commerce的备份。
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: b9bbbb9b83ed995951feaa9391015f02a9661206
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 0%

---

# 备份管理

您可以随时使用[!DNL Cloud Console]中的&#x200B;**[!UICONTROL Backup]**&#x200B;按钮或使用`magento-cloud snapshot:create`命令执行活动Starter环境的手动备份。

备份或&#x200B;_快照_&#x200B;是对环境数据的完整备份，包括来自正在运行的服务（MySQL数据库）的所有永久数据以及存储在装入卷(var、pub/media、app/etc)上的任何文件。 快照&#x200B;_不_&#x200B;包含代码，因为代码已存储在基于Git的存储库中。 无法下载快照的副本。

>[!WARNING]
>
>虽然备份通常包含已装入目录的内容，包括如`pub/media`之类的公共Web目录，但不要将备份输出文件移动到如`pub/media`或`pub/static`之类的公共Web目录。

备份/快照功能&#x200B;**不**&#x200B;适用于Pro暂存和生产环境，默认情况下这些环境接收用于灾难恢复的常规备份。 有关详细信息，请参阅[专业备份和灾难恢复](../architecture/pro-architecture.md#backup-and-disaster-recovery)。 与Pro暂存环境和生产环境中的自动实时备份不同，备份&#x200B;**不是**&#x200B;自动。 _您有_&#x200B;责任手动创建备份或设置cron作业以定期创建Starter或Pro集成环境的备份。

## 创建手动备份

您可以从[!DNL Cloud Console]创建任何活动Starter环境和集成Pro环境的手动备份，或从Cloud CLI创建快照。 您必须具有环境的[管理员角色](../project/user-access.md)。

**创建Pro环境的数据库备份**：
要为任何Pro环境（包括暂存环境和生产环境）创建数据库转储，请参阅[创建数据库转储](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud)知识库文章。

**要使用[!DNL Cloud Console]**&#x200B;创建任何Starter环境的备份：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。
1. 从项目导航栏中选择一个环境。 环境必须处于活动状态。
1. 在&#x200B;_备份_&#x200B;视图中，单击&#x200B;**[!UICONTROL Backup]**。 此选项不适用于Pro环境。

   ![备份](../../assets/button-backup.png){width="150"}

**要使用[!DNL Cloud Console]**&#x200B;创建集成环境的备份：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。
1. 从项目导航栏中选择集成/开发环境。 环境必须处于活动状态。
1. 选择右上角菜单中的&#x200B;**[!UICONTROL Backup]**&#x200B;选项。 此选项适用于Starter和Pro环境。
1. 单击&#x200B;**[!UICONTROL Yes]**&#x200B;按钮。

**要使用`magento-cloud` CLI创建快照**：

1. 在本地工作站上，转到您的项目目录。
1. 将环境分支签出到快照。
1. 创建快照。

   ```bash
   magento-cloud snapshot:create --live
   ```

   或者，您可以使用`magento-cloud backup`短命令。 `--live`选项使环境保持运行以避免停机。 要获取选项的完整列表，请输入`magento-cloud snapshot:create --help`。

   示例响应：

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. 验证最新的快照。

   ```bash
   magento-cloud snapshot:list
   ```

   该列表返回有关快照状态的信息：

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 恢复手动备份

您必须具有环境的[管理员访问权限](../project/user-access.md)。 您最多有&#x200B;**7天**&#x200B;到&#x200B;_还原_&#x200B;手动备份。 恢复备份不会更改当前Git分支的代码。 以这种方式恢复备份不适用于Pro暂存和生产环境；请参阅[专业备份和灾难恢复](../architecture/pro-architecture.md#backup-and-disaster-recovery)。

恢复时间因数据库的大小而异：

- 大型数据库（200 GB以上）可能需要5小时
- 中型数据库(150 GB)可能需要2.5小时
- 小型数据库(60 GB)可能需要1小时

>[!TIP]
>
>无需备份即可恢复：
>
>- 若要回滚到以前的代码或移除环境中添加的扩展，请参阅[回滚代码](#roll-back-code)。
>- 要还原&#x200B;_没有_&#x200B;备份的不稳定环境，请参阅[还原环境](../development/restore-environment.md)。

**要使用[!DNL Cloud Console]**&#x200B;还原备份：

1. 登录到[[!DNL Cloud Console]](https://console.adobecommerce.com)。
1. 从项目导航栏中选择一个环境。
1. 在&#x200B;_备份_&#x200B;视图中，从&#x200B;_存储_&#x200B;列表中选择备份。 备份功能&#x200B;**不**&#x200B;适用于Pro环境。
1. 在![更多](../../assets/icon-more.png){width="32"} （_更多_）菜单中，单击&#x200B;**还原**。
1. 查看从备份信息还原，然后单击&#x200B;**是，还原**。

**要使用Cloud CLI还原快照**：

1. 在本地工作站上，转到您的项目目录。
1. 签出要恢复的环境分支。
1. 列出所有可用的快照。

   ```bash
   magento-cloud snapshot:list
   ```

   该列表返回有关可用快照的信息：

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. 使用列表中的快照ID恢复快照。

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## 恢复灾难恢复快照

要在Pro暂存和生产环境中还原灾难恢复快照，请[直接从服务器](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3)导入数据库转储。

## 回滚代码

备份和快照&#x200B;_不_&#x200B;包含您的代码副本。 您的代码已存储在基于Git的存储库中，因此您可以使用基于Git的命令来回滚（或还原）代码。 例如，使用`git log --oneline`滚动浏览以前的提交；然后使用[`git revert`](https://git-scm.com/docs/git-revert)从特定提交还原代码。

此外，您可以选择将代码存储在&#x200B;_非活动_&#x200B;分支中。 使用git命令而不是使用`magento-cloud`命令来创建分支。 请参阅Cloud CLI主题中的关于[Git命令](../dev-tools/cloud-cli-overview.md#git-commands)。
