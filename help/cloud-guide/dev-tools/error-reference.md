---
title: ECE工具包的错误消息
description: 查看在Adobe Commerce上云基础架构的构建、部署和部署后过程中可能发生的错误代码和消息的列表。
recommendations: noDisplay
role: Developer
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '2763'
ht-degree: 4%

---

# ECE工具的错误消息

此错误消息参考提供了在Adobe Commerce上云基础架构的构建、部署和部署后过程中可能发生的错误疑难解答信息。

部署期间发生的所有严重错误消息和警告错误消息都会写入`var/log/cloud.log`和`/var/log/cloud.error.log`文件。 云错误日志文件仅包含来自最新部署的错误。 空文件表示部署成功，并且没有错误。

在`cloud.error.log`文件中，每个条目均采用JSON字符串的格式，以便于分析：

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

错误消息按部署阶段之一进行分类：生成、部署和部署后。 每个部分都提供一个关联错误的列表，其中包含每个错误的以下信息：

- **错误代码**：错误消息的Adobe Commerce分配的标识符
- **阶段**：指示在生成、部署或部署后阶段期间是否出现错误
- **步骤**：指示部署方案中可能返回错误的步骤。 如果&#x200B;_Step_&#x200B;列为空，则该错误是可通过多个步骤返回或在预处理操作期间返回的常规错误。 有关生成、部署和部署后步骤的详细信息，请参阅[基于方案的部署](../deploy/scenario-based.md)。
- **建议**：提供排除故障和解决错误的指导
- **标题（错误描述）**：总结错误原因的描述
- **类型**：指示错误是严重错误还是警告

<!-- Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository. -->

## 严重错误

严重错误表示云基础架构项目配置中的Commerce存在导致部署失败的问题，例如错误、不支持或缺少所需设置的配置。 在部署之前，必须更新配置以解决这些错误。

### 构建阶段

| 错误代码 | 构建步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 2 |  | 无法写入`./app/etc/env.php`文件 | 部署脚本无法对`/app/etc/env.php`文件进行所需的更改。 检查您的文件系统权限。 |
| 3 |  | 未在`schema.yaml`文件中定义配置 | 未在`./vendor/magento/ece-tools/config/schema.yaml`文件中定义配置。 检查配置变量名称是否正确且已定义。 |
| 4 |  | 未能分析`.magento.env.yaml`文件 | `./.magento.env.yaml`文件格式无效。 使用YAML解析器检查语法并修复任何错误。 |
| 5 |  | 无法读取`.magento.env.yaml`文件 | 无法读取`./.magento.env.yaml`文件。 检查文件权限。 |
| 6 |  | 无法读取`.schema.yaml`文件 | 无法读取`./vendor/magento/ece-tools/config/magento.env.yaml`文件。 检查文件权限并重新部署(`magento-cloud environment:redeploy`)。 |
| 7 | 刷新模块 | 无法写入`./app/etc/config.php`文件 | 部署脚本无法对`/app/etc/config.php`文件进行所需的更改。 检查您的文件系统权限。 |
| 8 | validate-config | 无法读取`composer.json`文件 | 无法读取`./composer.json`文件。 检查文件权限。 |
| 9 | validate-config | `composer.json`文件缺少所需的自动加载部分 | `composer.json`文件中缺少必需的`autoload`部分。 比较自动加载部分与云模板中的`composer.json`文件，并添加缺少的配置。 |
| 10 | validate-config | `.magento.env.yaml`文件包含未在架构中声明的选项，或者配置有无效值或阶段的选项 | `./.magento.env.yaml`文件包含无效的配置。 有关详细信息，请查看错误日志。 |
| 11 | 刷新模块 | 命令失败： `/bin/magento module:enable --all` | 尝试在本地运行`composer update`。 然后，提交并推送更新的`composer.lock`文件。 另外，查看`cloud.log`以了解更多信息。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 12 | apply-patch | 应用修补程序失败 |  |
| 13 | set-report-dir-nesting-level | 无法写入文件`/pub/errors/local.xml` |  |
| 14 | copy-sample-data | 无法复制示例数据文件 |  |
| 15 | compile-di | 命令失败： `/bin/magento setup:di:compile` | 有关详细信息，请查看`cloud.log`。 将`VERBOSE_COMMANDS: '-vvv'`添加到`.magento.env.yaml`中，以获取更详细的命令输出。 |
| 16 | dump-autoload | 命令失败： `composer dump-autoload` | `composer dump-autoload`命令失败。 有关详细信息，请查看`cloud.log`。 |
| 17 | 跑步机 | 为Javascript捆绑运行`Baler`的命令失败 | 检查`SCD_USE_BALER`环境变量，验证是否已配置并启用JS捆绑包的Baler模块。 如果您不需要Baler模块，请设置`SCD_USE_BALER: false`。 |
| 18 | compress-static-content | 未找到所需的实用程序（超时，破折号） |  |
| 19 | deploy-static-content | 命令`/bin/magento setup:static-content:deploy`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 20 | compress-static-content | 静态内容压缩失败 | 有关详细信息，请查看`cloud.log`。 |
| 21 | 备份数据：静态内容 | 无法将静态内容复制到`init`目录中 | 有关详细信息，请查看`cloud.log`。 |
| 22 | backup-data： writable-dirs | 无法将某些可写目录复制到`init`目录 | 无法将可写目录复制到`./init`文件夹中。 检查您的文件系统权限。 |
| 23 |  | 无法创建记录器对象 |  |
| 24 | 备份数据：静态内容 | 未能清除`./init/pub/static/`目录 | 未能清除`./init/pub/static`文件夹。 检查您的文件系统权限。 |
| 25 |  | 找不到编辑器包 | 如果您直接从GitHub存储库安装了Adobe Commerce应用程序版本，请验证是否配置了`DEPLOYED_MAGENTO_VERSION_FROM_GIT`环境变量。 |
| 26 | validate-config | 删除Adobe Commerce和Magento Open Source2.4及更高版本中不再支持的MagentoBraintree模块配置。 | Magento2.4.0及更高版本不再支持Braintree模块。 从`.magento.app.yaml`文件的变量部分删除CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL变量。 对于Braintree支付支持，请改用Commerce Marketplace中的正式扩展。 |

### 部署阶段

| 错误代码 | 部署步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 101 | 预部署：缓存 | 缓存配置不正确（缺少端口或主机） | 缓存配置缺少必需的参数`server`或`port`。 有关详细信息，请查看`cloud.log`。 |
| 102 |  | 无法写入`./app/etc/env.php`文件 | 部署脚本无法对`/app/etc/env.php`文件进行所需的更改。 检查您的文件系统权限。 |
| 103 |  | 未在`schema.yaml`文件中定义配置 | 未在`./vendor/magento/ece-tools/config/schema.yaml`文件中定义配置。 检查配置变量名称是否正确，以及它是否已定义。 |
| 104 |  | 未能分析`.magento.env.yaml`文件 | 未在`./vendor/magento/ece-tools/config/schema.yaml`文件中定义配置。 检查配置变量名称是否正确，以及它是否已定义。 |
| 105 |  | 无法读取`.magento.env.yaml`文件 | 无法读取`./.magento.env.yaml`文件。 检查文件权限。 |
| 106 |  | 无法读取`.schema.yaml`文件 |  |
| 107 | pre-deploy： clean-redis-cache | 未能清除Redis缓存 | 未能清除Redis缓存。 检查Redis缓存配置是否正确，以及Redis服务是否可用。 查看[安装Redis服务](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/redis.html)。 |
| 108 | 预部署：set-production-mode | 命令`/bin/magento maintenance:enable`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 109 | validate-config | 数据库配置不正确 | 检查是否正确配置了`DATABASE_CONFIGURATION`环境变量。 |
| 110 | validate-config | 会话配置不正确 | 检查是否正确配置了`SESSION_CONFIGURATION`环境变量。 配置必须至少包含`save`参数。 |
| 111 | validate-config | 搜索配置不正确 | 检查是否正确配置了`SEARCH_CONFIGURATION`环境变量。 配置必须至少包含`engine`参数。 |
| 112 | validate-config | 资源配置不正确 | 检查是否正确配置了`RESOURCE_CONFIGURATION`环境变量。 配置必须包含至少`connection`个参数。 |
| 113 | validate-config：elasticsuite-integrity | ElasticSuite已安装，但Elasticsearch服务不可用 | 检查`SEARCH_CONFIGURATION`环境变量是否配置正确，并验证Elasticsearch服务是否可用。 |
| 114 | validate-config：elasticsuite-integrity | ElasticSuite已安装，但使用了其他搜索引擎 | ElasticSuite已安装，但已配置另一个搜索引擎。 更新`SEARCH_CONFIGURATION`环境变量以启用Elasticsearch，并验证`services.yaml`文件中的Elasticsearch服务配置。 |
| 115 |  | 数据库查询执行失败 |  |
| 116 | install-update： setup | 命令`/bin/magento setup:install`失败 | 有关详细信息，请查看`cloud.log`和`install_upgrade.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 117 | install-update： config-import | 命令`app:config:import`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 118 |  | 未找到所需的实用程序（超时，破折号） |  |
| 119 | install-update： deploy-static-content | 命令`/bin/magento setup:static-content:deploy`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 120 | compress-static-content | 静态内容压缩失败 | 有关详细信息，请查看`cloud.log`。 |
| 121 | deploy-static-content：generate | 无法更新已部署的版本 | 无法更新`./pub/static/deployed_version.txt`文件。 检查您的文件系统权限。 |
| 122 | clean-static-content | 未能清除静态内容文件 |  |
| 123 | install-update： split-db | 命令`/bin/magento setup:db-schema:split`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 124 | clean-view-preprocessed | 未能清除`var/view_preprocessed`文件夹 | 无法清除`./var/view_preprocessed`文件夹。 检查您的文件系统权限。 |
| 125 | install-update： reset-password | 未能更新`/var/credentials_email.txt`文件 | 未能更新`/var/credentials_email.txt`文件。 检查您的文件系统权限。 |
| 126 | install-update： update | 命令`/bin/magento setup:upgrade`失败 | 有关详细信息，请查看`cloud.log`和`install_upgrade.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 127 | clean-cache | 命令`/bin/magento cache:flush`失败 | 有关详细信息，请查看`cloud.log`。 有关更详细的命令输出，请将`VERBOSE_COMMANDS: '-vvv'`选项添加到`.magento.env.yaml`文件中。 |
| 128 | disable-maintenance-mode | 命令`/bin/magento maintenance:disable`失败 | 有关详细信息，请查看`cloud.log`。 将`VERBOSE_COMMANDS: '-vvv'`添加到`.magento.env.yaml`中，以获取更详细的命令输出。 |
| 129 | install-update： reset-password | 无法读取重置密码模板 |  |
| 130 | install-update： cache_type | 命令失败： `php ./bin/magento cache:enable` | 命令`php ./bin/magento cache:enable`仅在安装了Adobe Commerce但部署开始时`./app/etc/env.php`文件不存在或为空时运行。 有关详细信息，请查看`cloud.log`。 将`VERBOSE_COMMANDS: '-vvv'`添加到`.magento.env.yaml`中，以获取更详细的命令输出。 |
| 131 | install-update | `crypt/key`键值在`./app/etc/env.php`文件或`CRYPT_KEY`云环境变量中不存在 | 如果Adobe Commerce部署开始时`./app/etc/env.php`文件不存在，或者未定义`crypt/key`值，则会出现此错误。 如果您从其他环境迁移了数据库，请从该环境中检索加密密钥值。 然后，将该值添加到当前环境中的[CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy.html#crypt_key)云环境变量。 请参阅[Adobe Commerce加密密钥](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/develop/overview.html#gather-credentials)。 如果意外删除了`./app/etc/env.php`文件，请使用以下命令从从先前部署创建的备份文件中恢复该文件： `./vendor/bin/ece-tools backup:restore` CLI命令。” |
| 132 |  | 无法连接到Elasticsearch服务 | 检查有效的Elasticsearch凭据并验证服务是否正在运行 |
| 137 |  | 无法连接到OpenSearch服务 | 检查有效的OpenSearch凭据并验证服务是否正在运行 |
| 133 | validate-config | 删除Adobe Commerce或Magento Open Source2.4及更高版本不再支持的MagentoBraintree模块配置。 | Adobe Commerce或Magento Open Source2.4.0及更高版本不再支持Braintree模块。 从`.magento.app.yaml`文件的变量部分删除CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL变量。 要获得Braintree支持，请改用Commerce Marketplace中的官方Braintree支付扩展。 |
| 134 | validate-config | Adobe Commerce和Magento Open Source2.4.0需要安装Elasticsearch服务 | 安装Elasticsearch服务 |
| 138 | validate-config | Adobe Commerce和Magento Open Source2.4.4要求安装OpenSearch或Elasticsearch服务 | 安装OpenSearch服务 |
| 135 | validate-config | 对于Adobe Commerce，搜索引擎必须设置为Elasticsearch，并且Magento Open Source>= 2.4.0 | 检查`engine`选项的SEARCH_CONFIGURATION变量。 如果已配置，请删除选项，或将值设置为“elasticsearch”。 |
| 136 | validate-config | 从Adobe Commerce和Magento Open Source2.5.0开始删除了拆分数据库。 | 如果使用拆分数据库，则必须还原或迁移到单个数据库，或者使用替代方法。 |
| 139 | validate-config | 搜索引擎不正确 | 此Adobe Commerce或Magento Open Source版本不支持OpenSearch。 您必须使用版本2.3.7-p3、2.4.3-p2或更高版本 |

### 部署后阶段

| 错误代码 | 部署后步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 201 | is-deploy-failed | 部署阶段失败 |  |
| 202 |  | `./app/etc/env.php`文件不可写 | 部署脚本无法对`/app/etc/env.php`文件进行所需的更改。 检查您的文件系统权限。 |
| 203 |  | 未在`schema.yaml`文件中定义配置 | 未在`./vendor/magento/ece-tools/config/schema.yaml`文件中定义配置。 检查配置变量名称是否正确，以及它是否已定义。 |
| 204 |  | 未能分析`.magento.env.yaml`文件 | `./.magento.env.yaml`文件格式无效。 使用YAML解析器检查语法并修复任何错误。 |
| 205 |  | 无法读取`.magento.env.yaml`文件 | 检查文件权限。 |
| 206 |  | 无法读取`.schema.yaml`文件 |  |
| 207 | 热身 | 未能预加载某些预热页面 |  |
| 208 | 第一字节时间 | 无法测试到第一个字节的时间(TTFB) |  |
| 227 | clean-cache | 命令`/bin/magento cache:flush`失败 | 有关详细信息，请查看`cloud.log`。 将`VERBOSE_COMMANDS: '-vvv'`添加到`.magento.env.yaml`中，以获取更详细的命令输出。 |

### 常规

| 错误代码 | 常规步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 243 |  | 未在`schema.yaml`文件中定义配置 | 检查配置变量名称是否正确，以及它是否定义了配置变量。 |
| 244 |  | 未能分析`.magento.env.yaml`文件 | `./.magento.env.yaml`文件格式无效。 使用YAML解析器检查语法并修复任何错误。 |
| 245 |  | 无法读取`.magento.env.yaml`文件 | 无法读取`./.magento.env.yaml`文件。 检查文件权限。 |
| 246 |  | 无法读取`.schema.yaml`文件 |  |
| 247 |  | 无法生成用于事件的模块 | 有关详细信息，请查看`cloud.log`。 |
| 248 |  | 无法启用用于事件的模块 | 有关详细信息，请查看`cloud.log`。 |
| 249 |  | 无法生成AdobeCommerceWebhookPlugins模块 | 有关详细信息，请查看`cloud.log`。 |
| 250 |  | 无法启用AdobeCommerceWebhookPlugins模块 | 有关详细信息，请查看`cloud.log`。 |

## 警告错误

警告错误表示云基础架构项目配置中的Commerce存在问题，例如不正确、已弃用、不支持或缺少可能影响站点操作的可选功能的配置设置。 尽管警告不会导致部署失败，但您应该查看警告消息并更新配置以解决它们。

### 构建阶段

| 错误代码 | 构建步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 1001 | validate-config | 文件app/etc/config.php不存在 |  |
| 1002 | validate-config | 的。不再支持/build_options.ini文件 |  |
| 1003 | validate-config | 共享配置文件中缺少模块部分 |  |
| 1004 | validate-config | 该配置与此版本的Magento不兼容 |  |
| 1005 | validate-config | 已忽略SCD选项 |  |
| 1006 | validate-config | 配置的状态不理想 |  |
| 1007 | 跑步机 | 无法使用Baler JS捆绑包 |  |

### 部署阶段

| 错误代码 | 部署步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 2001 | 预部署：缓存 | 为不可用的Redis服务配置了缓存。 将忽略配置。 |  |
| 2002 | validate-config | 配置的状态不理想 |  |
| 2003 | validate-config | 尚未配置错误报告的目录嵌套级别值 |  |
| 2004 | validate-config | 中的配置无效。/pub/errors/local.xml文件。 |  |
| 2005 | validate-config | 管理员数据仅在初始安装期间用于创建管理员用户。 在升级过程中，会忽略对管理员数据所做的任何更改。 | 初始安装后，您可以从配置中删除管理员数据。 |
| 2006 | validate-config | 未创建管理员用户，因为未设置管理员电子邮件 | 安装后，您可以手动创建管理员用户：使用ssh连接到您的环境。 然后，运行`bin/magento admin:user:create`命令。 |
| 2007 | validate-config | 将php版本更新为建议的版本 |  |
| 2008 | validate-config | Adobe Commerce和Magento Open Source2.1已弃用Solr支持。 |  |
| 2009 | validate-config | Adobe Commerce和Magento Open Source 2.2或更高版本不再支持Solr。 |  |
| 2010 | validate-config | Elasticsearch服务安装在基础设施层，但不用作搜索引擎。 | 请考虑从基础架构层删除Elasticsearch服务以优化资源使用。 |
| 2011 | validate-config | 基础结构层上的Elasticsearch服务版本与Adobe Commerce应用程序使用的elasticsearch/elasticsearch module的当前版本不兼容。 |  |
| 2012 | validate-config | 当前配置与此版本的Adobe Commerce不兼容 |  |
| 2013 | validate-config | 已忽略SCD选项，因为部署过程未在构建阶段运行 |  |
| 2014 | validate-config | 该配置包含已弃用的变量或值 |  |
| 2015 | validate-config | 环境配置无效 |  |
| 2016 | validate-config | 无法解码JSON类型配置 |  |
| 2017 | validate-config | 当前配置与此版本的Adobe Commerce不兼容 |  |
| 2018 | validate-config | 某些服务已通过EOL |  |
| 2019 | validate-config | MySQL搜索配置选项已弃用 | 请改用Elasticsearch。 |
| 2029 | validate-config | 拆分数据库在Adobe Commerce和Magento Open Source2.4.2中已弃用，将在2.5中删除。 | 如果使用拆分数据库，则应开始计划恢复或迁移到单个数据库，或者使用替代方法。 |
| 2020 | install-update | Adobe Commerce安装已完成，但`app/etc/env.php`配置文件缺失或为空。 | 所需的数据将从环境配置和.magento.env.yaml文件中恢复。 |
| 2021 | install-update：db-connection | 对于使用自定义连接的拆分数据库 |  |
| 2022 | install-update：db-connection | 您已经更改为与从属连接不兼容的数据库配置。 |  |
| 2023 | install-update：split-db | 将跳过启用拆分数据库。 |  |
| 2024 | install-update：split-db | SPLIT_DB变量缺少拆分连接类型的配置。 |  |
| 2025 | install-update：split-db | 未设置从属连接。 |  |
| 2026 | pre-deploy：restore-writable-dirs | 未能将构建阶段生成的一些数据还原到已装入的目录 | 有关详细信息，请查看`cloud.log`。 |
| 2027 | validate-config：mage-mode-variable | 不支持MAGE_MODE环境变量的模式值 | 移除MAGE_MODE环境变量，或将其值更改为“production”。 云基础架构上的Adobe Commerce仅支持“生产”模式。 |
| 2028 | 远程存储 | 无法启用远程存储。 | 验证远程存储凭据。 |
| 2030 | validate-config | Elasticsearch和OpenSearch服务都安装在基础架构层。 Adobe Commerce和Magento Open Source2.4.4及更高版本默认使用OpenSearch | 请考虑从基础结构层删除Elasticsearch或OpenSearch服务以优化资源使用。 |

### 部署后阶段

| 错误代码 | 部署后步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 3001 | validate-config | Adobe Commerce中已启用调试日志记录 | 要节省磁盘空间，请勿为生产环境启用调试日志记录。 |
| 3002 | 热身 | 无法获取存储url |  |
| 3003 | 热身 | 无法获取存储URL |  |
| 3004 | 备份 | 无法创建备份文件 |  |

### 常规

| 错误代码 | 常规步骤 | 错误描述（标题） | 建议的操作 |
| - | - | - | - |
| 4001 |  | 无法获取系统处理器计数： |  |
