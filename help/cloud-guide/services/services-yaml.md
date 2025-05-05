---
title: 配置服务
description: 了解如何在云基础架构上配置Adobe Commerce使用的服务。
feature: Cloud, Configuration, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1046'
ht-degree: 0%

---

# 配置服务

`services.yaml`文件定义Adobe Commerce在云基础架构上支持和使用的服务，例如MySQL、Redis和Elasticsearch或OpenSearch。 您无需订阅外部服务提供商。

>[!NOTE]
>
>`.magento/services.yaml`文件是在项目的`.magento`目录中本地管理的。 在构建过程中仅访问配置以在集成环境中定义所需的服务版本，并在部署完成后删除，这样在服务器上就找不到它们。


部署脚本使用`.magento`目录中的配置文件为环境配置配置的服务。 如果某个服务包含在`.magento.app.yaml`文件的[`relationships`](../application/properties.md#relationships)属性中，则该服务对您的应用程序可用。 `services.yaml`文件包含&#x200B;_类型_&#x200B;和&#x200B;_磁盘_&#x200B;值。 服务类型定义服务&#x200B;_name_&#x200B;和&#x200B;_version_。

更改服务配置会导致部署为环境提供更新的服务，这会对以下环境造成影响：

- 所有入门环境，包括生产`master`
- 专业集成环境

{{pro-update-service}}

## 默认服务和支持的服务

云基础架构支持和部署以下服务：

- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

您可以在当前[默认`services.yaml`文件](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml)中查看默认版本和磁盘值。 以下示例显示了`services.yaml`配置文件中定义的`mysql`、`redis`、`opensearch`或`elasticsearch`以及`rabbitmq`服务：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## 服务值

您必须提供服务ID和服务类型配置`type: <name>:<version>`。 如果服务使用永久存储，则必须提供磁盘值。

使用以下格式：

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

`service-id`值标识项目中的服务。 您只能使用小写字母数字字符：`a`到`z`和`0`到`9`，如`redis`。

此&#x200B;_service-id_&#x200B;值在`.magento.app.yaml`配置文件的[`relationships`](../application/properties.md#relationships)属性中使用：

```yaml
relationships:
    redis: "<name>:redis"
```

您可以命名每种服务类型的多个实例。 例如，您可以使用多个Redis实例 — 一个用于会话，一个用于缓存。

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

重命名`services.yaml`文件&#x200B;**中的服务将永久删除**&#x200B;以下内容：

- 使用您指定的新名称创建服务之前的现有服务。
- 服务的所有现有数据都会被删除。 Adobe强烈建议您在更改现有服务的名称之前[备份您的入门环境](../storage/snapshots.md)。

### `type`

`type`值指定服务名称和版本。 例如：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

`disk`值指定要分配给服务的永久磁盘存储的大小（以MB为单位）。 使用永久存储的服务（如MySQL）必须提供磁盘值。 使用内存而不是永久存储的服务（如Redis ）不需要磁盘值。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

每个项目的当前默认存储量为5 GB，即512 0 MB。 您可以在应用程序及其各项服务之间分配此金额。

## 服务关系

在云基础架构项目上的Adobe Commerce中，在`.magento.app.yaml`文件中配置的服务[关系](../application/properties.md#relationships)决定了应用程序可用的服务。

您可以从[`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md)环境变量检索所有服务关系的配置数据。 配置数据包括服务名称、类型和版本，以及任何所需的连接详细信息，如端口号和登录凭据。

**验证本地环境中的关系**：

1. 在本地环境中，显示活动环境的关系。

   ```bash
   magento-cloud relationships
   ```

1. 确认响应中的`service`和`type`。 响应提供连接信息，如IP地址和端口号。

   >缩写示例响应

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**验证远程环境中的关系**：

1. 使用SSH登录到远程环境。

1. 列出环境中配置的所有服务的关系配置数据。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或者，使用以下`ece-tools`命令查看关系：

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 确认响应中的`service`和`type`。 响应提供连接信息，如IP地址和端口号以及任何所需的用户名和密码凭据。

## 服务版本

云基础架构上Adobe Commerce的服务版本和兼容性支持取决于在云基础架构上部署和测试的版本，有时与Adobe Commerce内部部署支持的版本不同。 有关Adobe已使用特定Adobe Commerce和Magento Open Source版本测试的第三方软件依赖项的列表，请参阅&#x200B;_安装_&#x200B;指南中的[系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=zh-Hans)。

### 软件EOL检查

在部署过程中，`ece-tools`包会根据每个服务的生命周期结束(EOL)日期检查已安装的服务版本。

- 如果服务版本在EOL日期后的三个月内，则部署日志中会显示通知。
- 如果EOL日期是过去的时间，则会显示警告通知。

为维护存储安全性，请在已安装的软件版本达到EOL之前更新这些版本。 您可以在[ece-tools&#39; `eol.yaml`文件](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml)中查看EOL日期。

### 迁移到OpenSearch

{{elasticsearch-support}}

对于Adobe Commerce版本2.4.4及更高版本，请参阅[设置OpenSearch服务](opensearch.md)。

## 更改服务版本

您可以升级已安装的服务版本，以便与云环境中部署的Adobe Commerce版本兼容。

不能直接降级已安装服务的服务版本。 但是，您可以使用所需的版本创建服务。 请参阅[降级服务版本](#downgrade-version)。

### 升级已安装的服务版本

您可以通过更新`services.yaml`文件中的服务配置来升级已安装的服务版本。

1. 更改`.magento/services.yaml`文件中服务的[`type`](#type)值：

   > 原始服务定义

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > 更新了服务定义

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### 降级版本

不能直接降级已安装的服务。 您有两个选项：

1. 使用新版本重命名现有服务，这将删除现有服务和数据，并添加新服务和数据。

1. 创建服务并从现有服务中保存数据。

更改服务版本时，必须更新`services.yaml`文件中的服务配置，并更新`.magento.app.yaml`文件中的关系。

**要通过重命名现有服务来降级服务版本**，请执行以下操作：

1. 重命名`.magento/services.yaml`文件中的现有服务并更改版本。

   >[!WARNING]
   >
   >重命名现有服务会替换现有服务并删除所有数据。 如果需要保留数据，请创建一个服务，而不是重命名现有服务。

   例如，要将&#x200B;_mysql_&#x200B;服务的MariaDB版本从版本10.4降级到10.3，请更改现有的&#x200B;_service-id_&#x200B;和&#x200B;_type_&#x200B;配置。

   > 原始`services.yaml`定义

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 新`services.yaml`定义

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. 更新`.magento.app.yaml`文件中的关系。

   > 原始`.magento.app.yaml`配置

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 已更新`.magento.app.yaml`配置

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 添加、提交和推送代码更改。

**要通过创建服务来降级服务**，请执行以下操作：

1. 将服务定义添加到具有降级版本规范的项目的`services.yaml`文件中。 请参阅以下示例中的&#x200B;_mysql2_：

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. 更改`.magento.app.yaml`文件中的关系配置以使用新服务。

   > 原始`.magento.app.yaml`配置

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 新`.magento.app.yaml`配置

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 添加、提交和推送代码更改。
