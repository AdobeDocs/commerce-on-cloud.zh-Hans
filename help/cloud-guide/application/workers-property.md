---
title: 工作人员
description: 了解如何在 [!DNL Commerce] 应用程序配置文件中配置Worker属性。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Workers属性

您可以定义辅助进程独立于Web实例运行，而无需运行Nginx实例；但是，辅助进程使用的网络存储与[!DNL Commerce]应用程序使用的网络存储相同。 您不需要在工作线程实例上设置Web服务器（使用Node.js或Go），因为路由器无法将公共请求定向到工作线程。 这使得工作人员实例非常适合后台任务或可能阻止部署的持续运行任务。

## 配置工作人员

辅助进程只能与Pro暂存和生产环境一起使用。 Pro集成和入门环境可以选择使用[CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner)变量。

要在Pro Staging或Production中配置工作程序，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)并包含以下信息：

- 项目编号
- 环境ID
- 工作人员姓名
- 启动命令

您可以为每个工作进程配置一个进程。 `.magento.app.yaml`文件中的基本常用辅助进程配置可能如下所示：

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

此示例定义了一个名为`queue`的具有小（大小为S）级别的资源分配工作，并在启动时运行`php ./bin/magento`命令。 然后，辅助进程`queue`在每个节点上作为辅助进程运行。 如果命令退出，则会自动重新启动。

## 命令和覆盖

使用Worker应用程序启动命令需要`commands.start`键。 您可以使用任何有效的shell命令，但最好使用应用程序的语言。 如果`start`键指定的命令终止，它将自动重新启动。

>[!IMPORTANT]
>
>`deploy`和`post_deploy`挂接和`crons`命令仅在Web容器上运行，而不会在辅助进程实例中运行。

### 继承

除非显式覆盖，否则`size`、`relationships`、`access`、`disk`和`mount`以及`variables`属性的定义将由辅助进程继承。

以下属性最常用于覆盖[顶级设置](properties.md)：

- `size` — 为单个后台进程分配更少的资源
- `variables` — 指示应用程序以不同方式运行

### 计时和排队

尽管每个工作进程队列位于另一个工作进程队列之后，但以下配置会在`var/time.txt`文件中生成一致的两秒时间戳分离，而不管PHP代码中是否处于八秒睡眠状态：

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
