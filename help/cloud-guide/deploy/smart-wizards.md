---
title: 智能向导
description: 了解如何使用智能向导来评估您在云基础架构项目上的Adobe Commerce是否遵循部署最佳实践。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 智能向导

智能向导可帮助您确定云配置是否遵循最佳实践。 可用的向导有助于进行以下配置：

- 最少的部署停机时间的理想状态
- 数据库和Redis的负载平衡配置
- 用于按需、构建阶段或部署阶段的静态内容部署(SCD)

每个Smart Wizard命令都会提供验证响应，并在适用的情况下提供正确配置的建议。

| 命令 | 描述 |
| ------- | ------------|
| `wizard:ideal-state` | 检查SCD是否位于&#x200B;_生成_&#x200B;阶段、`SKIP_HTML_MINIFICATION`变量是否为`true`以及在云环境中配置的post_deploy挂接。 不用于本地开发环境。 |
| `wizard:master-slave` | 检查`REDIS_USE_SLAVE_CONNECTION`变量和`MYSQL_USE_SLAVE_CONNECTION`变量是否为`true`。 |
| `wizard:scd-on-demand` | 检查`SCD_ON_DEMAND`全局环境变量是否为`true`。 |
| `wizard:scd-on-build` | 检查&#x200B;_内部版本_&#x200B;阶段的`SCD_ON_DEMAND`全局环境变量是`false`，`SKIP_SCD`环境变量是`false`。 验证`config.php`文件是否包含商店、商店组和网站的信息。 |
| `wizard:scd-on-deploy` | 检查&#x200B;_部署_&#x200B;阶段的`SCD_ON_DEMAND`全局环境变量是`false`，`SKIP_SCD`环境变量是`false`。 验证`config.php`文件是否包含&#x200B;_NOT_&#x200B;包含包含相关信息的商店、商店组和网站的列表。 |

例如，您可以验证配置是否正确启用SCD按需功能：

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

成功的配置将返回：

```
SCD on-demand is enabled
```

失败的配置返回：

```
SCD on-demand is disabled
```

## 验证理想配置

云项目的&#x200B;_理想_&#x200B;配置有助于通过预热缓存并在用户请求时生成静态内容来最大程度地缩短部署停机时间。 此向导在部署过程中自动运行。 如果您的云未针对此&#x200B;_理想状态_&#x200B;进行配置，则会收到类似于以下内容的消息：

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

根据输出，您需要对配置进行以下更正：

1. 启用跳过HTML缩小变量。

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. 配置部署后挂接。

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. 推送代码更改并重新运行测试。 当您的配置是&#x200B;_理想的_&#x200B;时，您会收到以下消息。

   ```
   Ideal state is configured
   ```
