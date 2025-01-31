---
title: 项目结构
description: 了解云基础架构上Adobe Commerce的文件结构和项目模板。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 项目结构

云基础架构项目上的Adobe Commerce包括凭据和应用程序配置的基本文件。 根据Adobe Commerce版本，这些文件在中作为模板提供。 在[`magento/magento-cloud` GitHub存储库](https://github.com/magento/magento-cloud)中查看基于Adobe Commerce版本的云模板。

下表介绍了云项目中包含的文件：

| 文件 | 描述 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | 将`www`重定向到Apex域和`php`应用程序以提供HTTP的配置文件。 请参阅[配置路由](../routes/routes-yaml.md)。 |
| `/.magento/services.yaml` | 定义MySQL实例(MariaDB)、Redis和OpenSearch或Elasticsearch的配置文件。 请参阅[配置服务](../services/services-yaml.md)。 |
| `/app` | `code`文件夹用于自定义模块。 `design`文件夹用于[自定义主题](../store/custom-theme.md)。 `etc`文件夹包含应用程序的配置文件。 |
| `/m2-hotfixes` | 用于自定义修补程序。 |
| `/update` | 支持模块使用的服务文件夹。 |
| `.gitignore` | 指定要忽略的文件和目录。 请参阅[`.gitignore`引用](#ignoring-files)。 |
| `.magento.app.yaml` | 定义用于构建应用程序的属性的配置文件。 请参阅[配置应用程序](../application/configure-app-yaml.md)。 |
| `.magento.env.yaml` | 生成、部署和部署后阶段的配置文件。 `ece-tools`包包含此文件的示例。 请参阅[配置环境](../environment/configure-env-yaml.md)。 |
| `composer.json` | 获取Adobe Commerce和配置脚本以准备您的应用程序。 请参阅[必需包](../development/overview.md#required-packages)。 |
| `composer.lock` | 存储每个包的版本依赖关系。 请参阅[必需包](../development/overview.md#required-packages)。 |
| `magento-vars.php` | 用于定义[多个商店](../store/multiple-sites.md)和使用变量的网站。 |

{style="table-layout:auto"}

>[!NOTE]
>
>将本地更改推送到远程服务器时，部署脚本将使用`.magento`目录中的配置文件定义的值，然后脚本将删除该目录及其内容。 您的本地开发环境不受影响。

## 应用程序根目录

应用程序根目录的位置取决于环境。

- **入门和专业集成**： `/app`
- **入门级产品**： `/<project-ID>`
- **专业暂存**： `/<project-ID>_stg`
- **专业生产**： `/<project-ID>`

### 可写目录

远程集成、暂存和生产环境是只读的。 出于安全原因，以下目录是&#x200B;*only*&#x200B;可写目录：

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>在生产环境和暂存环境中，三节点群集中的每个节点都有一个`/tmp`目录，该目录不与其他节点共享。

## 忽略文件

云基础架构项目存储库上存在具有Adobe Commerce的基本`.gitignore`文件。 在magento-cloud存储库](https://github.com/magento/magento-cloud/blob/master/.gitignore)中查看最新的[.gitignore文件。 要添加位于`.gitignore`列表中的文件，您可以在暂存提交时使用`-f` （强制）选项：

```bash
git add <path/filename> -f
```

## 更改基本模板

您可以使用以下步骤更改现有项目的结构，以反映云基础架构上Adobe Commerce的最新基础模板。

1. 将项目克隆到本地工作站。

1. 使用`extra`部分的以下值更新`composer.json`文件。

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 添加为基本模板设计的`.gitignore`文件。 例如，如果您需要版本2.2.6模板的`.gitignore`文件，请使用2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore)的[.gitignore文件作为引用。

1. 清除Git缓存。

   ```bash
   git rm -r --cached .
   ```

1. 添加和提交更改。

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
