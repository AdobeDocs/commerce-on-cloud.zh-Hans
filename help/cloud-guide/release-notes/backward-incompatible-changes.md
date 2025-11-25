---
title: 向后不兼容的更改
description: 了解在升级现有云项目时的向后兼容性。
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# 向后不兼容的更改

在升级到最新版本的`ece-tools`包或其他适用于Commerce包的Cloud Tools Suite时，向后不兼容的更改可能要求您调整现有Cloud项目的云配置和流程。

## 对`ece-tools`包的更改

以前包含在`ece-tools`包中的某些功能现在在单独的包中提供。 这些包是`ece-tools`的编辑器依赖项，当您安装或更新ece-tools时，会自动安装和更新这些依赖项。

新架构不应影响您的安装或更新过程。 但是，在使用Adobe Commerce进行云基础架构项目时，您可能需要更改某些命令语法和流程。 有关详细信息，请查看以下向后不兼容的更改信息和[Cloud Tools Suite发行说明](cloud-tools-suite.md)。

### 服务版本要求更改

对于使用`ece-tools` v2002.1.0及更高版本的云项目，我们将PHP的最低版本要求从7.0.x更改为7.1.x。 如果您的环境配置指定了PHP 7.0，请更新[文件中的](../application/php-settings.md)php配置`.magento.app.yaml`。

>[!WARNING]
>
>由于PHP版本要求的更改，`ece-tools` 2002.1.0在运行Adobe Commerce 2.1.15或更高版本的云基础架构项目中仅支持Adobe Commerce。 如果您的项目使用早期版本，则必须在更新到[&#x200B; 2002.1.0之前](../development/commerce-version.md)升级`ece-tools`。

### 环境配置更改

下表提供了有关`ece-tools` v2002.1.0中已删除或弃用的环境变量和其他环境配置文件的信息。

| 项目 | 替换 |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES`变量 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS`变量 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT`变量 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK`变量 | 无。 现在，生成始终创建指向静态内容目录`pub/static`的符号链接。 |
| `build_options.ini`文件 | 使用[`.magento.env.yaml`](../application/configure-app-yaml.md)文件配置环境变量，以管理所有环境中的生成和部署操作。<p>如果构建包含`build_options.ini`文件的云环境，则构建失败。 |

### CLI命令更改

下表总结了ECE-Tools v2002.1.0中可能需要更新命令或脚本的CLI命令更改。

| 命令 | 替换 |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

在早期的ECE-Tools版本中，您可以使用`m2-ece-build`和`m2-ece-deploy`命令在`.magento.app.yaml`文件中配置部署挂接。 当您更新到v2002.1.0时，请检查`hooks`文件中的`.magento.app.yaml`配置以了解过时的命令，并根据需要替换它们。

## 云修补程序更改

- **删除已下载的修补程序**- `magento/magento-cloud-patches`包捆绑了[软件下载](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html?lang=zh-Hans)页面中的所有可用修补程序，并在您部署到云时自动应用这些修补程序。 要防止升级到ECE-Tools 2002.1.0或更高版本后出现补丁程序冲突，请删除手动下载并添加到项目中的Adobe提供的任何补丁程序。

- **正在更新应用修补程序命令** — 我们将用于应用修补程序的命令从`vendor/bin/ece-tools`目录移动到`vendor/bin/ece-patches`目录。 如果使用此命令手动应用修补程序，请使用新路径。

  > 手动应用修补程序

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker更改

- **PHP的最低版本要求现在是PHP 7.1** — 如果您的Cloud Docker for Commerce主机运行的是早期版本，请升级到PHP v7.1或更高版本。

- **适用于Commerce的Cloud Docker命令更改**-

   - **正在为Docker生成操作更新Commerce命令的Cloud Docker** — 我们将Commerce命令的Cloud Docker从`vendor/bin/ece-tools`目录移动到`vendor/bin/ece-docker`目录。 更新脚本和命令以使用新路径。

     升级到`ece-tools` 2002.1.0后，使用以下命令查看可用的`ece-docker`命令。

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **正在更新Cloud Docker-compose命令** — 我们已将命令文件的路径从`./bin/docker`重命名为`./bin/magento-docker`。 更新脚本和命令以使用新路径。

   - **Cron容器不再包含在默认Docker配置中** — 现在，您必须将`--with-cron`选项添加到`ece-docker build:compose`命令以在Docker环境配置中包含Cron容器。 请参阅[Cloud Docker for Commerce](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs)指南中的&#x200B;_管理cron作业_。

     以前使用cron作业生成的容器现在不包含cron容器。

   - **使用临时容器** — 在以前的版本中，未删除由`bin/magento-docker`命令操作创建的容器，因此您可以将它们用于其他操作。 现在，`magento-docker`命令会移除在该命令完成后创建的所有容器。

     如果要保留通过Docker撰写操作创建的容器，请使用`docker-compose run`命令而不是`bin/magento-docker`命令。

   - **正在运行部署后挂接**- `cloud-deploy`命令不再运行部署后挂接。 使用新的`cloud-post-deploy`命令在部署后运行部署后挂接。 更新脚本以添加命令以运行部署后挂接。

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     或者，如果您直接使用`docker-compose`命令，请在部署命令之后运行`docker-compose run deploy cloud-post-deploy`命令。

- **正在刷新数据库** — 数据库容器现在存储在`magento-db`永久Docker卷中。 刷新Docker环境时，不再自动删除数据库。 如果需要，请使用以下命令之一手动删除它。

   - 删除`magento-db`容器：

     ```bash
     docker volume rm magento-db
     ```

   - 关闭Docker容器时删除所有关联的卷：

     ```bash
     docker-compose down -v
     ```

- **覆盖存档和备份文件的文件同步设置** — 使用docker-sync或mutagen时，具有以下扩展名的存档和备份文件不再同步：SQL、GZ、ZIP和BZ2。 您可以通过重命名文件以其他扩展名结尾，来覆盖这些文件类型的默认文件同步。 例如： `synchronize-me.zip-backup`
