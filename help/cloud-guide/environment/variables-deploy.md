---
title: 部署变量
description: 请参阅在云基础架构部署阶段控制Adobe Commerce中操作的环境变量列表。
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 3f2a4f7dc9c23afb3af80304023d9e742c974ccd
workflow-type: tm+mt
source-wordcount: '2483'
ht-degree: 0%

---

# 部署变量

以下&#x200B;_部署_&#x200B;变量在部署阶段控制操作，可以继承和覆盖来自[全局变量](variables-global.md)的值。 在`deploy`文件的`.magento.env.yaml`阶段中插入这些变量：

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

有关自定义生成和部署过程的详细信息：

- [部署配置](configure-env-yaml.md)
- [部署过程](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

配置Redis页和默认缓存。 在设置`cm_cache_backend_redis`参数时，必须指定`server`、`port`和`database`选项。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

以下示例使用[配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature)中定义的&#x200B;_Redis预加载功能_：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

要使用自定义[REDIS_BACKEND](#redis_backend)模型(不仅来自允许列表)，请将`_custom_redis_backend`选项设置为`true`以启用正确的验证，如以下示例所示：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **默认**—`true`
- **版本**—Adobe Commerce 2.1.4及更高版本

启用或禁用清理在生成或部署阶段生成的[静态内容文件](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)。 在开发中使用默认值&#x200B;_true_&#x200B;作为最佳实践。

- **`true`** — 在部署更新的静态内容之前删除所有现有的静态内容。
- **`false`** — 仅当生成的内容包含较新版本时，部署才会覆盖现有的静态内容文件。

如果您通过单独的进程修改静态内容，则将值设置为&#x200B;_false_。

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

如果在部署之前未能清除静态视图文件，则在未删除先前版本的情况下将更新部署到现有文件时，可能会导致出现问题。 由于[静态文件回退](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache)规则，如果目录包含同一文件的多个版本，则回退操作可能会显示错误的文件。

## `CRON_CONSUMERS_RUNNER`

- **默认**—`cron_run = false`，`max_messages = 1000`
- **版本**—Adobe Commerce 2.2.0及更高版本

使用此环境变量可确认消息队列在部署后正在运行。

- `cron_run` — 一个布尔值，用于启用或禁用`consumers_runner` cron作业（默认值= `false`）。
- `max_messages` — 指定每个使用者在终止前必须处理的最大消息数（默认值= `1000`）的数字。 您可以将值设置为`0`以防止使用者终止。
- `consumers` — 一个字符串数组，指定要运行的使用者。 空数组运行&#x200B;_所有_&#x200B;使用者。

- `multiple_processes` — 指定每个使用者要衍生的进程数的数字。 在Commerce **2.4.4**&#x200B;或更高版本中支持。

>[!NOTE]
>
>要返回消息队列`consumers`的列表，请在远程环境中运行`./bin/magento queue:consumers:list`命令。

运行特定`consumers`且为每个使用者派生`multiple_processes`的示例数组：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

运行所有`consumers`的空数组的示例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

默认情况下，部署进程将覆盖`env.php`文件中的所有设置。 请参阅本地Adobe Commerce的[Commerce配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html)中的&#x200B;_管理消息队列_。

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **默认**—`false`
- **版本**—Adobe Commerce 2.2.0及更高版本

通过选择以下选项之一，配置`consumers`处理消息队列中的消息的方式：

- `false`—`Consumers`处理队列中的可用消息，关闭TCP连接并终止。 即使已处理的消息数小于`Consumers`部署变量中指定的`max_messages`值，`CRON_CONSUMERS_RUNNER`也不等待其他消息进入队列。

- `true`—`Consumers`继续处理来自消息队列的消息，直到达到`max_messages`部署变量中指定的最大消息数(`CRON_CONSUMERS_RUNNER`)，然后关闭TCP连接并终止使用者进程。 如果队列在到达`max_messages`之前排空，则使用者将等待更多消息到达。

>[!WARNING]
>
>如果使用工作程序运行`consumers`而不是使用cron作业，则将此变量设置为true。

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

>[!WARNING]
>
>通过`CRYPT_KEY`而不是[!DNL Cloud Console]文件设置`.magento.env.yaml`值，以避免在您的环境的源代码存储库中公开密钥。 请参阅[设置环境和项目变量](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html#configure-environment)。

在没有安装过程的情况下将数据库从一个环境移动到另一个环境时，需要相应的加密信息。 Adobe Commerce使用[!DNL Cloud Console]中设置的加密密钥值作为`crypt/key`文件中的`env.php`值。

## `DATABASE_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

如果在[文件的](../application/properties.md#relationships)关系属性`.magento.app.yaml`中定义了数据库，则可以自定义数据库连接以进行部署。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

此外，您还可以配置表前缀。

>[!WARNING]
>
>如果不将合并选项与表前缀一起使用，则必须提供默认连接设置或部署验证失败。

以下示例使用带有默认连接设置的`ece_`表前缀，而不是使用`_merge`选项：

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

示例输出：

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.2.0及更高版本

在部署之间保留自定义的[!DNL Elastic Suite]服务设置，并在主[!DNL Elastic Suite]配置的“system/default/smile_elasticsuite_core_base_settings”部分使用它。 如果已安装[!DNL Elastic Suite]编辑器包，则会自动对其进行配置。

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>在[缩放架构](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)上具有三个节点（或三个服务节点）的Pro暂存/生产群集上，`indices_settings`应设置如下：
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**已知限制**：

- 将搜索引擎更改为`elasticsuite`以外的任何类型会导致部署失败并出现相应的验证错误
- 删除Elasticsearch服务会导致部署失败并出现相应的验证错误

>[!NOTE]
>
>有关在Adobe Commerce中使用或对[!DNL Elastic Suite]插件进行故障排除的详细信息，请参阅[[!DNL Elastic Suite] 文档](https://github.com/Smile-SA/elasticsuite)。

## `ENABLE_GOOGLE_ANALYTICS`

- **默认**—`false`
- **版本**—Adobe Commerce 2.1.4及更高版本

在部署到暂存和集成环境时，启用和禁用Google Analytics。 默认情况下，Google Analytics仅适用于生产环境。 将此值设置为`true`以在暂存和集成环境中启用Google Analytics。

- **`true`** — 在暂存环境和集成环境中启用Google Analytics。
- **`false`** — 在暂存环境和集成环境中禁用Google Analytics。

将`ENABLE_GOOGLE_ANALYTICS`环境变量添加到`deploy`文件中的`.magento.env.yaml`阶段：

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>在生产环境中，部署过程始终会启用Google Analytics。

## `FORCE_UPDATE_URLS`

- **默认**—`true`
- **版本**—Adobe Commerce 2.1.4及更高版本

在部署到Pro或Starter Staging and Production环境时，此变量将数据库中的Adobe Commerce基本URL替换为由[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)变量指定的项目URL。 使用此设置可覆盖[UPDATE_URLS](#update_urls)部署变量的默认行为，在部署到暂存或生产环境时会忽略该行为。

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **默认**—`file`
- **版本**—Adobe Commerce 2.2.5及更高版本

锁定提供程序阻止启动重复的cron作业和cron组。 在生产环境中使用`file`锁定提供程序。 入门环境和Pro集成环境不使用[MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md)变量，因此`ece-tools`自动应用`db`锁定提供程序。

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

请参阅[安装指南](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html)中的&#x200B;_配置锁定_。

## `MYSQL_USE_SLAVE_CONNECTION`

- **默认**—`false`
- **版本**—Adobe Commerce 2.1.4及更高版本

>[!TIP]
>
>`MYSQL_USE_SLAVE_CONNECTION`变量仅在Adobe Commerce上受云基础架构暂存和Production Pro群集环境支持，在入门项目上不受支持。

Adobe Commerce可以异步读取多个数据库。 设置为`true`可自动使用到数据库的&#x200B;_只读_&#x200B;连接来接收非主节点上的只读通信。 此连接通过负载平衡提高了性能，因为只有一个节点处理读写通信。 设置为`false`可从`env.php`文件中删除任何现有的只读连接数组。

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

当`MYSQL_USE_SLAVE_CONNECTION`变量设置为`true`时，在Pro暂存和生产环境的`synchronous_replication`文件中，`true`参数默认设置为`env.php`。 当`MYSQL_USE_SLAVE_CONNECTION`设置为`false`时，未配置`synchronous_replication`参数。

## `QUEUE_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

使用此环境变量可在部署之间保留自定义的AMQP服务设置。 例如，如果您希望使用现有的消息队列服务而不是依赖云基础架构为您创建它，请使用`QUEUE_CONFIGURATION`环境变量将其连接到您的站点：

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **默认**—`Cm_Cache_Backend_Redis`
- **版本**—Adobe Commerce 2.3.0及更高版本

指定Redis缓存的后端模型配置。

Adobe Commerce版本2.3.0及更高版本包括以下后端模型：

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

如何设置`REDIS_BACKEND`的示例

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>如果将`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`指定为Redis后端模型以启用[二级缓存](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html)，`ece-tools`将自动生成缓存配置。 请参阅[Adobe Commerce配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example)中的示例&#x200B;_配置文件_。 要覆盖生成的缓存配置，请使用[CACHE_CONFIGURATION](#cache_configuration)部署变量。

## `REDIS_USE_SLAVE_CONNECTION`

- **默认**—`false`
- **版本**—Adobe Commerce 2.1.16及更高版本

>[!WARNING]
>
>请&#x200B;_不_&#x200B;在[缩放的架构](../architecture/scaled-architecture.md)项目中启用此变量。 它导致Redis连接错误。 Redis奴隶仍在活动，但不用于Redis读取。 作为替代方法，Adobe建议使用Adobe Commerce 2.3.5或更高版本，实施新的Redis后端配置，并为Redis实施L2缓存。

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION`变量仅在Adobe Commerce上受云基础架构暂存和Production Pro群集环境支持，在入门项目上不受支持。

Adobe Commerce可以异步读取多个Redis实例。 设置为`true`可自动使用到Redis实例的&#x200B;_只读_&#x200B;连接来接收非主节点上的只读流量。 此连接通过负载平衡提高了性能，因为只有一个节点处理读写通信。 设置为`false`可从`env.php`文件中删除任何现有的只读连接数组。

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

您必须在`.magento.app.yaml`文件和`services.yaml`文件中配置Redis服务。

[ECE-Tools版本2002.0.18](../release-notes/cloud-release-archive.md#v2002018)及更高版本使用更多容错设置。 如果Adobe Commerce无法从Redis _从_&#x200B;实例读取数据，则会从Redis _主_&#x200B;实例读取数据。

只读连接不可用于集成环境或使用[`CACHE_CONFIGURATION`变量](#cache_configuration)。

## `VALKEY_BACKEND`

- **默认**—`Cm_Cache_Backend_Redis`
- **版本**—Adobe Commerce 2.8.0及更高版本

`VALKEY_BACKEND`指定Valkey缓存的后端模型配置。

Adobe Commerce版本2.8.0及更高版本包含以下后端模型：

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

以下示例说明如何设置`VALKEY_BACKEND`：

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>如果将`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`指定为Valkey后端模型以启用[二级缓存](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html)，`ece-tools`将自动生成缓存配置。 请参阅[Adobe Commerce配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example)中的示例&#x200B;_配置文件_。 要覆盖生成的缓存配置，请使用[CACHE_CONFIGURATION](#cache_configuration)部署变量。

## `VALKEY_USE_SLAVE_CONNECTION`

- **默认**—`false`
- **版本**—Adobe Commerce 2.4.8及更高版本

>[!WARNING]
>
>请&#x200B;_不_&#x200B;在[缩放的架构](../architecture/scaled-architecture.md)项目中启用此变量。 它导致Valkey连接错误。 Redis奴隶仍在活动，但不用于Redis读取。 或者，Adobe建议使用Adobe Commerce 2.4.8或更高版本，实施新的Valkey后端配置，并为Valkey实施L2缓存。

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION`变量仅在Adobe Commerce上受云基础架构暂存和Production Pro群集环境支持，在入门项目上不受支持。

Adobe Commerce可以异步读取多个Redis实例。 `VALKEY_USE_SLAVE_CONNECTION`设置为`true`可自动使用到Redis实例的&#x200B;_只读_&#x200B;连接来接收非主节点上的只读流量。 此连接通过负载平衡提高了性能，因为只有一个节点处理读写通信。 将`VALKEY_USE_SLAVE_CONNECTION`设置为`false`以从`env.php`文件中删除任何现有的只读连接数组。

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

您必须在`.magento.app.yaml`文件和`services.yaml`文件中配置Redis服务。

[ECE-Tools版本2002.0.18](../release-notes/cloud-release-archive.md#v2002018)及更高版本使用更多容错设置。 如果Adobe Commerce无法从Valkey _slave_&#x200B;实例读取数据，则会从Redis _master_&#x200B;实例读取数据。

只读连接不可用于集成环境或使用[`CACHE_CONFIGURATION`变量](#cache_configuration)。

## `RESOURCE_CONFIGURATION`

- **默认值** — 未设置
- **版本**—Adobe Commerce 2.1.4及更高版本

将资源名称映射到数据库连接。 此配置对应于`resource`文件的`env.php`部分。

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **默认**—`4`
- **版本**—Adobe Commerce 2.1.4及更高版本

指定压缩静态内容时要使用的[gzip](https://www.gnu.org/software/gzip)压缩级别（`0`到`9`）；`0`禁用压缩。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **默认**—`600`
- **版本**—Adobe Commerce 2.1.4及更高版本

当压缩静态资源所花费的时间超过压缩超时限制时，将中断部署过程。 设置静态内容压缩命令的最长执行时间（秒）。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

您可以为每个主题配置多个区域设置。 此自定义通过减少不必要的主题文件来加快部署过程。 例如，您可以部署英语版的&#x200B;_magento/backend_&#x200B;主题以及其他语言的自定义主题。

以下示例使用三种区域设置部署`Magento/backend`主题：

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

此外，您可以选择&#x200B;_不_&#x200B;部署主题：

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.2.0及更高版本

允许您增加静态内容部署的最大预期执行时间。

默认情况下，Adobe Commerce将最大预期执行时间设置为900秒，但在某些情况下，您可能需要更多时间来完成Cloud项目的静态内容部署。

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **默认**—`false`
- **版本**—Adobe Commerce 2.4.2及更高版本

在部署阶段，设置`SCD_NO_PARENT: true`，以便在部署阶段不生成父主题的静态内容。 此设置可最大限度地缩短部署时间，并防止在部署期间静态内容构建失败时可能发生的站点停机时间。 请参阅[静态内容部署](../deploy/static-content.md)。

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **默认**—`quick`
- **版本**—Adobe Commerce 2.2.0及更高版本

允许您自定义静态内容的[部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)。 请参阅[部署静态视图文件](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)。

如果您有多个区域设置，请仅使用这些选项&#x200B;__：

- `standard` — 为所有包部署所有静态视图文件。
- `quick` — （_默认值_）可最大限度地缩短部署时间。
- `compact` — 节省服务器上的磁盘空间。 在Adobe Commerce版本2.2.4及更早版本中，此设置将使用`scd_threads`的值覆盖`1`的值。

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **默认** — 自动
- **版本**—Adobe Commerce 2.1.4及更高版本

设置静态内容部署的线程数。 默认值是根据检测到的CPU线程数设置的，不超过4的值。 增加线程数会加快静态内容部署；减少线程数会减慢部署速度。 您可以设置线程值，例如：

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

要进一步缩短部署时间，请使用[配置管理](../store/store-settings.md)和`scd-dump`命令将静态部署移动到生成阶段。

## `SEARCH_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

使用此环境变量可在部署之间保留自定义的搜索服务设置。 例如：

Elasticsearch配置：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

OpenSearch配置(适用于Commerce 2.4.6及更高版本)：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

配置Redis会话存储 需要会话存储变量的`save`、`redis`、`host`、`port`和`database`选项。 例如：

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

以下示例将新值合并到现有配置：

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **默认值**— _未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

设置为`true`可在部署阶段跳过静态内容部署。

在部署阶段，设置`SKIP_SCD: true`，以便在部署阶段不进行静态内容生成。 此设置可最大限度地缩短部署时间，并防止在部署期间静态内容构建失败时可能发生的站点停机时间。 请参阅[静态内容部署](../deploy/static-content.md)。

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **默认**—`true`
- **版本**—Adobe Commerce 2.1.4及更高版本

在部署时，将数据库中的Adobe Commerce基本URL替换为[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)变量指定的项目URL。 此配置对于本地开发非常有用，因为本地开发为本地环境设置了基本URL。 当您部署到云环境时，URL会更新，以便您可以使用项目URL访问店面和管理员。

如果您在部署到Pro或Starter暂存和生产环境时必须更新URL，请使用[`FORCE_UPDATE_URLS`](#force_update_urls)变量。

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

为部署阶段执行的[个CLI命令启用或禁用](https://symfony.com/doc/current/console/verbosity.html)Symfony`bin/magento`调试详细级别。

>[!NOTE]
>
>要使用VERBOSE_COMMANDS设置控制命令输出中成功和失败的`bin/magento` CLI命令的详细信息，必须设置[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`。

选择日志中提供的详细信息级别：

- `-v`=正常输出
- `-vv`=更多详细输出
- `-vvv` =详细输出，非常适用于调试

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
