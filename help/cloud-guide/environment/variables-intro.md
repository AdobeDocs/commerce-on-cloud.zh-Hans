---
title: 环境变量
description: 请参阅云基础架构上特定于Adobe Commerce的环境变量列表。
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 环境变量

云基础架构上的Adobe Commerce允许您分配环境变量以覆盖配置选项。 `ece-tools`包根据[Cloud变量](variables-cloud.md)、[!DNL Cloud Console]中设置的变量和`.magento.env.yaml`配置文件中的值在`env.php`文件中设置值。

`.magento.env.yaml`文件中的环境变量通过覆盖现有Commerce配置来自定义云环境。 如果默认值是`Not Set`，则`ece-tools`包将采取&#x200B;**NO**&#x200B;操作并使用[!DNL Commerce]默认值或来自`MAGENTO_CLOUD_RELATIONSHIPS`配置的值。 如果设置了默认值，则`ece-tools`包将采取行动来设置该默认值。

环境变量的类型包括：

- [管理员](variables-admin.md) — 变量覆盖项目管理员变量
- [MAGENTO_云](variables-cloud.md) — 特定于云基础架构的变量
- `.magento.env.yaml`文件中使用的变量：
   - [全局](variables-global.md) — 变量会影响生成、部署和部署后阶段
   - [生成](variables-build.md) — 变量控制生成操作
   - [部署](variables-deploy.md) — 变量控制部署操作
   - [部署后](variables-post-deploy.md) — 部署后的变量控制操作

变量为&#x200B;_层级_，这意味着如果变量未被覆盖，则它继承自父环境。

您可以从[!DNL Cloud Console]或使用Adobe Commerce CLI设置[管理员变量](variables-admin.md)。 您可以通过[`.magento.env.yaml`](configure-env-yaml.md)文件管理其他环境变量，以管理所有环境（包括Pro暂存环境和生产环境）中的构建和部署操作，而无需支持票证。

>[!TIP]
>
>YAML文件区分大小写，不允许使用制表符。 请注意在整个`.magento.env.yaml`文件中使用一致的缩进，否则您的配置可能无法按预期工作。 此文档和示例文件中的示例使用&#x200B;_双空格_&#x200B;缩进。 使用[ece-tools验证命令](configure-env-yaml.md#validate-configuration-file)检查您的配置。
