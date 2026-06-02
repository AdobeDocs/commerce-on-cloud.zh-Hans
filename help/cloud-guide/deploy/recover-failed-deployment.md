---
title: 从组件故障中恢复
description: 了解如果组件无法在云基础架构上的Adobe Commerce中正确部署，如何进行恢复。
feature: Cloud, Deploy
exl-id: f5e79366-5548-40dd-ac2a-56dc84c5d4e2
TQID: https://experienceleague.adobe.com/1krotAWilGcnp3OUMk-fl5iHFObGpWFUC8EpYSPjQfU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 0%

---

# 从组件故障中恢复

本主题讨论在组件无法正确部署时如何恢复。 典型示例包括具有远程环境不满足的依赖项的组件，例如不兼容的PHP版本。

您可以通过以下任一方式从失败的部署中恢复：

- [恢复备份](../storage/snapshots.md#restore-a-snapshot)
- 从以前的更改中清除项目和代码并重新部署

## 清理、删除和重新部署

要从上一个部署中清理，请标识已添加或已更新的组件，然后移除该组件。 首先，登录到远程环境并手动清除`var`目录的内容。 然后从`composer.json`文件中移除该组件并重新部署环境。

**要清除`var`目录**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 清除`var`目录。

   ```shell
   rm -rf var/*
   ```

1. 注销。

**要删除组件**：

1. 在本地工作站上，转到您的项目目录。

1. 清除缓存。

   ```bash
   composer clear-cache
   ```

1. 从`composer.json`文件中删除组件。

   ```bash
   composer remove <component-name>:<version>
   ```

   如果显示以下消息，则无需再执行任何操作：

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. 正在更新依赖关系，请稍候。

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

在[还原环境](../development/restore-environment.md)中查看有关在不备份的情况下还原环境的详细信息。

{{stuck-deployment-tip}}
