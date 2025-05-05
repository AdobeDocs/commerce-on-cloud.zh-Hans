---
title: 升级项目的最佳实践
description: 查看升级项目文件的最佳实践列表。
feature: Cloud, Best Practices, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 升级项目的最佳实践

遵循生成和部署的最佳实践，并使用[升级和修补程序](../development/commerce-version.md)工作流升级您的应用程序。 请遵循以下准则来计划升级和升级后的工作：

- **备份项目** — 在升级Adobe Commerce和任何第三方或自定义扩展之前，请在集成、暂存和生产环境中备份数据库。 请参阅[备份数据库](../development/commerce-version.md#project-backup)。

- **检查兼容性问题**-

   - 确保任何自定义主题与新的Adobe Commerce版本兼容

   - 升级第三方扩展和自定义扩展后，请使用`magento-cloud local:build`命令在部署之前验证编辑器依赖项。

   - 查看Adobe Commerce发行说明和扩展文档，确保您已实施解决与升级Adobe Commerce版本和扩展相关的已知功能问题和错误所需的任何解决方法或配置更改。

   - 确保安装的服务版本与新的Adobe Commerce版本兼容，并根据需要升级服务。 请参阅[服务](../services/services-yaml.md)。

   - 测试数据库，以解决Adobe Commerce版本和扩展更新所引发的任何问题。

   - 在部署到远程环境之前，对特定于环境的设置进行任何所需的更新。

   - 确保搜索服务版本与PHP客户端版本兼容。 请参阅[设置Elasticsearch](../services/elasticsearch.md)或[设置OpenSearch](../services/opensearch.md)。

- **检查远程环境中的数据库连接和可用存储**-

   - 使用SSH登录到远程服务器并验证与MySQL数据库的连接。 请参阅[连接到数据库](../services/mysql.md#connect-to-the-database)。

   - 验证远程环境中的可用存储 — 使用`disk free`命令查看和管理云环境中的可用磁盘空间。 请参阅[管理磁盘空间](../storage/manage-disk-space.md)。

      - 检查已升级数据库的大小，并验证`services.yaml`文件是否分配了足够的磁盘空间。

      - 释放磁盘空间 — 清除缓存，并在部署之前清除`/log`和`/tmp`目录。

- **在部署到暂存环境之前，在本地环境和集成环境中规划并执行一次成功的升级** — 在升级之后，测试您的部署并解决任何问题。

- **将代码合并到暂存环境，然后合并到生产环境** — 测试并解决暂存环境中的任何问题，然后再将更改推送到生产环境。

- **完成升级后任务**-

   - 使用SSH登录到远程服务器并验证以下内容：

      - 检查索引器状态并根据需要重新索引。 请参阅&#x200B;_配置指南_&#x200B;中的[管理索引器](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=zh-Hans)。

      - 检查Adobe Commerce数据库中的`cron`日志和`cron_schedule`表以验证cron状态，并根据需要重新运行cron作业。
请参阅_配置指南_&#x200B;中的[日志记录](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=zh-Hans#logging)。

   - 在暂存环境和生产环境中完成升级后用户验收测试UAT，并修复与第三方和自定义扩展升级相关的任何问题。
