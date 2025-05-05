---
title: 存储配置管理
description: 了解如何在云基础架构环境的所有Adobe Commerce中管理和同步存储配置设置。
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# 存储配置管理

存储区的默认配置存储在相应模块的`config.xml`中。 当您在Commerce Admin或CLI `bin/magento config:set`命令中更改设置时，这些更改将反映在核心数据库中，特别是`core_config_data`表中。 这些设置将覆盖`config.xml`文件中存储的默认配置。

存储设置（引用管理员&#x200B;**存储** > **设置** > **配置**&#x200B;节中的配置）基于配置类型存储在部署配置文件中：

- `app/etc/config.php` — 与静态内容部署相关的商店、网站、模块或扩展名的配置设置、静态文件优化和系统值。 请参阅&#x200B;_配置指南_&#x200B;中的[config.php引用](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html)。
- `app/etc/env.php` — 系统特定覆盖和敏感设置的值，这些值应该&#x200B;_NOT_&#x200B;存储在源代码管理中。 请参阅&#x200B;_配置指南_&#x200B;中的[env.php引用](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html)。

>[!NOTE]
>
>由于云基础架构上的Adobe Commerce仅支持生产和维护模式，因此&#x200B;**高级** > **开发人员**&#x200B;部分无法在管理员中访问。 您必须具有[环境管理员权限](../project/user-access.md)才能完成配置管理任务。 您可以使用[环境变量](../environment/configure-env-yaml.md)配置其他设置。

配置管理提供了一种使用Pipeline部署以最小的停机时间跨环境部署一致存储设置的方法。 云基础架构项目上的Adobe Commerce包括构建服务器、生成和部署脚本，以及设计有[管道部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html)的部署环境。

## 配置覆盖方案

所有系统配置都根据以下覆盖方案在构建和部署阶段进行设置：

1. 如果存在环境变量，请使用自定义配置并忽略默认配置。
1. 如果环境变量不存在，请使用[`.magento.app.yaml`文件](../application/configure-app-yaml.md)中`MAGENTO_CLOUD_RELATIONSHIPS`名称 — 值对的配置。 忽略默认配置。
1. 如果环境变量不存在，`MAGENTO_CLOUD_RELATIONSHIPS`不包含名称 — 值对，请删除所有自定义配置并使用默认配置中的值。

总之，环境变量将覆盖所有其他值。

>[!TIP]
>
>有关管道部署的覆盖方案的详细信息，请参阅&#x200B;_配置指南_&#x200B;中的[配置管理](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html)。

如果在多个位置配置了相同的设置，则应用程序将依靠以下配置层次结构来确定将哪个值应用于环境：

| 优先级 | 配置<br>方法 | 描述 |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>环境变量 | 从[!DNL Cloud Console]中环境配置的&#x200B;_变量_&#x200B;选项卡添加的值。 在此处为敏感配置或特定于环境的配置指定值。 无法从管理员编辑此处指定的设置。 请参阅[环境配置变量](../project/overview.md#configure-environment)。 |
| 2 | `.magento.app.yaml` | 在`.magento.app.yaml`文件的`variables`部分中添加的值。 在此处指定值可确保在所有环境中进行一致的配置。 **请勿在`.magento.app.yaml`文件中指定敏感值。**&#x200B;查看[应用程序设置](../application/configure-app-yaml.md)。 |
| 3 | `app/etc/env.php` | 使用`app:config:dump`命令添加此处存储的特定于环境的配置值。 使用环境变量或CLI设置特定于系统的敏感值。 查看[敏感数据](#sensitive-data)。 `env.php`文件是&#x200B;**不包括在源代码管理中的**。 |
| 4 | `app/etc/config.php` | 使用`app:config:dump`命令添加此处存储的值。 共享配置值已添加到`config.php`。 从管理员或使用CLI设置共享配置。 `config.php`文件包含在源代码管理中。 |
| 5 | 数据库 | 通过在管理员中设置配置来添加此处存储的值。 使用以上任何方法设置的配置将被锁定（灰显），并且无法从管理员中进行编辑。 |
| 6 | `config.xml` | 许多配置在`config.xml`文件中为模块设置了默认值。 如果Adobe Commerce找不到通过以上任何方法设置的任何值，则会回退到默认值（如果已设置）。 |

{style="table-layout:auto"}

## 配置转储

您可以使用以下`ece-tools`命令生成包含所有当前存储配置的`config.php`文件：

```bash
./vendor/bin/ece-tools config:dump
```

“转储”到`app/etc/config.php`文件的数据变为&#x200B;_锁定_，这意味着Commerce管理员中的相应字段变为&#x200B;**只读**。 `config.php`文件仅包含您配置的设置。 它不会锁定默认值。 仅锁定您更新的值还可以确保暂存环境和生产环境中使用的所有扩展不会由于只读配置（尤其是Fastly）而中断。

>[!WARNING]
>
>`ece-tools config:dump`命令不会检索模块的详细配置，如B2B。 如果需要完整的配置转储，请使用`app:config:dump`命令，但此命令将配置值锁定在只读状态。

### 敏感数据

使用`bin/magento app:config:dump`命令时将任何敏感配置导出到`app/etc/env.php`文件。 您可以使用CLI命令设置敏感值： `bin/magento config:sensitive:set`。 请参阅&#x200B;_Commerce PHP扩展_&#x200B;指南中的[敏感设置和特定于环境的设置](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/)，了解如何将配置设置指定为敏感设置或特定于系统。

请参阅&#x200B;_配置指南_&#x200B;中的[敏感或系统特定的设置](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html)列表。

### SCD性能

根据存储的大小，您可能要部署大量静态内容文件。 通常，当应用程序处于维护模式时，静态内容会在部署阶段进行部署。 最佳配置是在构建阶段生成静态内容。 请参阅[选择部署策略](../deploy/static-content.md)。

如果在转储配置后启用了配置管理，则应将SCD_*变量从部署阶段移动到构建阶段，以便在构建阶段正确启用静态内容生成。 查看[环境变量](../environment/configure-env-yaml.md#environment-variables)。

配置管理前&#x200B;**&#x200B;**：

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

**启用配置管理后**：

将SCD_*变量移至构建阶段：

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

>[!NOTE]
>
>在部署静态文件之前，构建和部署阶段使用GZIP压缩静态内容。 压缩静态文件可减少服务器负载并提高站点性能。 请参阅[生成选项](../environment/variables-build.md)，了解有关自定义或禁用文件压缩的信息。

## 管理设置的过程

下面说明了此过程的高级概述：

![入门配置管理概述](../../assets/starter/configuration-management-flow.png)

**要配置存储并生成配置文件**：

1. 在管理员中为您的商店完成以下环境之一的所有配置：

   - 入门：活跃的开发分支
   - Pro：集成环境中的活动分支

   除非您计划将数据库从此环境转储到暂存环境和生产环境，否则这些配置不包括实际产品。 通常，开发数据库不包含您的完整存储数据。

1. 在本地工作站上，转到您的项目目录。

1. 创建远程数据库的本地转储。

   ```bash
   magento-cloud db:dump
   ```

1. 添加、提交和推送代码更改以更新远程环境。

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

部署完成后，登录到已更新环境的管理员以验证设置。 根据需要继续将任何其他配置合并到暂存环境和生产环境。

### 更新配置

当您通过管理员修改环境并再次运行命令时，新配置将附加到`config.php`文件中的代码中。

>[!WARNING]
>
>虽然您可以在暂存环境和生产环境中手动编辑`config.php`文件，但建议&#x200B;**不要**。 该文件有助于使所有环境中的所有配置保持一致。 永远不要删除`config.php`文件以对其进行重建。 删除文件可能会删除生成和部署过程所需的特定配置和设置。

### 还原配置文件

在部署过程中创建了原始`app/etc/env.php`和`app/etc/config.php`文件的副本，并将其存储在同一文件夹中。 下面显示了同一`app/etc`文件夹中的BAK（备份文件）和PHP（原始文件）：

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

较旧的配置使用了`app/etc/config.local.php`文件。 请参阅[迁移旧配置](#migrate-older-configurations)。

**要还原配置文件**：

1. 在本地工作站上，使用SSH登录到远程项目和环境。

   ```bash
   magento-cloud ssh
   ```

1. 验证备份文件的位置和可用性。

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   示例响应：

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. 还原备份文件。

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### 迁移旧配置

如果您在云基础架构2.2或更高版本上升级到Adobe Commerce，则可能需要将设置从`config.local.php`文件迁移到新的`config.php`文件。 如果管理员中的配置设置与文件的内容匹配，请按照说明生成并添加`config.php`文件。

如果两者不同，则可以将来自`config.local.php`文件的内容附加到新的`config.php`文件：

1. 按照说明生成`config.php`文件。

1. 打开`config.php`文件并删除最后一行。

1. 打开`config.local.php`文件并复制其内容。

1. 将内容粘贴到`config.php`文件中，保存并完成将其添加到Git中。

1. 跨环境部署。

您只需完成一次此迁移。 迁移后，使用`config.php`文件。

### 更改区域设置

您无需遵循复杂的配置导入和导出过程即可更改商店区域设置，_如果_&#x200B;您启用了[SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand)。 您可以使用管理员更新区域设置。

通过在集成分支中启用`SCD_ON_DEMAND`，可以向暂存或生产环境添加其他区域设置，生成具有新区域设置信息的更新`config.php`文件，并将配置文件复制到目标环境。

>[!WARNING]
>
>此进程&#x200B;**覆盖**&#x200B;存储配置；仅当环境包含相同的存储时，才执行以下操作。

1. 在集成环境中，使用[`.magento.env.yaml`文件](../environment/configure-env-yaml.md)启用`SCD_ON_DEMAND`变量。

1. 使用您的管理员添加必要的区域设置。

1. 使用SSH登录到远程环境并生成包含所有区域设置的`/app/etc/config.php`文件。

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. 将新的配置文件从远程集成环境复制到本地环境目录。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 添加、提交和推送代码更改以更新远程环境。
