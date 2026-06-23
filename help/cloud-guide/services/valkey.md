---
title: 设置Valkey服务
description: 了解如何在Cloud Infrastructure上设置和优化Valkey作为适用于Adobe Commerce的后端缓存解决方案。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 7828287703ea93d9b84f7991e316bd2286964b90
workflow-type: tm+mt
source-wordcount: 229
ht-degree: 0%

---

# 设置Valkey服务

[Valkey](https://valkey.io)是一个可选的后端缓存解决方案，它取代了Adobe Commerce默认使用的`Zend Framework Zend_Cache_Backend_File`。

请参阅&#x200B;_实施行动手册最佳实践指南_&#x200B;中的[配置Valkey](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration){target="_blank"}。

{{service-instruction}}

**要使用Valkey替换Redis，请更新以下三个文件中的配置**：

1. 将Redis配置替换为所需的Valkey名称，并在`.magento/services.yaml`文件中键入。

   ```yaml
   cache:
       type: valkey:<version>
   ```

   要提供您自己的Valkey配置，请在`.magento/services.yaml`文件中添加`core_config`密钥：

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. 在`.magento.app.yaml`文件中配置关系。

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. 按如下方式配置`.magento.env.yaml`以替换Redis配置：

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 使用Valkey CLI

假定您的Valkey关系名为`valkey`，则可以使用`valkey-cli`工具访问它。

1. 使用SSH连接到已安装并配置Valkey的集成环境。

1. 打开到主机的SSH通道。

   ```bash
   valkey-cli -h valkey.internal
   ```

## 获取已安装的Valkey版本

使用以下命令获取集成环境中安装的Valkey版本：

```bash
valkey-cli -h valkey.internal info | grep version
```

响应：

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey on Pro暂存和生产

要获取在暂存或生产环境中安装的Valkey版本，请使用`valkey-server`命令：

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

使用以下命令获取Pro Staging或生产环境中安装的Valkey配置：

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

响应：

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
