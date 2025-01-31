---
title: 全局变量
description: 请参阅环境变量列表，这些变量可控制Adobe Commerce对云基础架构部署过程的操作。
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# 全局变量

全局变量控制[!DNL Commerce]部署过程的每个阶段的操作：生成、部署和部署后。 由于全局变量会影响每个阶段，因此您必须在`.magento.env.yaml`文件的`global`阶段中设置它们：

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

有关自定义生成和部署过程的详细信息：

- [部署配置](configure-env-yaml.md)
- [部署过程](../deploy/process.md)

## `ENABLE_EVENTING`

- **默认值**-_未设置_
- **版本**—Adobe Commerce 2.4.5及更高版本

当设置为`true`时，允许cron运行消息队列使用者。 适用于Adobe Commerce的Adobe I/O Events使用消息队列来加快严重事件的交付。

Adobe建议您还将[`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner)变量添加到`.magento.env.yaml`文件的`deploy`阶段（其中`cron_run`设置为`true`）。

以下示例显示完全配置的`ENABLE_EVENTING`变量。

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **默认值**-_未设置_
- **版本**—Adobe Commerce 2.4.4及更高版本

当设置为`true`时，启用Commerce Webhook。 webhook在外部端点上运行，例如App Builder运行时操作或第三方清单管理系统。 [_Webhooks指南_](https://developer.adobe.com/commerce/extensibility/webhooks)详细描述了此功能。

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

覆盖所有输出流的最低日志记录级别，而不更改代码，这在排除部署问题时很有用。 例如，如果您的部署失败，则可以使用此变量全局增加日志记录粒度。 请参阅[日志级别](log-handlers.md#log-levels)。 日志记录处理程序中的`min_level`值将覆盖此设置。

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>`MIN_LOGGING_LEVEL`变量的设置不会更改文件处理程序的日志级别配置，默认情况下设置为`debug`。

## `SCD_ON_DEMAND`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

在用户请求时启用生成静态内容(SCD)。 按需静态内容由于缩短了部署时间，因此非常适合用于开发和测试工作流。

使用[`post_deploy`挂接](../application/hooks-property.md)预加载缓存可减少网站停机时间。 缓存预热仅适用于[!DNL Cloud Console]中包含暂存和生产环境的Pro项目以及入门项目。 将`SCD_ON_DEMAND`环境变量添加到`.magento.env.yaml`文件中的`global`阶段：

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

`SCD_ON_DEMAND`变量在两个阶段（生成和部署）都跳过SCD，清除`pub/static`和`var/view_preprocessed`文件夹，并将以下内容写入`app/etc/env.php`文件：

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.2.0及更高版本

允许您增加静态内容部署的最大预期执行时间。

默认情况下，Adobe Commerce将最大预期执行时间设置为900秒，但在某些情况下，您可能需要更多时间来完成Cloud项目的静态内容部署。

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.4.2及更高版本

设置为`true`以防止在生成和部署阶段为父主题生成静态内容。 当此选项设置为`true`时，生成的静态内容较少，这缩短了总体生成和部署时间。

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.3.0及更高版本

[Baler](https://github.com/magento/baler)是一个模块，用于扫描生成的JavaScript代码并创建优化的JavaScript捆绑包。 将优化的捆绑包部署到您的站点可以减少加载您的站点时的网络请求数并缩短页面加载时间。

设置为`true`可在执行静态内容部署后运行过滤器。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>在使用此功能之前，请安装和配置栏位模块。 由于包是在Alpha版本中，因此仅在暂存环境中启用此选项。

## `SKIP_HTML_MINIFICATION`

- **默认值**：
   - `true` — 适用于`ece-tools` 2002.0.13及更高版本
   - `false` — 适用于早期版本的`ece-tools`
- **版本**—Adobe Commerce 2.1.4及更高版本

启用或禁用在生成阶段结束时将静态视图文件复制到`<magento_root>/init/`目录。 如果设置为`true`，将不会复制文件，并且HTML缩小功能可应请求使用。 将此值设置为`true`可减少部署到暂存和生产环境时的停机时间。

- **`false`** — 在生成阶段结束时将`view_preprocessed`目录复制到`<magento_root>/init/`目录，并在部署阶段开始时恢复`<magento_root>/var`目录中的目录。
- **`true`** — 启用随选HTML缩小；在生成阶段结束时&#x200B;_不_&#x200B;将`<magento_root>var/view_preprocessed`复制到`<magento_root>/init/`目录。

将`SKIP_HTML_MINIFICATION`环境变量添加到`.magento.env.yaml`文件中的`global`阶段：

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

使用`X_FRAME_CONFIGURATION`变量更改Adobe Commerce站点的[`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html)标头配置。 此配置控制浏览器如何呈现`<frame>`、`<iframe>`或`<object>`中的页面。 使用以下选项之一：

- `DENY` — 页面无法显示在框架中。
- `SAMEORIGIN`—(默认Adobe Commerce设置。) 页面只能在与页面本身同源的框架中显示。

>[!WARNING]
>
>`ALLOW-FROM <uri>`选项已被弃用，因为Adobe Commerce支持的浏览器不再支持它。 查看[浏览器兼容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)。

将`X_FRAME_CONFIGURATION`环境变量添加到`.magento.env.yaml`文件中的`global`阶段：

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
