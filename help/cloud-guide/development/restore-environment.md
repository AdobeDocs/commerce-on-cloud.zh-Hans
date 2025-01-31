---
title: 恢复环境
description: 了解如何从云基础架构项目中卸载Adobe Commerce应用程序并将环境还原到稳定状态。
role: Developer
topic: Development
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# 恢复环境

如果您在集成环境中遇到问题，并且没有[有效的备份](../storage/snapshots.md)，或者希望将环境重置为空白板，则可以使用以下方法之一还原/重置环境：

- 重置或还原Git分支中的代码
- 卸载[!DNL Commerce]应用程序
- 强制重新部署
- 手动重置数据库

{{stuck-deployment-tip}}

## 重置Git分支

重置Git分支会将代码恢复到以前的稳定状态。

**重置分支**：

1. 在本地工作站上，转到您的项目目录。

1. 查看Git提交历史记录。 使用`--oneline`在一行中显示缩写提交：

   ```bash
   git log --oneline
   ```

   示例响应：

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. 选择表示代码的最后一个已知稳定状态的提交哈希。

   要将分支重置为其初始初始化状态，请查找创建分支的第一个提交。 您可以使用`--reverse`以反向时间顺序显示历史记录。

1. 使用硬重置选项重置分支。 使用此命令时请务必小心，因为它会丢弃自选定的提交后所做的所有更改。

   ```bash
   git reset --hard <commit>
   ```

1. 推送更改以触发重新部署，从而重新安装Adobe Commerce。

   ```bash
   git push --force <origin> <branch>
   ```

## 卸载Commerce

通过还原数据库、删除部署配置并清除`var/`子目录，卸载[!DNL Commerce]应用程序可将环境返回到原始状态。 本指导还会将您的Git分支重置为以前的稳定状态。 如果您最近没有备份，但可以使用SSH访问远程环境，请按照以下步骤恢复环境：

- 禁用配置管理
- 卸载Adobe Commerce
- 重置Git分支

卸载Adobe Commerce软件将删除并还原数据库，删除部署配置，并清除`var/`子目录。 禁用[配置管理](../store/store-settings.md)很重要，这样它就不会在下次部署期间自动应用以前的配置设置。 确保您的`app/etc/`目录不包含`config.php`文件。

**要卸载Adobe Commerce软件**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 删除配置文件。
   - 对于Adobe Commerce 2.2及更高版本：

     ```bash
     rm app/etc/config.php
     ```

   - 对于Adobe Commerce 2.1：

     ```bash
     rm app/etc/config.local.php
     ```

1. 卸载Adobe Commerce应用程序。

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. 确认Adobe Commerce已成功卸载。

   将显示以下消息以确认卸载成功：

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. 清除`var/`子目录。

   ```bash
   rm -rf var/*
   ```

1. 注销。

>[!TIP]
>
>或者，清除生成缓存是一种良好做法。
>
>```bash
>magento-cloud project:clear-build-cache
>```

## 强制重新部署

如果您尝试卸载Adobe Commerce但部署仍然失败，则可以尝试手动强制重新部署。

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## 重置数据库

如果您尝试卸载Adobe Commerce，但命令失败或无法完成，则可以手动重置数据库。

**要重置数据库**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 连接到数据库。

   ```bash
   mysql -h database.internal
   ```

1. 删除`main`数据库。

   ```shell
   drop database main;
   ```

1. 创建空的`main`数据库。

   ```shell
   create database main;
   ```

1. 删除以下配置文件。

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. 注销并触发重新部署。

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
