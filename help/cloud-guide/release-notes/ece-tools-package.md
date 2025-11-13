---
title: ECE-Tools发行说明
description: 请参阅ECE-Tools软件包的最新改进列表。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 16d5577da8841c2f65f9b5298beaa7fb84a1ab47
workflow-type: tm+mt
source-wordcount: '3314'
ht-degree: 0%

---

# ECE-Tools发行说明

[ece-tools](https://github.com/magento/ece-tools)包是一组用于管理和部署云项目的脚本和工具。 这些发行说明介绍了此包的最新改进，此包是[适用于Commerce的Cloud Tools Suite](cloud-tools-suite.md)的一部分。

>[!NOTE]
>
>有关更新到[包的最新版本的信息，请参阅](../dev-tools/update-package.md)升级ECE工具`ece-tools`。

`ece-tools`包使用以下版本控制序列： `200<major>.<minor>.<patch>`

发行说明包括：

- ![新图标](../../assets/new.svg)新功能
- ![修复图标](../../assets/fix.svg)修复和改进

<!--Add release notes below-->

## v2002.2.9 {#latest}

发行日期： 2025年11月13日

- ![修复图标](../../assets/fix.svg) **Symfony包** — 已添加对最新Symfony YAML包的支持。<!-- MCLOUD-14020 -->
- ![修复图标](../../assets/fix.svg) **修复了活动服务的缓存清理** — 添加了活动服务验证。<!-- MCLOUD-14166 -->

## v2002.2.8

发行日期： 2025年10月8日

- ![新图标](../../assets/new.svg) **ActiveMQ** — 已添加对ActiveMQ的支持。<!-- MCLOUD-13770 -->
- ![新图标](../../assets/new.svg) **ActiveMQ**&#x200B;已添加功能测试。<!-- MCLOUD-13813 -->


## v2002.2.7

发行日期： 2025年8月7日

- ![修复图标](../../assets/fix.svg) **PHP 8.4修复** — 添加了类型兼容性。<!-- MCLOUD-13965 -->
- ![修复图标](../../assets/fix.svg) **EOL验证器** — 已更新生命周期结束(EOL)服务日期。<!-- MCLOUD-13929 -->
- ![新图标](../../assets/new.svg) **Valkey** — 添加了PHP 8.2和PHP 8.3功能测试。<!-- MCLOUD-13610 -->
- ![修复图标](../../assets/fix.svg) **Valkey验证器** — 修复了ECE工具警告消息。<!-- MCLOUD-13896 -->
- ![修复图标](../../assets/fix.svg) **ECE工具**&#x200B;已添加单元测试改进。<!-- MCLOUD-13838 -->
- ![新图标](../../assets/new.svg) **服务验证器** — 添加了Opensearch、MariaDB和PHP的新版本支持。<!-- MCLOUD-13923 -->
- ![新图标](../../assets/new.svg) **Opensearch3** — 已添加对Opensearch3的支持。<!-- MCLOUD-13763 -->
- ![修复图标](../../assets/fix.svg) **对2.4.4-p7/p12**&#x200B;的Opensearch支持 — 已更新验证器脚本。<!-- MCLOUD-13945 -->
- ![新图标](../../assets/new.svg) **Opensearch3测试** — 已添加功能测试。<!-- MCLOUD-13769 -->

## v2002.2.6

发布日期： 2025年6月3日

- ![修复图标](../../assets/fix.svg) **改进与2.4.8的兼容性** — 更新了第三方库以更好地与2.4.8<!-- MCLOUD-13707 -->兼容

## v2002.2.5

发行日期： 2025年5月27日

- ![新图标](../../assets/new.svg) **Extended Valkey兼容性**-Adobe Commerce中的Extended Valkey兼容性。<!-- MCLOUD-13595 -->
- ![修复图标](../../assets/fix.svg) **已更新RabbitMQ验证器** — 已更新RabbitMQ的验证器。<!-- MCLOUD-13589 -->
- ![修复图标](../../assets/fix.svg) **已更新MariaDB验证器** — 已更新MariaDB 10.11的ece-tools验证器。<!-- MCLOUD-13593 -->
- ![修复图标](../../assets/fix.svg) **扩展Opensearch2兼容性** — 使Opensearch2与最新的2.4.4版本兼容。<!-- MCLOUD-13710 -->

## v2002.2.4

发行日期： 2025年4月24日

- ![修复图标](../../assets/fix.svg) **适用于2.4.4/2.4.5**&#x200B;的Opensearch2 — 修复了与Adobe Commerce版本2.4.4/2.4.5.`opensearch2`中支持<!-- MCLOUD-13607 -->相关的问题

## v2002.2.3

发行日期： 2025年4月9日

- ![修复图标](../../assets/fix.svg) **修复Valkey**&#x200B;修复了Valkey自定义配置的问题。<!-- MCLOUD-13569 -->
- ![修复图标](../../assets/fix.svg) **修复验证器** — 适用于RabbitMQ 4.0的修复验证器。<!-- MCLOUD-13560 -->

## v2002.2.2

发行日期： 2025年4月7日

## v2002.2.2

发行日期： 2025年4月7日

- ![new icon](../../assets/new.svg) **Valkey** — 添加了对新服务(Valkey)的支持，该服务是Redis的替代服务。<!-- MCLOUD-13455 -->
- ![修复图标](../../assets/fix.svg) **适用于2.4.4/2.4.5**&#x200B;的Opensearch2 — 在Adobe Commerce版本2.4.4/2.4.5.`opensearch2`中添加了对<!-- MCLOUD-13493 -->的支持

## v2002.2.1

发行日期： 2024年2月6日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 添加了对PHP 8.4.<!-- MCLOUD-13145 -->的支持
- ![修复图标](../../assets/fix.svg) **Opensearch的验证器** — 修复了生成有关错误服务版本的误导性消息的验证器。<!-- MCLOUD-13184 -->

## v2002.2.0

发行日期： 2024年10月7日

- ![新图标](../../assets/new.svg) **MariaDB 11.4** — 已添加MariaDB 11.4的支持。
- ![修复图标](../../assets/fix.svg) **重构的代码** — 已删除对旧PHP版本7.4、7.3、7.2和相关库的支持。<!-- MCLOUD-9278 -->
- ![修复图标](../../assets/fix.svg) **升级了单色版本** — 添加了对单色版本3.6的支持。<!-- MCLOUD-12855 -->
- ![修复图标](../../assets/fix.svg) **RabbitMQ、MariaDB和PHP的验证器** — 修复了生成有关错误服务版本的误导性消息的验证器。

## v2002.1.19

发行日期： 2024年5月21日

- ![新图标](../../assets/new.svg) **Lua** — 已为CACHE_CONFIGURATION添加选项useLua。
- ![修复图标](../../assets/fix.svg) **验证器** — 已更新Redis和RabbitMQ新版本的验证器。

## v2002.1.18

发行日期： 2024年4月8日

- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.3的支持。
- ![修复图标](../../assets/fix.svg) **验证器** — 已更新EOL验证器。

## v2002.1.17

发行日期： 2024年1月16日

- ![修复图标](../../assets/fix.svg) **Elasticsearch和OpenSearch的验证器** — 修复了在启用LiveSearch时生成误导性消息以安装搜索服务的验证器。<!-- MCLOUD-10167 -->
- ![修复图标](../../assets/fix.svg) **部署警告** — 修复了导致出现非空文件夹部署警告的问题。<!-- MCLOUD-8958 -->

## v2002.1.16

发行日期： 2023年10月16日

- ![新图标](../../assets/new.svg) **ENABLE_WEBHOOKS全局环境变量** — 添加了[ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks)全局变量，以便与Commerce Webhooks一起使用来连接到外部端点，如App Builder运行时操作或第三方清单管理系统。

## v2002.1.15

发行日期： 2023年7月31日

- ![修复图标](../../assets/fix.svg) **错误代码** — 已更新错误代码架构和错误代码文档生成器。
- ![修复图标](../../assets/fix.svg) **自定义Redis模型的验证器** — 已更新自定义Redis后端模型的验证器。 [查看缓存配置的示例](../environment/variables-deploy.md#cache_configuration)。
- ![修复图标](../../assets/fix.svg) **RabbitMQ的验证器** — 已添加对RabbitMQ 3.11的支持
- ![修复图标](../../assets/fix.svg) **修复了错误的链接** — 修复了欢迎电子邮件模板中指向载入文档的错误链接。

## v2002.1.14

发行日期： 2023年3月10日

- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.2的支持。
- ![新图标](../../assets/new.svg) **服务验证器** — 更新了Commerce 2.4.6所需服务的验证器：MariaDB 10.6、Redis 7.0、PHP 8.2、OpenSearch 2.x和RabbitMQ 3.9。
- ![修复图标](../../assets/fix.svg) **ece-tools db-dump** — 修复了导致`db-dump`操作过早停止的问题。

## v2002.1.13

发行日期： 2022年10月27日

- ![新图标](../../assets/new.svg) **已添加对Adobe Commerce的Adobe I/O Events的支持**。 扩展开发人员现在可以使用[Adobe I/O Events](https://developer.adobe.com/events/docs/)框架将云实例中的Commerce事件信息发送到其为[Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/)编写的应用程序。 适用于Adobe Commerce的Adobe I/O Events在“合作伙伴预览”中。<!-- CEXT-932 -->
- ![新图标](../../assets/new.svg) **OPcache配置的验证器** — 已添加验证器以检查排除路径的OPcache配置。<!-- MCLOUD-9485 -->
- ![修复图标](../../assets/fix.svg) **修复了GraphQL缓存配置的问题** — 现在，ECE-Tools在`id_salt`文件中保留`cache`配置中的GraphQL `app/etc/env.php`值。<!-- MCLOUD-9486 -->

## v2002.1.12

发行日期： 2022年9月13日

- ![新图标](../../assets/new.svg) **启用`synchronous_replication`** — 启用`synchronous_replication=>true`时，ECE-Tools在`app/etc/env.php`文件中设置`MYSQL_USE_SLAVE_CONNECTION`。 此配置仅影响Commerce 2.4.6+。 在`MYSQL_USE_SLAVE_CONNECTION`部署变量[.](../environment/variables-deploy.md#mysql_use_slave_connection)中查看<!-- MCLOUD-9142 -->变量说明
- ![新图标](../../assets/new.svg) **OpenSearch** — 添加了配置和设置下一个Adobe Commerce版本2.4.6的`opensearch`引擎的功能。请参阅[设置OpenSearch服务](../services/opensearch.md)。<!-- MCLOUD-9236 -->

## v2002.1.11

发行日期： 2022年8月4日

- ![修复图标](../../assets/fix.svg) **ElasticSuite Validator和OpenSearch** — 修复了在安装OpenSearch时的ElasticSuite完整性检查验证器问题。<!-- MCLOUD-8767 -->
- ![修复图标](../../assets/fix.svg) **部署命令的返回类型** — 修复了部署命令的返回类型。<!-- AC-3208 -->
- 安装Commerce 2.4.5时出现了![修复图标](../../assets/fix.svg) **[!DNL RabbitMQ]问题** — 修复了安装Commerce 2.4.5时出现的[!DNL RabbitMQ]崩溃问题。<!-- MCLOUD-9059 -->

## v2002.1.10

发行日期： 2022年3月31日

- ![修复图标](../../assets/fix.svg) **Elasticsearch 7.10** — 更新了验证器以支持Elasticsearch 7.10版本。<!-- MCLOUD-8548 -->

## v2002.1.9

发行日期： 2022年3月10日

- ![新图标](../../assets/new.svg) **OpenSearch** — 添加了对Adobe Commerce版本2.4.4、2.4.3-p2和2.3.7-p3.<!-- MCLOUD-8296 -->的OpenSearch的支持
- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.1的支持。
- ![修复图标](../../assets/fix.svg) **symfony/process** — 添加了与symfony/process ^5.3的兼容性。<!-- MCLOUD-8283 -->

- ![新图标](../../assets/new.svg) **使用者多个进程** — 添加了`multiple_processes`选项，以便您可以指定每个使用者要衍生的进程数。 在`CRON_CONSUMERS_RUNNER`部署变量[.](../environment/variables-deploy.md#cron_consumers_runner)中查看<!-- MCLOUD-8295 -->变量说明
- ![新图标](../../assets/new.svg) **OpenSearch方案和完整主机路径** — 添加了配置Elasticsearch方案和完整主机路径的功能。
- ![修复图标](../../assets/fix.svg) **AWS S3** — 更改了AWS S3启用方法。
- ![修复图标](../../assets/fix.svg) **修复driver_options读取器** — 已添加`env.php`从验证器的`ece-tools`文件中读取DB连接的driver_options配置。<!-- MCLOUD-8420 -->

## v2002.1.8

发行日期： 2021年10月25日

- ![新图标](../../assets/new.svg) **备用转储位置** — 添加了`--dump-directory`选项，以便您可以为数据库转储选择目标目录。 现在`/app/var/dump-main`是数据库转储的默认目标目录。 请参阅[备份管理：转储数据库](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![修复图标](../../assets/fix.svg) **更新单一日志** — 已将`monolog`包所需的最低版本更新为`^2.3`。<!-- ACMP-1263 -->
- ![修复图标](../../assets/fix.svg) **更新Symfony** — 已更新Symfony依赖项以便与Adobe Commerce 2.4.4兼容。<!-- ACMP-1533 -->
- ![修复图标](../../assets/fix.svg) **功能/解决自动加载** — 修复了部署到集成环境并看到`CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`错误时的问题。<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

发行日期： 2021年7月29日

**配置更新**—

- ![新图标](../../assets/new.svg)已添加对Composer 2.0.<!--MCLOUD-8003-->的支持

- ![修复图标](../../assets/fix.svg) **更新了`symphony/console`**&#x200B;的编辑器要求 — 更新了`composer.json`包的ECE-Tools `symphony/console`版本要求，以修复导致`di:compile`命令失败并出现以下错误的问题： `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![修复图标](../../assets/fix.svg)更新了软件生命周期结束检查(`eol.yaml`)以包含Elasticsearch 7.9.x。<!--MCLOUD-7938-->

## v2002.1.6

发行日期： 2021年4月20日

- ![新图标](../../assets/new.svg) **Redis身份验证凭据** — 已添加在部署阶段从`relationships`属性读取Redis授权凭据的功能。<!--MCLOUD-7694-->

- ![新图标](../../assets/new.svg) **Elasticsearch授权凭据** — 已添加在部署阶段从`relationships`属性读取Elasticsearch授权凭据的功能。<!--MCLOUD-7695-->

- ![新图标](../../assets/new.svg) **专用会话存储服务** — 已添加`redis-session`作为会话存储的第二个选项。 您可以使用`redis-session`服务来存储会话信息，并使用`redis`服务来缓存以提供更好的性能。<!--MCLOUD-7698-->

- ![新图标](../../assets/new.svg) **已弃用的SPLIT_DB消息** — 为Adobe Commerce 2.4.2的已弃用`SPLIT_DB`选项添加了验证器警告和严重消息，并在Adobe Commerce 2.5.0中删除了该选项。<!--MCLOUD-7806-->

- ![修复图标](../../assets/fix.svg) **来自关系的Elasticsearch版本** — 修复了服务验证器，以便从Cloud Docker和集成环境中的`relationships`属性中检索Elasticsearch的正确版本。<!--MCLOUD-7572-->

- ![修复图标](../../assets/fix.svg) **灵活的Redis端口验证**—Redis现在可以从`server` URL验证自定义缓存连接中的端口。 例如，您可以将端口号添加到服务器URL中，如下所示： `server: 'tcp://rfs-store-simple-page-cache:26379'`。 这有助于防止`port`选项缺失或不正确的验证错误。<!--MCLOUD-7722-->

- ![修复图标](../../assets/fix.svg) **升级到Adobe Commerce 2.4.2** — 修复了在升级到Adobe Commerce 2.4.2后要求用户手动运行`bin/magento setup:upgrade`以使他们的站点正常运行的问题。<!--MCLOUD-7776-->

## v2002.1.5

发行日期： 2021年2月1日

- ![新图标](../../assets/new.svg) **远程存储** — 添加了`REMOTE_STORAGE`环境变量，以启用云项目以使用存储服务(如AWS S3)远程存储媒体文件。 此配置选项是ECE-Tools包的一部分，但在云基础架构上的Adobe Commerce上不受支持。<!--MCLOUD-7153-->

- ![新图标](../../assets/new.svg) **新`cloud:config:validate`命令** — 添加了命令`php vendor/bin/ece-tools cloud:config:validate`，用于在将更改推送到远程云环境之前验证`.magento.env.yaml`配置。<!--MCLOUD-7120-->

- ![新图标](../../assets/new.svg) **刷新opcache** — 添加了`opcache.enable_cli` PHP选项的支持，可在运行部署挂接之前刷新OPcache。 此配置将重置缓存配置，以确保当前配置设置应用于每个部署。<!--MCLOUD-7015-->

- ![新图标](../../assets/new.svg) **验证Aurora DB** — 已更新数据库服务验证，以便它与Aurora数据库兼容。<!--MCLOUD-7269-->

- ![新图标](../../assets/new.svg) **新SCD_NO_PARENT环境变量** — 已添加`SCD_NO_PARENT`环境变量(适用于Adobe Commerce >=2.4.2)以管理父主题的静态内容生成。<!--MCLOUD-7284-->

- ![修复图标](../../assets/fix.svg) **内存限制和命令** — 修复了当`php vendor/bin/ece-tools`文件的大小超过PHP memory_limit时，`cloud.log`命令无法运行的问题。 现在，我们只从日志文件读取较小的数据子集，而不是将整个`cloud.log`文件读入内存。<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![修复图标](../../assets/fix.svg) **自定义数据库连接** — 修复了未使用为`.magento.env.yaml`定义的自定义数据库连接的`DATABASE_CONFIGURATION`配置问题。 未将连接设置添加到`app/etc/env.php`.<!--MCLOUD-7426-->

- ![修复图标](../../assets/fix.svg) **空错误日志** — 修复了`cloud.error.log`为空时导致部署失败的问题。<!--MCLOUD-7296-->

- ![修复图标](../../assets/fix.svg) **MariaDB 10.3验证** — 修复了Adobe Commerce 2.3.6-p1的MariaDB 10.3验证。<!--MCLOUD-7416-->

- ![修复图标](../../assets/fix.svg) **缓存:flush日志记录** — 改进了日志条目以指示`cache:flush`步骤的开始和完成。<!--MCLOUD-7503-->

## v2002.1.4

发行日期： 2020年11月19日

- ![修复图标](../../assets/fix.svg)修复了在`SEARCH_CONFIGURATION`环境变量中指定的搜索引擎是`elasticsearch`以外的值时导致部署失败的问题。<!--MCLOUD-7283-->

## v2002.1.3

发行日期： 2020年11月9日

**基础架构更新**—

- ![新图标](../../assets/new.svg)当静态内容设置为在生成阶段中部署时，为只读`pub/static`目录添加了ECE-Tools支持。<!--MC-37699-->

- ![新图标](../../assets/new.svg)已添加对Elasticsearch 7.9和Redis 6的支持，以便与即将发布的Adobe Commerce版本兼容。<!--MCLOUD-7191-->

- ![修复图标](../../assets/fix.svg)已更新ECE-Tools `composer.json`以添加质量修补程序工具所需的依赖项。 这修复了ECE-Tools和magento-cloud-patches包之间存在的循环依赖关系。<!--MCLOUD-6910-->

**验证和日志改进**—

- ![新图标](../../assets/new.svg)已添加搜索引擎验证，以确保在云基础架构2.4及更高版本上为Adobe Commerce设置`elasticsearch`。 如果验证失败，部署将停止，并显示一条关键错误消息，建议修复此问题。 查看[严重错误，部署阶段](../dev-tools/error-reference.md#deploy-stage)。<!--MCLOUD-6937-->

- ![新图标](../../assets/new.svg)已添加Elasticsearch验证，以检查Elasticsearch服务版本与Adobe Commerce版本之间的兼容性。<!--MCLOUD-7193-->

- ![新图标](../../assets/new.svg)更新了Elasticsearch兼容性错误消息，以显示与Adobe Commerce Elasticsearch模块兼容的Elasticsearch版本。 现在，错误消息会提供要在您的Elasticsearch基础架构中安装的特定Cloud版本，以便与您的Adobe Commerce版本使用的Elasticsearch模块兼容。 查看[警告错误，部署阶段](../dev-tools/error-reference.md#deploy-stage-1)。<!--MCLOUD-6698-->

- ![新图标](../../assets/new.svg)为无效的`2026`环境变量设置添加了警告错误`2027`和`MAGE_MODE`。 唯一有效值为`production`。 在此修复之前，可以将`MAGE_MODE`设置为`developer`，而不会出现部署错误，但只会在以后尝试写入只读文件时导致错误。 查看[警告错误](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![修复图标](../../assets/fix.svg)修复了Redis、RabbitMQ和MySQL服务的验证，以确保这些版本与Adobe Commerce版本兼容。 这些服务的有效版本现在写入`cloud.log`.<!--MCLOUD-7098-->

- ![修复图标](../../assets/fix.svg)已更新`cloud.log`以包含在缓存预热期间发送请求的并发请求限制。 此值在[WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency)部署后变量中进行配置。<!--MCLOUD-5563-->

**CLI命令更新**—

- ![新图标](../../assets/new.svg)添加了CLI命令（`cloud:config:create`和`cloud:config:update`），用于创建和更新`.magento.env.yaml`文件，其配置可以包括一个或多个生成、部署和部署后变量。 请参阅[从CLI创建配置文件](../environment/configure-env-yaml.md#create-configuration-file-from-cli)。<!--MCLOUD-7072-->

**环境变量更新**—

- ![新图标](../../assets/new.svg)已添加[SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload)内部版本变量。 将变量设置为`true`会在安装Cloud Docker for Commerce期间停止应用程序运行`composer dump-autoload`命令。 变量仅与具有可写文件系统（为使用`./vendor/bin/ece-docker build:compose --with-test`进行测试和开发而创建）的Commerce容器的Cloud Docker相关。 对于此类安装，跳过`composer dump-autoload`命令可防止在运行尝试从已删除的`generated`目录访问文件的其他命令时出现错误。<!--MCLOUD-6939-->

## v2002.1.2

发行日期： 2020年8月5日

**验证和日志改进**—

- ![新图标](../../assets/new.svg)添加了`schema.error.yaml`文件，该文件包含在生成、部署和部署后过程中可能发生的所有错误和警告通知以及解决错误的建议。 此文件中的信息也可在&#x200B;_Commerce云指南_&#x200B;中找到。 查看ece-tools[的](../dev-tools/error-reference.md)错误消息引用。<!--MCLOUD-5878-->

- ![新图标](../../assets/new.svg)已将云错误日志(`/var/log/cloud.error.log`)条目更改为JSON格式，以使该日志更易于以编程方式解析。<!--MCLOUD-5879-->

- ![新图标](../../assets/new.svg)添加了额外的错误检查以生成、部署和部署后处理，并改进了现有检查：

   - 错误代码2026 — 未能将构建阶段生成的一些数据恢复到装入的目录

   - 错误代码3004 — 无法创建备份文件

   - 错误代码102 — 添加了对`env.php`文件不可写<!--MCLOUD-6221-->时发生的问题的额外检查

- ![新图标](../../assets/new.svg)已添加&#x200B;**QUALITY_PATCHES**&#x200B;环境变量，以指定要在部署过程中应用的一个或多个质量修补程序。 查看[生成变量](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

发布日期： 2020年6月25日

- ![新图标](../../assets/new.svg) **基础架构更新**—

   - ![新图标](../../assets/new.svg) **日志记录改进** — 改进了日志跟踪功能，将退出代码分配给严重的部署错误，并在错误消息通知和日志事件中公开退出代码。 查看ece-tools[的](../dev-tools/error-reference.md)错误消息引用。<!-- MCLOUD-5637, 5531-->

   - ![新图标](../../assets/new.svg)改进了数据库转储的进程(`vendor/bin/ece-tools db-dump`)并更新了日志消息，以明确说明数据库转储操作将应用程序切换到维护模式，停止使用者队列进程，并在转储开始之前禁用cron作业。<!--MCLOUD-5324, MCLOUD-2062-->

   - ![修复图标](../../assets/fix.svg)修复了一个问题，以确保在部署到暂存环境和生产环境时项目URL正确更新。 现在，`ece-tools`在项目路由配置中设置了`primary:true`属性的路由使用该URL。 查看[部署变量](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![修复图标](../../assets/fix.svg)已更新用于应用修补程序的`generate.xml`生成方案工作流。 必须提前应用修补程序以更新Adobe Commerce，从而修复可能导致`di:compile`和`module:refresh`步骤失败的任何问题。<!--MCLOUD-5941-->

   - ![修复图标](../../assets/fix.svg)修复了安装过程中错误返回`Crypt key missing`错误的问题。 `crypt/key`值在安装期间自动生成。<!--MCLOUD-6120-->

- ![新图标](../../assets/new.svg) **服务更新**—

   - ![新图标](../../assets/new.svg)添加了对PHP 7.4和MariaDB 10.4的支持。<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![新图标](../../assets/new.svg) **环境变量更新**—

   - ![new icon](../../assets/new.svg)添加了&#x200B;**SCD_USE_BALER**&#x200B;变量，以便在Adobe Commerce on Cloud基础架构构建过程中为JavaScript捆绑启用包模块。 查看[生成变量](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->中的变量说明

   - ![new icon](../../assets/new.svg)已添加&#x200B;**REDIS_BACKEND**&#x200B;环境变量以为Adobe Commerce 2.3.5或更高版本的Redis缓存配置Redis后端模型。 查看[部署变量](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->中的变量说明

- ![新图标](../../assets/new.svg) **CLI命令更新**—

   - ![新图标](../../assets/new.svg)更新了以下CLI命令，其中包含用于更详细日志记录的选项：

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     每个调用的日志记录级别由[`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands)文件中`.magento.env.yaml`变量的配置决定。<!--MCLOUD-3503-->

- ![新图标](../../assets/new.svg) **验证改进**—

   - ![新图标](../../assets/new.svg) **Elasticsearch 7.x兼容性检查** — 更新了Elasticsearch验证以进行Elasticsearch 7.x软件兼容性检查。<!--MCLOUD-5542-->

   - ![新图标](../../assets/new.svg) **更新了服务版本和EOL验证检查** — 更新了验证以根据Adobe Commerce 2.4检查已安装的服务版本。要求。<!--MCLOUD-6144-->

   - ![修复图标](../../assets/fix.svg)修复了一个验证问题，以便仅在`post-deploy`文件中缺少`.magento.app.yaml`挂接配置时显示以下部署后警告消息：

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![新图标](../../assets/new.svg) **已添加Zend Framework依赖项验证** — 已添加已迁移到Laminas项目的Zend Framework的编辑器依赖项验证。 如果缺少所需的依赖关系，则在构建过程中会显示以下错误消息。

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     请参阅[验证Zend框架依赖项](../development/commerce-version.md#verify-zend-framework-composer-dependencies)。<!--MCLOUD-4094-->

   - ![新图标](../../assets/new.svg) **已添加`env.php`文件和数据的验证** — 已在安装和升级过程中添加对`env.php`文件和数据的检查。<!--MCLOUD-5991-->

      - 如果安装中缺少`env.php`文件，并且未在`crypt/key`文件中指定`.magento.app.yaml`值，则部署将失败，并出现以下通知：

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - 如果安装不包含`env.php`文件，或配置仅包含一个缓存类型，则在升级过程中将运行`cron:enable`命令以恢复包含所有`cache_types`的文件。 以下通知将添加到日志中：

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

发行日期： 2020年2月6日

- ![新图标](../../assets/new.svg) **基础架构更新**—

   - ![新图标](../../assets/new.svg) **为Commerce的Cloud Docker添加了单独的包** — 将Docker包与`ece-tools`包分离，以保持代码质量并提供独立的发行版。 从`ece-tools`magento-cloud-docker[&#x200B; GitHub存储库中管理与](https://github.com/magento/magento-cloud-docker)相关的更新和修复。<!--MAGECLOUD-2927-->

   - ![新图标](../../assets/new.svg) **更新了修补功能** — 已将修补功能从ECE-Tools包移动到单独的[magento-cloud-patches](https://github.com/magento/magento-cloud-patches)包。 在部署期间，`ece-tools`使用新包来应用修补程序。 请参阅[Cloud修补程序发行说明](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![新图标](../../assets/new.svg) **已更新编辑器依赖项** — 已更新云基础架构上Adobe Commerce的`composer.json`文件，该文件依赖于`magento/magento-cloud-docker`包。 现在，`ece-tools`包含[`Cloud Tools Suite for Commerce`](cloud-tools-suite.md)中所有包的依赖项。 安装或更新`ece-tools`时，会自动安装和更新这些软件包。

- ![新图标](../../assets/new.svg) **支持基于方案的部署**—<!--MAGECLOUD-4101-->

   - ![新图标](../../assets/new.svg)现在您可以使用XML配置文件自定义生成、部署和部署后进程以覆盖或自定义默认配置。

   - ![新图标](../../assets/new.svg) **已在`hooks`中更改`.magento.app.yaml`**&#x200B;配置 — 我们更新了`hooks`配置格式以支持基于方案的部署。 旧版ECE-Tools 2002.0.x仍然受支持。 但是，必须更新为新格式才能使用基于场景的部署功能。 请参阅[基于方案的部署](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks)。

>[!NOTE]
>
>在更新到ECE-Tools版本2002.1.0之前，请向后查看[&#x200B;   不兼容的更改](backward-incompatible-changes.md)，以了解可能需要您   在云基础架构项目配置或流程上更新Adobe Commerce。

- ![新图标](../../assets/new.svg) **服务更新**—

   - ![新图标](../../assets/new.svg)添加了对PHP 7.3.<!--MAGECLOUD-4022-->的支持

   - ![新图标](../../assets/new.svg)已添加对RabbitMQ 3.8.<!--MAGECLOUD-4674-->的支持

   - ![新图标](../../assets/new.svg)已添加验证，以根据每个服务的EOL日期检查已安装的服务版本。 现在，如果服务版本在EOL日期后的三个月内，客户将收到通知，如果EOL日期在过去，客户将收到警告。<!--MAGECLOUD-4076-->

   - ![修复图标](../../assets/fix.svg)修复了Elasticsearch配置问题，以确保在所有环境中都配置了正确的Elasticsearch设置。<!--MAGECLOUD-4474-->

>[!NOTE]
>
>请参阅[服务版本](../services/services-yaml.md#service-versions)，以获取云基础架构上Adobe Commerce中使用的服务及其版本与Cloud模板的兼容性列表。

- ![新图标](../../assets/new.svg) **环境变量更新**—

   - ![新图标](../../assets/new.svg)扩展了`WARM_UP_PAGES`环境变量的功能，以支持特定产品页面的缓存预加载。 请参阅[部署后变量](../environment/variables-post-deploy.md#warm_up_pages)主题中的扩展定义。<!--MAGECLOUD-4444-->

   - ![新图标](../../assets/new.svg)添加了`ERROR_REPORT_DIR_NESTING_LEVEL`环境变量，以简化`<magento_root>/var/report/`目录中的错误报告数据管理。 请参阅[生成变量](../environment/variables-build.md#error_report_dir_nesting_level)主题中的变量说明。

   - ![修复图标](../../assets/fix.svg)已删除`SCD_EXCLUDE_THEMES`、`STATIC_CONTENT_THREADS`、`DO_DEPLOY_STATIC_CONTENT`和`STATIC_CONTENT_SYMLINK`环境变量。 查看[向后不兼容的更改](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![修复图标](../../assets/fix.svg)修复了Elastic Suite配置过程中的问题，以便在配置`ELASTICSUITE_CONFIGURATION`部署变量而不使用`_merge`选项时，按预期覆盖默认配置。<!--MAGECLOUD-4388-->

- ![新图标](../../assets/new.svg) **CLI命令更新**—

   - ![新图标](../../assets/new.svg) **新cron命令** — 您现在可以使用`cron:disable`和`cron:enable`命令在Adobe Commerce中在云基础架构环境中手动管理cron处理。 使用disable命令停止所有活动的cron进程并禁用所有cron作业。 准备就绪后，使用enable命令重新启用cron作业。 请参阅[禁用cron作业](../application/crons-property.md#disable-cron-jobs)。

   - ![新图标](../../assets/new.svg) **改进了错误报告** — 为ECE-Tools处理期间发生的CLI命令失败添加了更好的日志记录。<!--MAGECLOUD-4849-->

   - ![新图标](../../assets/new.svg) **删除已弃用的生成命令** — 删除了以下生成命令： `m2-ece-build`、`m2-ece-deploy`、`m2-ece-scd-dump`，并将`ece-tools docker`命令重命名为`ece-docker`。 查看[向后不兼容的更改](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![新图标](../../assets/new.svg)删除了已弃用的`build_options.ini`文件并添加了验证，如果文件存在，则生成会失败。 使用[.magento.env.yaml](../environment/configure-env-yaml.md)文件配置生成选项。

- ![修复图标](../../assets/fix.svg)修复了`config.php`文件为空时导致生成过程失败的问题。<!--MAGECLOUD-4127-->

## 2002.0.23

发行日期： 2020年2月27日

- ![修复图标](../../assets/fix.svg)修复了`ece-tools` 2002.0.x版本的兼容性问题，该问题导致按需静态内容生成无法在生产模式下成功完成。

## 旧版本

请参阅版本2002.0.22和更早版本的[发行说明存档](cloud-release-archive.md)。
