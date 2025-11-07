---
title: 设置RabbitMQ服务
description: 了解如何启用RabbitMQ服务来管理云基础架构上Adobe Commerce的消息队列。
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 设置[!DNL RabbitMQ]服务

[Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)是Adobe Commerce中的系统，它允许[模块](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)将消息发布到队列。 它还定义了异步接收消息的消费者。

MQF使用[RabbitMQ](https://www.rabbitmq.com/)作为消息代理，该消息代理提供了一个用于发送和接收消息的可伸缩平台。 它还包括用于存储未传递消息的机制。 [!DNL RabbitMQ]基于高级消息队列协议(AMQP) 0.9.1规范。

>[!NOTE]
>
>云基础架构上的Adobe Commerce还支持将[ActiveMQ Artemis](activemq.md)作为使用STOMP协议的替代消息队列服务。

>[!IMPORTANT]
>
>如果您希望使用现有的基于AMQP的服务（如[!DNL RabbitMQ]），而不是依靠Adobe Commerce基础架构为您创建，请使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)环境变量将其连接到您的站点。

{{service-instruction}}

**启用RabbitMQ**：

1. 将所需的名称、类型和磁盘值（以MB为单位）与安装的RabbitMQ版本一起添加到`.magento/services.yaml`文件。

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. 在`.magento.app.yaml`文件中配置关系。

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 连接到RabbitMQ以进行调试

出于调试目的，通过下列方式之一直接连接到服务实例会很有用：

- 从本地开发环境连接
- 从应用程序连接
- 从PHP应用程序连接

### 从本地开发环境连接

1. 登录到`magento-cloud` CLI和项目：

   ```bash
   magento-cloud login
   ```

1. 查看已安装并配置RabbitMQ的环境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 使用SSH连接到云环境：

   ```bash
   magento-cloud ssh
   ```

1. 从[$MAGENTO_CLOUD_RELATIONSHIP](../application/properties.md#relationships)变量中检索RabbitMQ连接详细信息和登录凭据：

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在响应中，查找RabbitMQ信息，例如：

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. 启用到RabbitMQ的本地端口转发（如果您的项目位于不同的区域，例如US-3、EU-5或AP-3区域，请将``us-3``替换为``eu-5``/``ap-3``/``us``）

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   访问`http://localhost:15672`的RabbitMQ管理Web界面的示例为：

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. 会话打开时，您可以从本地工作站启动您选择的RabbitMQ客户端，该客户端配置为使用MAGENTO_CLOUD_RELATIONSHIPS变量中的端口号、用户名和密码连接到`localhost:<portnumber>`。

### 从应用程序连接

要连接到在应用程序中运行的RabbitMQ，请在[文件中安装客户端（如](https://github.com/dougbarth/amqp-utils)amqp-utils`.magento.app.yaml`）作为项目依赖项。

例如，

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

登录PHP容器时，输入可用于管理队列的任何`amqp-`命令。

### 从PHP应用程序连接

要使用PHP应用程序连接到RabbitMQ，请将PHP库添加到源树中。

## [!DNL RabbitMQ]服务疑难解答

请参阅[无法连接到Adobe Commerce Cloud中的RabbitMQ](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27688)。

## 正在升级[!DNL RabbitMQ]服务

有关升级说明，请参阅[更改服务版本](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version)。
