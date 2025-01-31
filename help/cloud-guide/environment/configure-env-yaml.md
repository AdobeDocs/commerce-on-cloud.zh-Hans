---
title: 配置环境
description: 了解如何使用环境变量在云基础架构环境（包括Pro Staging和Production）上跨所有Commerce配置构建和部署操作。
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# 配置环境变量以进行部署

`.magento.env.yaml`文件使用环境变量集中管理所有环境（包括Pro Staging和Production）中的生成和部署操作。 要在每个环境中配置唯一的操作，必须在每个环境中修改此文件。

>[!TIP]
>
>YAML文件区分大小写，不允许使用制表符。 请注意在整个`.magento.env.yaml`文件中使用一致的缩进，否则您的配置可能无法按预期工作。 文档和示例文件中的示例使用&#x200B;_双空格_&#x200B;缩进。 使用[ece-tools验证命令](#validate-configuration-file)检查您的配置。

## 文件结构

`.magento.env.yaml`文件包含两个部分：`stage`和`log`。 `stage`部分控制[云部署进程](../deploy/process.md)的阶段中发生的操作。

- `stage` — 使用阶段部分为以下部署阶段定义某些操作：
   - `global` — 控制生成、部署和部署后阶段的操作。 您可以在“生成”、“部署”和“部署后”部分中覆盖这些设置。
   - `build` — 仅在生成阶段控制操作。 如果未在此部分中指定设置，则构建阶段将使用全局部分中的设置。
   - `deploy` — 仅在部署阶段控制操作。 如果未在此部分中指定设置，则部署阶段将使用全局部分中的设置。
   - `post-deploy` — 控制部署应用程序后&#x200B;_的操作_&#x200B;和&#x200B;_容器开始接受连接_。
- `log` — 使用日志部分配置[通知](set-up-notifications.md)，包括通知类型和详细级别。
   - `slack` — 配置要发送给Slack机器人的消息。
   - `email` — 配置要发送给一个或多个电子邮件收件人的电子邮件。
   - [日志处理程序](log-handlers.md) — 配置发送到远程日志服务器的硬件和软件应用程序消息。

### 环境变量

`ece-tools`包根据[Cloud变量](variables-cloud.md)、[!DNL Cloud Console]中设置的变量和`.magento.env.yaml`配置文件中的值在`env.php`文件中设置值。 `.magento.env.yaml`文件中的环境变量通过覆盖现有Commerce配置来自定义云环境。 如果默认值为`Not Set`，则`ece-tools`包将采取&#x200B;**NO**&#x200B;操作并使用[!DNL Commerce]默认值或MAGENTO_CLOUD_RELATIONSHIPS配置中的值。 如果设置了默认值，则`ece-tools`包将采取行动来设置该默认值。

以下主题包含您可以在`.magento.env.yaml`文件中使用的所有变量的详细定义，例如是否设置了默认值：

- [全局](variables-global.md) — 变量控制每个阶段的操作：生成、部署和部署后
- [生成](variables-build.md) — 变量控制生成操作
- [部署](variables-deploy.md) — 变量控制部署操作
- [部署后](variables-post-deploy.md) — 部署后的变量控制操作

### 从CLI创建配置文件

您可以使用以下`ece-tools`命令为云环境生成`.magento.env.yaml`配置文件。

>创建配置文件

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>更新配置文件中的值

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

两个命令都需要一个参数：为至少一个生成、部署或部署后变量指定值的JSON格式数组。 例如，以下命令设置`SCD_THREADS`和`CLEAN_STATIC_FILES`变量的值：

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

并使用以下设置创建一个`.magento.env.yaml`文件：

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

您可以使用`cloud:config:update`命令更新新文件。 例如，以下命令更改`SCD_THREADS`值并添加`SCD_COMPRESSION_TIMEOUT`配置：

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

更新的文件包含以下配置：

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### 验证配置文件

在将更改推送到远程云环境之前，请使用以下`ece-tools`命令验证`.magento.env.yaml`配置文件。

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

以下示例响应提供了要更正的项目列表：

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## php常量

您可以在`.magento.env.yaml`文件定义中使用PHP常量，而不是硬编码值。 以下示例使用PHP常量定义`driver_options`：

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>使用低于3.2版本的`symfony/yaml`包时，常量分析不起作用。

## 错误处理

当由于`.magento.env.yaml`配置文件中的意外值而发生失败时，您会收到一条错误消息。 例如，以下错误消息显示每个具有意外值的项目的建议更改列表，有时会提供有效选项：

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

进行任何更正，提交并推送更改。 如果没有收到错误消息，则对配置文件的更改将通过验证。

## 配置管理优化

如果在转储配置后启用了配置管理，则应将SCD_*变量从部署移动到构建阶段。 请参阅[静态内容部署策略](../deploy/static-content.md)。

>在配置管理之前：

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>启用配置管理后，将SCD_*变量移动到构建阶段：

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
