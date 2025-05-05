---
title: 变量级别和选项
description: 了解在云基础架构项目运行时环境中自定义Adobe Commerce时使用的各种变量级别和选项。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 变量级别

项目变量适用于项目中的所有环境。 环境变量适用于特定环境或分支。 环境&#x200B;_从父环境继承_&#x200B;变量定义。

您可以通过专门为环境定义变量来覆盖继承的值。 例如，要设置开发变量，请在集成环境的`.magento.env.yaml`文件中定义变量值。 从集成环境分支的所有环境都会继承这些值。 有关使用`.magento.env.yaml`文件配置环境的详细信息，请参阅[部署配置](configure-env-yaml.md)。

>[!BEGINTABS]

>[!TAB CLI]

**要使用Cloud CLI设置变量**：

- **项目特定的变量** — 为项目中的&#x200B;_所有_&#x200B;环境设置相同的值。 这些变量在所有环境中都可在生成和运行时使用。

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **特定于环境的变量** — 为&#x200B;_特定_&#x200B;环境设置唯一值。 这些变量在运行时可用，并由子环境继承。 使用`-e`选项在命令中指定环境。

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

设置特定于项目的变量后，必须手动重新部署远程环境，以使更改生效。 推送新的承诺以触发重新部署。

>[!TAB 控制台]

**要使用[!DNL Cloud Console]**&#x200B;设置变量：

1. 在&#x200B;_[!DNL Cloud Console]_&#x200B;中，单击项目导航右侧的配置图标。

   ![配置项目](../../assets/icon-configure.png){width="36"}

1. 要设置项目级别的变量，请在&#x200B;_项目设置_&#x200B;下单击&#x200B;**变量**。

   ![项目变量](../../assets/ui-project-variables.png)

1. 要设置环境级变量，请在&#x200B;_环境_&#x200B;列表中，选择一个环境，然后单击&#x200B;**[!UICONTROL Variables]**&#x200B;选项卡。

   ![环境变量选项卡](../../assets/ui-environment-variables.png)

1. 单击&#x200B;**[!UICONTROL Create variable]**。

1. 提供变量的名称和值。 从以下选项中进行选择：

   - 运行时可用
   - 在构建期间可用
   - JSON值
   - 敏感变量（隐藏在控制台和CLI响应中的值）
   - 使可继承（子环境可以继承环境级变量）

1. 单击&#x200B;**[!UICONTROL Create variable]**。

>[!CAUTION]
>
>在[!DNL Cloud Console]中设置特定于环境的变量会自动重新部署环境。

>[!ENDTABS]

## 可见性

您可以使用`--visible-<build|runtime>`命令限制变量在生成或运行时的可见性。 此外，还可以选择设置继承和敏感度。

使用以下选项可防止查看或继承变量：

- `--inheritable false` — 禁用子环境的继承。 这对于在`master`分支上设置仅用于生产的值以及允许所有其他环境使用同名的项目级变量非常有用。
- `--sensitive true` — 在[!DNL Cloud Console]中将变量标记为&#x200B;_不可读_。 您不能在用户界面中查看变量；但是，您可以从应用程序容器中查看变量，就像任何其他变量一样。

下面演示了阻止查看或继承变量的特定情况。 只能在CLI中指定这些选项。 此案例并非适用于所有可用的环境变量。

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## 验证变量级别和值

您可以使用Cloud CLI查看现有变量的列表。

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
