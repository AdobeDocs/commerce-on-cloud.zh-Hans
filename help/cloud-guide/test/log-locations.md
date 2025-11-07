---
title: 查看和管理日志
description: 了解云基础架构中可用的日志文件类型以及在何处查找它们。
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: 445c5162f9d3436d9e5fe3df41af47189e344cfd
workflow-type: tm+mt
source-wordcount: '1313'
ht-degree: 0%

---

# 查看和管理日志

云基础架构项目上的Adobe Commerce日志可用于排除与[生成和部署挂接](../application/hooks-property.md)、云服务和Adobe Commerce应用程序相关的问题。

您可以从文件系统、[!DNL Cloud Console]和`magento-cloud` CLI中查看日志。

- **文件系统** - `/var/log`系统目录包含所有环境的日志。 `var/log/`目录包含特定环境专属的应用程序特定日志。 这些目录不在群集中的节点之间共享。 在Pro生产和暂存环境中，必须检查每个节点上的日志。

- **[!DNL Cloud Console]** — 您可以在环境&#x200B;_消息_&#x200B;列表中看到生成、部署和部署后日志信息。

- **云CLI** — 您可以使用`magento-cloud log`命令查看本地环境日志，或使用`magento-cloud ssh`命令查看远程环境日志。

## 日志位置

系统日志存储在以下位置：

- 集成： `/var/log/<log-name>.log`
- Pro暂存： `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Pro生产： `/var/log/platform/<project-ID>/<log-name>.log`

`<project-ID>`的值取决于项目以及环境是“暂存”还是“生产”。 例如，项目ID为`yw1unoukjcawe`时，暂存环境用户为`yw1unoukjcawe_stg`，生产环境用户为`yw1unoukjcawe`。

使用该示例，部署日志为： `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### 查找特定错误日志记录

当遇到特定日志记录编号（如`475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9`）的错误时，可以使用以下方法通过查询Commerce应用程序服务器远程环境日志来查找记录：

>[!NOTE]
>
>有关使用Secure Shell (SSH)访问Commerce应用程序的远程环境日志的说明，请参阅[到远程环境的安全连接](../development/secure-connections.md)。

#### 方法1：使用grep搜索

```bash
# Search for the specific error record in all log files
magento-cloud ssh -e <environment-ID> "grep -r '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' /var/log/"

# Search in specific log files
magento-cloud ssh -e <environment-ID> "grep '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' /var/log/exception.log"
```

#### 方法2：在归档日志中搜索

如果以前发生错误，请检查归档日志文件：

```bash
# Search in compressed log files
magento-cloud ssh -e <environment-ID> "find /var/log -name '*.gz' -exec zgrep '475a3bca674d3bbc77b35973d028e6da1cbee7404888bfb113daffc6b2f4a7b9' {} \;"
```

#### 方法3：使用New Relic (Pro environments)

对于专业生产和暂存环境，请使用New Relic日志搜索特定的错误记录。 有关详细信息，请参阅[New Relic日志管理](../monitor/log-management.md)。

### 查看远程环境日志

大多数日志都包含在远程环境中发生的事件。 对于Pro，有多个节点，每个节点都有唯一的日志。 使用下列内容查看所有主机的列表：

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

示例响应：

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**要查看远程环境日志的列表**：

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Pro示例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**要查看远程日志**：

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Pro示例：

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>对于Pro Staging和Pro Production环境，为具有固定文件名的日志文件启用自动日志旋转、压缩和删除。 每种日志文件类型都有一个旋转模式和生命周期。
>有关环境的日志轮换和压缩日志的生命周期的完整详细信息，请参见： `/etc/logrotate.conf`和`/etc/logrotate.d/<various>`。
>对于Pro Staging和Pro Production环境，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以请求更改日志轮换配置。

>[!TIP]
>
>无法在Pro Integration环境中配置日志轮换。
>对于Pro集成，您必须实施自定义解决方案/脚本，并[配置cron](../application/crons-property.md)以根据需要运行脚本。

>[!NOTE]
>
>入门项目环境没有日志轮换。

## 生成和部署日志

将更改推送到环境后，您可以从`var/log/cloud.log`文件中的每个挂接查看日志记录。 日志包含每个挂接的启动和停止消息。 在以下示例中，消息为“`Starting post-deploy.`”和“`Post-deploy is complete.`”

检查日志条目上的时间戳，验证并查找特定部署的日志。 以下是可用于故障排除的日志输出的简短示例：

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>在配置云环境时，您可以设置[基于日志的Slack和电子邮件通知](../environment/set-up-notifications.md)，以便执行生成和部署操作。

以下日志具有适用于所有云项目的公共位置：

- **部署日志**： `var/log/cloud.log`
- **上次部署错误日志**： `var/log/cloud.error.log`
- **调试日志**： `var/log/debug.log`
- **异常日志**： `var/log/exception.log`
- **系统日志**： `var/log/system.log`
- **支持日志**： `var/log/support_report.log`
- **报告**： `var/report/`

虽然`cloud.log`文件包含来自部署过程每个阶段的反馈，但部署挂接创建的日志对每个环境都是唯一的。 特定于环境的部署日志位于以下目录中：

- **入门和专业集成**： `/var/log/deploy.log`
- **专业暂存**： `/var/log/platform/<project-ID>_stg/deploy.log`
- **专业生产**： `/var/log/platform/<project-ID>/deploy.log`

### 部署日志

每个部署的日志都连接到特定的`deploy.log`文件。 以下示例在终端中打印当前环境的部署日志：

```bash
magento-cloud log -e <environment-ID> deploy
```

示例响应：

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### 错误日志

部署过程中生成的错误和警告消息将写入`var/log/cloud.log`和`var/log/cloud.error.log`文件。 云错误日志文件仅包含来自最新部署的错误和警告。 空文件表示部署成功，并且没有错误。

您可以使用[Cloud CLI SSH](#view-remote-environment-logs)查看日志文件，也可以使用ECE-Tools显示错误及建议：

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

示例响应：

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

大多数错误消息都包含说明和建议的操作。 使用ECE-Tools[的](../dev-tools/error-reference.md)错误消息引用查找错误代码以获得进一步的指导。 有关进一步指导，请使用[Adobe Commerce部署疑难解答程序](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)。

## 应用程序日志

与部署日志类似，应用程序日志对于每个环境都是唯一的：

| 日志文件 | Starter和Pro集成 | 描述 |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **部署日志** | `/var/log/deploy.log` | 来自[部署挂接](../application/hooks-property.md)的活动。 |
| **部署后日志** | `/var/log/post_deploy.log` | 来自[部署后挂接](../application/hooks-property.md)的活动。 |
| **Cron日志** | `/var/log/cron.log` | cron作业的输出。 |
| **Nginx访问日志** | `/var/log/access.log` | 在Nginx启动时，缺少目录和排除的文件类型出现HTTP错误。 |
| **Nginx错误日志** | `/var/log/error.log` | 用于调试与Nginx相关的配置错误的启动消息。 |
| **PHP访问日志** | `/var/log/php.access.log` | 对PHP服务的请求。 |
| **PHP FPM日志** | `/var/log/app.log` | |

对于Pro暂存和生产环境，部署、部署后和Cron日志仅在群集中的第一个节点上可用：

| 日志文件 | Pro Staging | Pro Production |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **部署日志** | 仅第一个节点：<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | 仅第一个节点：<br>`/var/log/platform/<project-ID>/deploy.log` |
| **部署后日志** | 仅第一个节点：<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | 仅第一个节点：<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Cron日志** | 仅第一个节点：<br>`/var/log/platform/<project-ID>_stg*/cron.log` | 仅第一个节点：<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx访问日志** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Nginx错误日志** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP访问日志** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM日志** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### 归档日志文件

应用程序日志每天压缩并存档一次，默认情况下保留&#x200B;**30天**（对于Pro暂存和生产群集） — 日志轮换在所有集成/入门环境中不可用。 压缩日志使用与`Number of Days Ago + 1`对应的唯一ID进行命名。 例如，在Pro生产环境中，过去21天的PHP访问日志按如下方式存储和命名：

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

存档的日志文件始终存储在压缩前原始文件所在的目录中。

您可以[提交支持票证](https://experienceleague.adobe.com/home?support-tab=home#support)以请求对您的日志保留期或logrotate配置进行更改。 您可以将保留期增加到最多365天，减少保留期以节省存储配额，或向logrotate配置添加其他日志路径。 这些更改可用于Pro暂存和生产群集。

例如，如果您创建自定义路径以在`var/log/mymodule`目录中存储日志，则可以请求此路径的日志轮换。 但是，当前的基础架构需要一致的文件名才能使Adobe正确配置日志轮换。 Adobe建议保持日志名称一致以避免配置问题。

>[!NOTE]
>
>**部署**&#x200B;和&#x200B;**部署后**&#x200B;日志文件未轮换和存档。 整个部署历史记录将写入这些日志文件中。

## 服务日志

由于每个服务都在一个单独的容器中运行，因此服务日志在集成环境中不可用。 云基础架构上的Adobe Commerce仅允许访问集成环境中的Web服务器容器。 以下服务日志位置适用于专业生产和暂存环境：

- **Redis日志**： `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Elasticsearch日志**： `/var/log/elasticsearch/elasticsearch.log`
- **Java垃圾回收日志**： `/var/log/elasticsearch/gc.log`
- **邮件日志**： `/var/log/mail.log`
- **MySQL错误日志**： `/var/log/mysql/mysql-error.log`
- **MySQL慢速日志**： `/var/log/mysql/mysql-slow.log`
- **RabbitMQ日志**： `/var/log/rabbitmq/rabbit@host1.log`

根据日志类型，服务日志会存档并保存不同的时间段。 例如，MySQL日志的生命周期最短–7天后删除。

>[!TIP]
>
>在缩放的体系结构中，日志文件位置取决于节点类型。 请参阅缩放体系结构[主题中的](../architecture/scaled-architecture.md#log-locations)日志位置。

## 记录用于专业生产和暂存的数据

在Pro生产和暂存环境中，使用与您的项目集成的[New Relic日志管理](../monitor/log-management.md)来管理云基础架构项目上与Adobe Commerce关联的所有日志中的聚合日志数据。

New Relic Logs应用程序提供了一个集中式日志管理功能板，用于对云基础架构生产和暂存环境上的Adobe Commerce进行故障排除和监控。 仪表板还提供对Fastly CDN、图像优化和Web应用程序防火墙(WAF)服务的日志数据的访问。 请参阅[New Relic服务](../monitor/new-relic-service.md)。
