---
title: 设置OpenSearch服务
description: 了解如何在云基础架构上为Adobe Commerce启用OpenSearch服务。
feature: Cloud, Search, Services
exl-id: e704ab2a-2f6b-480b-9b36-1e97c406e873
source-git-commit: a6f927d7931d0fbbf37269cb41b4b6006ac04268
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# 设置OpenSearch服务

在Elasticsearch的许可更改之后，[OpenSearch](https://www.opensearch.org)服务是Elasticsearch 7.10.2的开源分支。 在GitHub中查看[开源项目](https://github.com/opensearch-project)。

{{elasticsearch-support}}

OpenSearch允许您从任何来源、任何格式获取数据，并实时搜索和可视化数据。

- 产品目录中的产品的快速和高级搜索
- OpenSearch分析器支持多种语言
- 支持停用词和同义词
- 在重新索引操作完成之前，索引不会影响客户

{{service-instruction}}

>[!TIP]
>
>除非您使用[实时搜索](https://experienceleague.adobe.com/en/docs/commerce/live-search/overview)，否则Adobe建议您始终为云基础架构项目上的Adobe Commerce设置OpenSearch，即使您计划为Adobe Commerce应用程序配置第三方搜索工具也是如此。 如果第三方搜索工具失败，则设置OpenSearch将提供回退选项。

**启用OpenSearch**：

1. 对于集成环境，请将`opensearch`服务添加到具有相应版本的`.magento/services.yaml`文件中，并分配磁盘空间（以MB为单位）。 在这种情况下，版本2是合适的。 次要版本不是必需的。

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   对于Pro项目，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)才能在暂存环境和生产环境中更改OpenSearch版本。

1. 设置或验证`relationships`文件中的`.magento.app.yaml`属性。

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   有关这些更改如何影响您的环境的信息，请参阅[配置服务](services-yaml.md)。

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

## OpenSearch软件兼容性

在云基础架构项目上安装或升级Adobe Commerce时，请始终检查OpenSearch服务版本与Adobe Commerce的[OpenSearch PHP](https://github.com/opensearch-project/opensearch-php)客户端之间的兼容性。

- **首次设置** — 确认`services.yaml`文件中指定的OpenSearch版本与为Adobe Commerce配置的OpenSearch PHP客户端兼容。

- **项目升级** — 验证新应用程序版本中的OpenSearch PHP客户端是否与云基础架构上安装的OpenSearch服务版本兼容。

服务版本和兼容性支持取决于在云基础架构上测试和部署的版本，并且有时不同于Adobe Commerce内部部署支持的版本。 有关支持的版本列表，请参阅[安装指南](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)中的&#x200B;_系统要求_。

**验证OpenSearch软件兼容性**：

1. 在本地工作站上，转到您的项目目录。

1. 显示活动环境的OpenSearch详细信息。

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. 或者，您可以使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 检索OpenSearch服务连接详细信息。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   在响应中，查找OpenSearch服务端点的IP地址和端口：

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. 从服务终结点检索已安装的OpenSearch服务`version:number`。

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## 重新启动OpenSearch服务

如果您需要重新启动OpenSearch服务，则必须联系Adobe Commerce支持部门。

## 其他搜索配置

- 默认情况下，每次部署时都会重新生成云环境的搜索配置。 您可以使用`SEARCH_CONFIGURATION`部署变量来保留部署之间的自定义搜索设置。 请参阅[部署变量](../environment/variables-deploy.md#search_configuration)。

- 为您的项目设置OpenSearch服务后，使用管理员UI测试OpenSearch连接并自定义Adobe Commerce的OpenSearch设置。

### 添加OpenSearch插件

或者，您可以通过将`configuration:plugins`部分添加到`.magento/services.yaml`文件中的OpenSearch服务来为OpenSearch添加插件。 例如，以下代码启用ICU分析和拼音分析插件。

>[!NOTE]
>
>这仅适用于集成和入门环境。 要在Pro暂存或生产群集中安装插件，请[提交支持请求](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)。


```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

有关插件的详细信息，请参阅[OpenSearch项目](https://github.com/opensearch-project)。

### 删除OpenSearch插件

从`opensearch:`文件的`.magento/services.yaml`部分中删除插件条目&#x200B;**不会**&#x200B;卸载或禁用该服务。 要完全禁用该服务，必须从`.magento/services.yaml`文件中删除插件后重新索引OpenSearch数据。 此设计可防止依赖这些插件的数据可能丢失或损坏。


**要删除OpenSearch插件**：

>[!NOTE]
>
>此更改仅适用于集成和入门环境。 您必须[提交支持票证](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)才能删除Pro暂存或生产群集中的插件。

1. 从`.magento/services.yaml`文件中删除OpenSearch插件条目。
1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 将`.magento/services.yaml`更改提交到云存储库。
1. 重新索引目录搜索索引（所有环境：集成、入门、Pro Staging和生产群集）。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 清理缓存。

   ```bash
   bin/magento cache:clean
   ```
