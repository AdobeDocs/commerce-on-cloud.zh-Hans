---
title: 备份数据库
description: 了解如何使用ECE-tools为云基础架构项目上的Adobe Commerce创建数据库备份。
feature: Cloud, Iaas, Storage
exl-id: 351f7691-3153-4b8a-83af-8b8895b93d98
source-git-commit: 3a3b0cd6e28f3e6ed3521a86f7c7c8868be0cf83
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 备份数据库

您可以使用`ece-tools db-dump`命令创建数据库的副本，而无需从服务和装载中捕获所有环境数据。 默认情况下，此命令在`app/var/`目录中为环境配置中指定的所有数据库连接创建备份。 数据库转储操作将应用程序切换到维护模式，停止使用者队列进程，并在转储开始之前禁用cron作业。

请考虑以下数据库转储准则：

- 对于生产环境，Adobe建议在非高峰时间完成数据库转储操作，以最大程度地减少站点处于维护模式时发生的服务中断。
- 如果在转储操作过程中发生错误，该命令将删除转储文件以节省磁盘空间。 查看日志以了解详细信息(`var/log/cloud.log`)。
- 对于Pro Production环境，此命令仅从三个高可用性节点中的&#x200B;_一个_&#x200B;转储，因此可能不会复制在转储期间写入其他节点的生产数据。 该命令会生成一个`var/dbdump.lock`文件，以防止该命令在多个节点上运行。
- 对于所有环境服务的备份，Adobe建议创建[备份](snapshots.md)。

可以通过将数据库名称附加到命令来选择备份多个数据库。 以下示例备份两个数据库： `main`和`sales`：

```bash
php vendor/bin/ece-tools db-dump main sales
```

使用`php vendor/bin/ece-tools db-dump --help`命令获取更多选项：

- `--dump-directory=<dir>` — 选择数据库转储的目标目录。 **不要选择公共Web目录，如`pub/media`或`pub/static`**。
- `--remove-definers` — 从数据库转储中删除DEFINER语句。

**要在暂存或生产环境中创建数据库转储**：

1. [使用SSH登录或创建通道以连接到包含要复制数据库的远程环境](../development/secure-connections.md)。

1. 列出环境关系并记下数据库登录信息。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. 创建数据库的备份。 要为数据库转储选择目标目录，请使用`--dump-directory`选项。

   >[!WARNING]
   >
   >如果指定目标目录，请不要选择公共Web目录，如`pub/media`或`pub/static`。

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   示例响应：

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /app/qxmtlseakof6y/var/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. `db-dump`命令在远程项目目录中创建`dump-<timestamp>.sql.gz`存档文件。

>[!TIP]
>
>如果要将此数据推送到特定环境，请参阅[迁移数据和静态文件](../deploy/staging-production.md#migrate-static-files)。
