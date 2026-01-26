---
title: '[!DNL ECE-Tools]包'
description: 了解 [!DNL ECE-Tools] 包以及它如何帮助管理和部署Adobe Commerce。
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# ECE-Tools包

[!DNL ECE-Tools]包是旨在管理和部署[!DNL Commerce]应用程序的一组脚本和工具。 `ece-tools`包简化了许多流程，例如管理cron作业、验证项目配置以及应用Adobe修补程序和修补程序。 您可以在GitHub[上查看并贡献到 [!DNL ECE-Tools] 开源](https://github.com/magento/ece-tools)代码存储库。

{{ece-tools-package}}

`ece-tools`包与Adobe Commerce兼容（从版本2.1.4开始），并包含脚本和Adobe Commerce on cloud infrastructure命令，这些命令旨在帮助管理代码并自动构建和部署项目。

下面列出了可用的`ece-tools`命令：

```bash
php ./vendor/bin/ece-tools list
```

## 生成和部署

`ece-tools`包中包含一些命令，这些命令用于执行在云基础架构上启动Adobe Commerce的生成、部署和部署后阶段的操作。 例如，`php ./vendor/bin/ece-tools build`命令开始应用程序构建过程。

默认情况下，这些`ece-tools`命令位于[配置文件的](../application/hooks-property.md)挂接属性`.magento.app.yaml`中。

## Docker配置生成器

`ece-tools`包包含对[magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker)包的依赖关系，该包为Docker图像提供功能和配置文件，以启动云基础架构上适用于Adobe Commerce的Docker开发环境。 您还可以作为独立包运行Cloud Docker for Commerce。 请参阅[Docker开发](../dev-tools/cloud-docker.md)。

## 服务、路由和变量

您可以使用`ece-tools`包显示有关在任何云环境中使用的Base64编码的[云变量](../environment/variables-cloud.md)的详细信息。 以下命令显示所有服务、路由和变量。

```bash
php ./vendor/bin/ece-tools env:config:show
```

要显示一组特定的信息，请使用以下格式：

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` — 显示来自`MAGENTO_CLOUD_RELATIONSHIPS`环境变量的关系数据，在`services.yaml`文件中定义。
- `routes` — 使用`MAGENTO_CLOUD_ROUTES`环境变量显示项目的已配置路由。
- `variables` — 使用`MAGENTO_CLOUD_VARIABLES`环境变量显示项目的已配置变量。

`services`选项的示例输出：

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## 验证环境配置

有一组验证命令可用于帮助评估项目的配置。 有关每个向导命令的详细说明，请参阅[优化部署](../deploy/smart-wizards.md)部分中的&#x200B;_智能向导_。 `wizard:ideal-state`命令在生成阶段自动运行。 验证项目的理想状态：

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>您必须在远程云环境中运行`wizard:ideal-state`命令。 在本地开发环境中运行时，该命令始终返回`The configured state is not ideal`错误。

示例输出：

```
Ideal state is configured
```

请参阅ece-tools[的](../release-notes/cloud-tools-suite.md)发行说明。

## Adobe修补程序和自定义修补程序

`ece-tools`包包含对[magento/magento-cloud-patches](https://github.com/magento/magento-cloud-patches)包的依赖项，该包提供了Adobe修补程序和修补程序，可改进所有Adobe Commerce版本与云环境的集成，并支持快速交付关键修补程序。 “ ”还提供了自定义修补程序，可将其添加到Adobe Commerce on cloud infrastructure项目。 请参阅[应用修补程序](../development/apply-patches.md)。

