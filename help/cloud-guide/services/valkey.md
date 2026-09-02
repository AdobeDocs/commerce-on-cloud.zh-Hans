---
title: 设置Valkey服务
description: 了解如何在云基础架构上设置和优化Valkey作为Adobe Commerce的后端缓存解决方案，包括替换Redis和自定义缓存后端设置。
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
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# 设置Valkey服务

[Valkey](https://valkey.io)是云基础架构上Adobe Commerce的可选后端缓存解决方案。 当您覆盖Adobe Commerce 2.4.9及更高版本上的默认缓存配置，或在2.4.5-p16、2.4.6-p14、2.4.7-p9和2.4.8-p4之前的修补程序版本上的默认缓存配置时，需要Valkey。

{{service-instruction}}

## 配置Valkey

要使用Valkey替换Redis，请更新以下文件：

- `.magento/services.yaml`
- `.magento.app.yaml`

### 配置服务

在`.magento/services.yaml`中，将Redis服务定义替换为Valkey服务定义。 将`<version>`替换为您的Adobe Commerce版本和当前云模板支持的Valkey版本。

```yaml
cache:
  type: valkey:<version>
```

**示例**

```yaml
cache:
  type: valkey:8.0
```

示例版本不是通用的。 实际的默认服务版本和支持的服务版本取决于您的Adobe Commerce版本和当前的Cloud模板。 使用当前项目模板指定的版本。 有关详细信息，请参阅[配置服务](services-yaml.md#service-versions)。

>[!WARNING]
>
>如果更改服务ID，则会删除现有服务并创建新服务。 永久删除已删除服务中的现有数据。 在重命名服务之前备份环境。

将`type`值从`redis:<version>`更改为`valkey:<version>`时，即使您保留了相同的服务ID，也不要假定缓存和会话数据会保留。 将迁移视为创建新的缓存：无法保证保留现有缓存和会话数据，并且在迁移完成后用户将注销。

### 配置服务关系

在`.magento.app.yaml`中，配置应用程序与Valkey服务之间的关系：

```yaml
relationships:
  valkey: "cache:valkey"
```

关系键`valkey`是应用程序用于访问服务的名称。 值`cache:valkey`引用了`.magento/services.yaml`中定义的服务ID和服务类型。

>[!TIP]
>
>Adobe Commerce通过`credis`客户端库与Valkey进行通信，默认情况下，该库通过普通PHP套接字工作。 若要提高性能，请在`.magento.app.yaml`中启用`redis` PHP扩展。 `credis`在编译后的扩展可用时自动使用该扩展。
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### 提交并部署更改

添加、提交和推送配置更改：

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

部署完成后，验证Valkey服务关系是否可用。

{{service-change-tip}}

{{valkey-newrelic}}

## 自定义Valkey配置

有关缓存、会话、L2和副本连接建议，请参阅&#x200B;_实施行动手册最佳实践指南_&#x200B;中的[Valkey和Redis服务配置的最佳实践](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

## 验证服务关系

要显示已解码的`MAGENTO_CLOUD_RELATIONSHIPS`对象，请在部署配置后从应用程序容器中运行以下命令：

使用SSH连接到远程云环境，然后运行：

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

该命令显示所有已配置的服务关系。 要识别Valkey连接详细信息，请找到valkey关系。

**示例输出**

以下缩写示例显示了`valkey`关系。 它不是一个通用的架构。

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

输出因环境和服务配置而异。 请勿对此示例中的主机名、端口、IP地址、群集名称、服务版本、用户名或密码进行硬编码。 在目标环境中使用`MAGENTO_CLOUD_RELATIONSHIPS`返回的值。

如果`jq`可用，则仅显示Valkey关系：

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

有关服务关系的详细信息，请参阅[配置服务](services-yaml.md)。

## 使用Valkey CLI

假定您的Valkey关系名为`valkey`，请使用`MAGENTO_CLOUD_RELATIONSHIPS`返回的主机和端口连接到Valkey：

```terminal
valkey-cli -h <host> -p <port>
```

**示例**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## 获取已安装的Valkey版本

>[!BEGINTABS]

>[!TAB 集成环境]

在集成环境中，使用`valkey`关系返回的主机和端口运行：

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**示例响应**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

版本和内部版本详细信息因环境而异。 请勿将显示的示例版本视为必需或通用服务版本。

>[!TAB 专业暂存和生产]

在专业暂存和生产环境中，运行：

```terminal
valkey-server -v
```

**示例响应**

```text
Valkey server v=<installed-version> ...
```

版本和内部版本详细信息因环境而异。 请勿将显示的示例版本视为必需或通用服务版本。

>[!ENDTABS]

## Valkey疑难解答

### Cache-clean错误引用Valkey配置的缓存上的Redis

预部署缓存清理失败可能会显示错误代码`[107]` (`clean-redis-cache`)和`Connection to Redis`消息，即使`cache`服务配置为Valkey也是如此。 无论后备缓存服务是Redis还是Valkey，`ece-tools`都会将此错误代码和消息用于缓存清理步骤。

如果基础错误是DNS故障（如关系主机的`Name or service not known`），则部署步骤在服务关系可用之前运行，或者`.magento.app.yaml`中的关系名称与`.magento/services.yaml`中的服务ID不匹配。 请参阅[验证服务关系](#verify-the-service-relationship)。
