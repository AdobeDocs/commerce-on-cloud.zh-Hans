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
source-git-commit: bec442a5b442adafbd23c7c8eac1adbdb7b93b65
workflow-type: tm+mt
source-wordcount: 328
ht-degree: 0%

---

# 设置Redis服务

[Redis](https://redis.io)是一个可选的后端缓存解决方案，它取代了Adobe Commerce默认使用的Zend Framework Zend_Cache_Backend_File。

请参阅&#x200B;_实施行动手册最佳实践指南_&#x200B;中的[配置红色](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

{{service-instruction}}

**启用Redis**：

1. 将所需的名称和类型添加到`.magento/services.yaml`文件中。

   ```yaml
   myredis:
       type: redis:<version>
   ```

   要提供您自己的Redis配置，请在`.magento/services.yaml`文件中添加`core_config`密钥：

   ```yaml
   cache:
       type: redis:<version>
   ```

1. 在`.magento.app.yaml`文件中配置关系。

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 使用Redis CLI

假定您的Redis关系名为`redis`，则可以使用`redis-cli`工具访问它。

1. 使用SSH连接到已安装并配置了Redis的集成环境。

1. 打开到主机的SSH通道。

   ```bash
   redis-cli -h redis.internal
   ```

## 获取已安装的Redis版本

使用以下命令获取集成环境中安装的Redis版本：

```bash
redis-cli -h redis.internal info | grep version
```

示例响应：

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Pro暂存和生产环境

要在暂存或生产环境中安装Redis版本，请使用`redis-server`命令：

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

使用以下命令获取Redis配置在Pro暂存或生产环境中安装：

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

示例响应：

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Redis疑难解答

请参阅以下Adobe Commerce支持文章，以获取有关Redis问题疑难解答的帮助：

- [Redis问题延迟管理员登录或签出](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [扩展Redis缓存实施Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [Adobe Commerce上的托管警报： Redis内存警告警报](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Adobe Commerce上的托管警报：Redis内存严重警报](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
