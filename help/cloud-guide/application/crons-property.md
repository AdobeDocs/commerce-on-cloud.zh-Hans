---
title: Crons属性
description: 请参阅有关如何在 [!DNL Commerce] 应用程序配置文件中配置“crons”属性的示例。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Crons属性

Adobe Commerce使用`crons`属性来计划重复活动。 它非常适合安排在一天中的特定时间运行特定任务。 由于只读环境的性质，在云基础架构项目上的Adobe Commerce的Web实例上，一次只能运行一个cron作业。 最佳做法是将长时间运行的任务划分为较小的排队任务。 或者，您可以构建[辅助实例](workers-property.md)。

Adobe建议您以[文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)的身份运行`crons`。 请&#x200B;_不要_&#x200B;以`root`或Web服务器用户的身份运行`crons`。

此配置不同于Adobe Commerce的内部部署，后者有多个默认cron作业。 请参阅&#x200B;_配置指南_&#x200B;中的[配置cron作业](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html)。

## 设置cron作业

`crons`属性描述按计划触发的进程。 每个作业都需要一个名称和以下选项：

- `spec` — 用于计划的cron表达式。
- `cmd` — 要在`start`和`stop`上运行的命令。
- `shutdown_timeout` — （_可选_）如果取消了cron作业，这是发送SIGKILL信号以停止该作业或进程之前的秒数。 默认值为10秒。
- `timeout` — （_可选_） cron作业在超时之前可以运行的最长时间。 默认为允许的最大值86400秒（24小时）。

默认情况下，每个Commerce云项目在`.magento.app.yaml`文件中都有以下默认`crons`配置：

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

如果您的项目需要自定义cron作业，则可以将其添加到默认`crons`配置。 请参阅[生成cron作业](#build-a-cron-job)。

### `crontab`

Adobe Commerce仅向Pro项目添加了auto-crons配置选项，以支持暂存和生产环境中的自助`crons`配置。 如果启用此选项，则可以使用`crontab`查看cron配置。 这是&#x200B;_不_&#x200B;可用于入门项目。

尽管您可以使用`crontab`查看Pro项目上的配置，但Adobe Commerce不使用`crontab`为部署在云基础架构上的站点运行cron作业。

**要在Pro环境中查看cron配置**，请执行以下操作：

1. 使用[SSH](../development/secure-connections.md#use-an-ssh-command)登录到远程环境。

1. 列出计划的cron进程。

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >如果`crontab -l`命令返回`Command not found`错误（仅在Pro暂存和生产环境中），您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以在您的项目上启用auto-crons自助服务配置选项。

以下示例显示仅具有默认`crons`配置的环境的`crontab`输出：

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## 构建cron作业

cron作业包括计划和时间规格以及在计划时间运行的命令。 对于入门环境和Pro `integration`环境，最小间隔为每5分钟一次。 对于Pro暂存环境和生产环境，最小间隔为每分钟一次。 在云基础架构上的Adobe Commerce上，将自定义cron作业添加到`crons`部分中的`.magento.app.yaml`文件。 常规格式为`spec`用于计划，为`cmd`用于指定要运行的命令或自定义脚本。

### 规范

Adobe Commerce为`crons`规范（规范）使用五值表达式： `* * * * *`

1. 分钟（0到59）对于所有Starter和Pro环境，cron作业支持的最低频率为五分钟。 您可能需要在管理员中配置设置。
2. 小时（0到23）
3. 日期（1到31）
4. 月（1至12）
5. 每周时间（0到6）（星期日到星期六；某些系统上的星期日也是7）

一些示例：

- `00 */3 * * *`每三小时在第一分钟运行一次(12:00 am、3:00 am、6:00 am)
- `20 */8 * * *`在分钟20每8小时运行一次（上午12:20，上午8:20，下午4:20）
- `00 00 * * *`每天午夜运行一次
- `00 * * * 1`在星期一午夜每周运行一次。

>[!NOTE]
>
>`.magento.app.yaml`文件中指定的`crons`时间基于服务器时区，而不是在数据库的存储配置值中指定的时区。

在确定计划时，请考虑完成任务所需的时间。 例如，如果您每三小时运行一次作业，并且任务需要40分钟才能完成，则可以考虑更改计划时间。

### 命令

`cmd`指定要运行的命令或自定义脚本。 命令脚本格式可以包括以下内容：

```text
<path-to-php-binary> <project-dir>/<script-command>
```

例如：

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

在此示例中，`<path-to-php-binary>`是`/usr/bin/php`。 包括项目ID的安装目录为`/app/abc123edf890/bin/magento`，脚本操作为`export:start catalog_category_product`。

### 将自定义cron作业添加到您的项目

在云基础架构平台上的Adobe Commerce上，您可以将自定义项添加到[`.magento.app.yaml`](../application/configure-app-yaml.md)文件的`crons`部分。

>[!NOTE]
>
>对于入门环境和Pro `integration`环境，最小间隔为每5分钟一次。 对于Pro暂存环境和生产环境，最小间隔为每分钟一次。 您不能配置比默认最小值更频繁的间隔。

在Adobe Commerce Pro项目中，必须先在项目上启用[自动cron功能](#set-up-cron-jobs)，然后才能使用`.magento.app.yaml`文件将自定义cron作业添加到暂存环境和生产环境。 如果未启用此功能，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以启用自动登录。

**要添加自定义cron作业**：

1. 在本地开发环境中，编辑Adobe Commerce `/app`目录中的`.magento.app.yaml`文件。

1. 在`crons`部分中，使用以下格式添加自定义项：

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   在以下示例中，`productcatalog`作业每八小时导出一次产品目录，即该小时后的20分钟。

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### 更新cron作业

要添加、删除或更新自定义作业，请在`.magento.app.yaml`文件的`crons`部分中更改配置。 然后，在将更改推送到暂存环境和生产环境之前，在远程`integration`环境中测试更新。

## 禁用cron作业

您可以在完成维护任务（如重新索引或清除缓存）之前手动禁用cron作业，以防止出现性能问题。 您可以使用`ece-tools` CLI命令`cron:disable`禁用所有cron作业并停止任何活动的cron进程。

**要禁用cron作业**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 禁用cron作业并停止活动cron进程。

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. 完成任何必需的维护任务后，请确保再次启用cron作业。

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## cron作业疑难解答

Adobe更新了Adobe Commerce on cloud基础架构包，以优化Adobe Commerce on cloud基础架构平台的cron处理并修复与cron相关的问题。 如果您遇到cron处理问题，请确保您的项目使用的是`ece-tools`包的最新版本。 请参阅[更新ECE-Tools](../dev-tools/update-package.md)。

您可以在每个环境的应用程序级日志文件中查看cron处理信息。 查看[应用程序日志](../test/log-locations.md#application-logs)。

请参阅以下Adobe Commerce支持文章，以获取有关排查cron相关问题的帮助：

- [Cron任务锁定来自其他组的任务](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [在云上手动重置卡住cron作业](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
