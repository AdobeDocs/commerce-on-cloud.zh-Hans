---
title: 设置Elasticsearch服务
description: 了解如何在云基础架构上为Adobe Commerce启用Elasticsearch服务。
feature: Cloud, Search, Services
exl-id: 238b9ed5-ce73-428f-9459-35de8573d5d8
source-git-commit: ef22e7b305c20148f4ee4b2c0e64e2114bf229b5
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# 设置Elasticsearch服务

[Elasticsearch](https://www.elastic.co)是一种开源产品，允许您从任何来源获取任何格式的数据，并实时搜索和可视化这些数据。

{{elasticsearch-support}}

对于Adobe Commerce版本2.4.4及更高版本，请参阅[设置OpenSearch服务](opensearch.md)。

- Elasticsearch对产品目录中的产品执行快速和高级搜索
- Elasticsearch Analyzer支持多种语言
- 支持停用词和同义词
- 在重新索引操作完成之前，索引不会影响客户

>[!TIP]
>
>Adobe建议您在云基础架构项目中始终为Adobe Commerce设置Elasticsearch，即使您计划为Adobe Commerce应用程序配置第三方搜索工具也是如此。 设置Elasticsearch时，可提供在第三方搜索工具失败时的回退选项。

{{service-instruction}}

**启用Elasticsearch**：

1. 对于入门项目，使用Elasticsearch版本将`elasticsearch`服务添加到`.magento/services.yaml`文件，并分配磁盘空间（以MB为单位）。

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   对于Pro项目，您必须提交Adobe Commerce支持工单以在暂存和生产环境中更改Elasticsearch版本。

1. 在`relationships`文件中设置`.magento.app.yaml`属性。

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   有关这些更改如何影响您的环境的信息，请参阅[服务](services-yaml.md)。

1. 部署过程完成后，使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 重新索引目录搜索索引。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 清理缓存。

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Elasticsearch软件兼容性

在云基础架构项目上安装或升级Adobe Commerce时，请始终检查Elasticsearch服务版本与Adobe Commerce的[Elasticsearch PHP](https://github.com/elastic/elasticsearch-php)客户端之间的兼容性。

- **首次设置** — 确认`services.yaml`文件中指定的Elasticsearch版本与为Adobe Commerce配置的Elasticsearch PHP客户端兼容。

- **项目升级** — 验证新应用程序版本中的Elasticsearch PHP客户端是否与云基础架构上安装的Elasticsearch服务版本兼容。

云基础架构上Adobe Commerce的服务版本和兼容性支持取决于云基础架构上部署的版本，并且有时与Adobe Commerce内部部署支持的版本不同。 请参阅[服务版本](services-yaml.md#service-versions)。

**检查Elasticsearch软件兼容性**：

1. 在本地工作站上，转到您的项目目录。

1. 显示活动环境的Elasticsearch详细信息。

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. 或者，您可以使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 检查`elasticsearch/elasticsearch`的编辑器包版本。

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   在响应中，检查`versions`属性中安装的版本。

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   此外，您还可以在环境根目录的`composer.lock`文件中找到Elasticsearch PHP客户端版本。

1. 从命令行中，检索Elasticsearch服务连接详细信息。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   在响应中，查找Elasticsearch服务端点的IP地址：

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. 从服务终结点检索已安装的Elasticsearch服务`version:number`。

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. 检查Elasticsearch服务和PHP客户端之间的版本兼容性。

   如果版本不兼容，请对环境配置进行以下更新之一：

   - 将Elasticsearch PHP客户端更改为与Elasticsearch服务版本兼容的版本。

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - 将`services.yaml`文件中的Elasticsearch服务版本更改为与Elasticsearch PHP客户端兼容的版本。

     {{pro-update-service}}

## 重新启动Elasticsearch服务

如果需要重新启动[Elasticsearch](https://www.elastic.co)服务，必须与Adobe Commerce支持部门联系。

## 其他搜索配置

- 默认情况下，每次部署时都会重新生成云环境的搜索配置。 您可以使用`SEARCH_CONFIGURATION`部署变量来保留部署之间的自定义搜索设置。 请参阅[部署变量](../environment/variables-deploy.md#search_configuration)。

- 为项目设置Elasticsearch服务后，使用管理员UI测试Elasticsearch连接并自定义Adobe Commerce的Elasticsearch设置。

### 添加Elasticsearch插件

或者，您可以通过将`configuration:plugins`部分添加到`.magento/services.yaml`文件中的Elasticsearch服务来为Elasticsearch添加插件。 例如，以下代码启用ICU分析和拼音分析插件。

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

如果您使用Elastic Suite第三方插件，则必须[将`ece-tools`包](../dev-tools/update-package.md)更新为版本2002.0.19或更高版本。
设置Elastic Suite时，将配置设置添加到`ELASTICSUITE_CONFIGURATION`部署变量。 此配置跨部署保存设置。

### 删除Elasticsearch的插件

从`elasticsearch:`中的`.magento/services.yaml`中删除插件项不会像您预期的那样卸载或禁用它们。 您必须重新索引Elasticsearch数据。 此行为旨在防止依赖这些插件的数据可能丢失或损坏。

**要删除Elasticsearch插件**：

1. 从`.magento/services.yaml`文件中删除Elasticsearch插件条目。
1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 将`.magento/services.yaml`更改提交到云存储库。
1. 重新索引目录搜索索引。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 清理缓存。

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>有关将Elastic Suite插件与Adobe Commerce结合使用或对其疑难解答的详细信息，请参阅[Elastic Suite文档](https://github.com/Smile-SA/elasticsuite)。

