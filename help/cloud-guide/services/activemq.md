---
title: 设置ActiveMQ服务
description: 了解如何启用ActiveMQ Artemis服务来管理云基础架构上Adobe Commerce的消息队列。
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# 设置[!DNL ActiveMQ]服务

[Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)是Adobe Commerce中的系统，它允许[模块](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)将消息发布到队列。 它还定义了异步接收消息的消费者。

MQF可以使用[ActiveMQ Artemis](https://activemq.apache.org/components/artemis/)作为消息代理，该消息代理为发送和接收消息提供了一个可伸缩的平台。 它还包括用于存储未传递消息的机制。 [!DNL ActiveMQ Artemis]支持用于消息传送的STOMP（流式文本导向消息传送协议）协议。

[!DNL ActiveMQ Artemis]可用作替代RabbitMQ来管理消息队列。 当您需要特定于ActiveMQ的功能或希望使用STOMP协议时，它特别有用。

>[!IMPORTANT]
>
>如果您希望使用现有的消息代理服务（如[!DNL ActiveMQ]），而不是依赖云基础架构上的Adobe Commerce来为您创建它，请使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)环境变量将其连接到您的站点。

{{service-instruction}}

**启用ActiveMQ Artemis**：

1. 将所需的名称、类型和磁盘值（以MB为单位）与安装的ActiveMQ版本一起添加到`.magento/services.yaml`文件中。

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. 在`.magento.app.yaml`文件中配置关系。

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 连接到ActiveMQ以进行调试

出于调试目的，您可以通过以下方式之一直接连接到服务实例：

- 从本地开发环境连接
- 从应用程序连接
- 从PHP应用程序连接

### 从本地开发环境连接

1. 登录到`magento-cloud` CLI和项目：

   ```bash
   magento-cloud login
   ```

1. 查看安装并配置了ActiveMQ Artemis的环境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 使用SSH连接到云环境：

   ```bash
   magento-cloud ssh
   ```

1. 从[$MAGENTO_CLOUD_RELATIONSHIP](../application/properties.md#relationships)变量检索ActiveMQ连接详细信息和登录凭据：

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在响应中，查找ActiveMQ信息。 关系名称取决于您在`.magento.app.yaml`文件中的配置方式。 例如，如果您使用`activemq-artemis`作为关系名称：

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. 启用到ActiveMQ的本地端口转发（如果您的项目位于不同的区域，例如US-3、EU-5或AP-3区域，请将``us-3``替换为``eu-5``/``ap-3``/``us``）

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   访问`http://localhost:8161`处的ActiveMQ Artemis Web控制台的示例为：

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis使用端口61616进行STOMP消息传递，使用端口8161进行Web控制台。

1. 在会话打开时，您可以使用MAGENTO_CLOUD_RELATIONSHIPS变量的用户名和密码访问`http://localhost:8161`上的ActiveMQ Artemis Web控制台。

### 从应用程序连接

要连接到在应用程序中运行的ActiveMQ，请在PHP应用程序中安装STOMP客户端库。

### 从PHP应用程序连接

要使用PHP应用程序连接到ActiveMQ，请将PHP STOMP库添加到源树中。 Adobe Commerce使用STOMP协议进行ActiveMQ通信，并且当检测到ActiveMQ Artemis为配置的服务时，会在部署期间自动设置配置。

## 协议支持

云基础架构上Adobe Commerce中的ActiveMQ Artemis使用STOMP（流文本导向消息协议）协议：

- **STOMP**：用于队列操作的消息协议(端口61616)
- **Web控制台**：可通过HTTP访问管理界面（端口8161）

## 与RabbitMQ的差异

虽然[!DNL ActiveMQ Artemis]和[!DNL RabbitMQ]都用作Adobe Commerce的消息代理，但存在一些差异：

- **协议**： ActiveMQ Artemis使用STOMP协议，而RabbitMQ使用AMQP
- **配置**：在配置ActiveMQ Artemis时，Adobe Commerce自动使用STOMP协议
- **优先级**：如果同时配置了ActiveMQ和RabbitMQ，则ActiveMQ优先于基于STOMP的操作，而AMQP操作使用RabbitMQ
- **Web控制台**： ActiveMQ提供基于Web的管理控制台，用于监视队列和消息

## 队列配置

将ActiveMQ Artemis配置为服务时，Adobe Commerce会自动将队列系统配置为使用STOMP协议。 在部署期间，配置将写入`app/etc/env.php`文件：

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

如果需要，您可以使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)环境变量覆盖此配置。

