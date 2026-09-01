---
title: 设置Redis服务
description: 了解如何在云基础架构上设置和优化Redis作为适用于Adobe Commerce的后端缓存解决方案。
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# 设置Redis服务

[Redis](https://redis.io)是一个可选的后端缓存解决方案，它取代了Adobe Commerce默认使用的`Zend Framework Zend_Cache_Backend_File`。

>[!IMPORTANT]
>
>Adobe Commerce 2.4.9或更高版本的2.4.5-p16、2.4.6-p14、2.4.7-p9和2.4.8-p4修补程序不支持Redis缓存。 对于不支持Redis的缓存配置，请使用[Valkey](valkey.md)。 按版本查看支持的缓存服务的[系统要求](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)。

{{service-instruction}}

## 启用Redis

要启用Redis，请更新以下文件：

- `.magento/services.yaml`
- `.magento.app.yaml`

### 配置服务

在`.magento/services.yaml`中，添加Redis服务定义。 将`<version>`替换为您的Adobe Commerce版本和当前Cloud模板支持的Redis版本。

```yaml
cache:
  type: redis:<version>
```

例如，对于支持Redis 7.2的Commerce版本和Cloud模板：

```yaml
cache:
  type: redis:7.2
```

示例版本不是通用的。 实际的默认服务版本和支持的服务版本取决于您的Adobe Commerce版本、修补程序级别和当前的Cloud模板。 验证[系统要求](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)和当前项目模板中支持的组合。

### 配置服务关系

在`.magento.app.yaml`中，配置应用程序与Redis服务之间的关系：

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

关系键`redis`是应用程序用于访问服务的名称。 值`cache:redis`包含在`.magento/services.yaml`中定义的服务ID (`cache`)和服务类型(`redis`)。

### 提交并部署更改

添加、提交和推送配置更改：

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

部署完成后，验证Redis服务关系是否可用。

{{service-change-tip}}

## 验证服务关系

部署配置后，从应用程序容器中运行以下命令以显示已解码的`MAGENTO_CLOUD_RELATIONSHIPS`对象：

使用SSH连接到远程云环境，然后运行：

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

该命令显示所有已配置的服务关系。 找到`redis`关系以标识Redis连接详细信息。

以下缩写示例显示了`redis`关系。 它不是一个通用的架构。

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

输出因环境和服务配置而异。 请勿对此示例中的主机名、端口、IP地址、群集名称、服务版本、用户名或密码进行硬编码。 在目标环境中使用`MAGENTO_CLOUD_RELATIONSHIPS`返回的值。

如果`jq`可用，请使用以下命令仅显示Redis关系：

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

有关服务关系的详细信息，请参阅[配置服务](services-yaml.md)。

## 自定义Redis配置

有关缓存、会话、L2和副本连接建议，请参阅&#x200B;_实施行动手册最佳实践指南_&#x200B;中的[Valkey和Redis服务配置的最佳实践](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

## 使用Redis CLI

假定您的Redis关系名为`redis`，请使用`MAGENTO_CLOUD_RELATIONSHIPS`返回的主机和端口连接到Redis。

在安装和配置Redis的情况下连接到环境，并运行以下命令：

```terminal
redis-cli -h <host> -p <port>
```

**示例**

```terminal
redis-cli -h redis.internal -p 6379
```

## 获取已安装的Redis版本

>[!BEGINTABS]

>[!TAB 集成环境]

在集成环境中，使用`redis`关系返回的主机和端口运行：

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**示例响应**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

版本和内部版本详细信息因环境而异。 请勿将显示的示例版本视为必需或通用服务版本。

>[!TAB 专业暂存和生产]

在专业暂存和生产环境中，运行：

```terminal
redis-server -v
```

**示例响应**

```text
Redis server v=<installed-version> ...
```

版本和内部版本详细信息因环境而异。 请勿将显示的示例版本视为必需或通用服务版本。

>[!ENDTABS]

## Redis疑难解答

请参阅以下Adobe Commerce支持文章，以获取有关Redis问题疑难解答的帮助：

- [Adobe Commerce上的托管警报： Redis内存警告警报](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Adobe Commerce上的托管警报：Redis内存严重警报](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### Cache-clean错误引用Valkey配置的缓存上的Redis

预部署缓存清理失败可能会显示错误代码`[107]` (`clean-redis-cache`)和`Connection to Redis`消息，即使`cache`服务配置为Valkey也是如此。 `ece-tools`使用此旧版面向Redis的错误代码和消息执行缓存清理步骤，而不管哪个服务支持`cache`关系，因此措辞不表示Redis已安装。

如果基础错误是DNS故障（如关系主机的`Name or service not known`），则部署步骤在服务关系可用之前运行，或者`.magento.app.yaml`中的关系名称与`.magento/services.yaml`中的服务ID不匹配。 请参阅[验证服务关系](#verify-the-service-relationship)。
