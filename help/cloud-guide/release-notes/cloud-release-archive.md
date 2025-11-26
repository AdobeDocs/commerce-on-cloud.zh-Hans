---
title: ece-tools的发行说明存档
description: 了解ece-tools的存档改进。
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 3ba39fa6-88e9-4177-956d-f3e382bf59e3
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '7145'
ht-degree: 0%

---

# ece-tools的发行说明存档

>[!NOTE]
>
>这些发行说明提供了`ece-tools` v2002.0.22及更高版本的信息和更新。 请参阅[Cloud Tools Suite的发行说明](cloud-tools-suite.md)以获取`ece-tools`和其他云包的最新更新。

## v2002.0.22

`ece-tools` 2002.0.22版本更改了`ece-tools`包的结构以将`Adobe Commerce on cloud infrastructure`修补程序版本与ECE-Tools版本分离。 从此版本开始，将使用[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)包交付修补程序和关键修补程序，该包是`ece-tools`包的新依赖项。 我们进行了这些更改，以降低计划发布更新以及处理社区贡献的复杂性。

- ![新图标](../../assets/new.svg) **对ECE-Tools包的更改**

   - ![new icon](../../assets/new.svg)已将Adobe Commerce修补程序从`ece-tools`包移动到新的[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)编辑器包。

   - ![新图标](../../assets/new.svg)已更新`composer.json`包的`ece-tools`文件以添加`magento/magento-cloud-patches` v1.0.0包的依赖项。

   - ![修复图标](../../assets/fix.svg)修复了从2.3.2-p2及更高版本开始，在仅安全版本之上应用修补程序集时导致`ece-tools`修补过程中断的问题。 此问题是由为[仅限安全的修补程序](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->采用的新版本化方案引入的

- ![修复图标](../../assets/fix.svg) **修补程序和关键修复** — 使用`ece-tools`版本2002.0.22更新您的云环境以应用以下修补程序和关键修复。 这些修补程序包含在`magento/magento-cloud-patches` v1.0.0程序包中。

   - ![修复图标](../../assets/fix.svg) **适用于2.3.1.x和2.3.2.x版本的Page Builder安全修补程序** — 修复了Page Builder预览中的问题，该问题允许未经身份验证的用户访问某些模板化方法，这些方法可用于通过网络触发任意代码执行(RCE)，从而导致全局信息泄漏。 在Adobe Commerce版本2.3.1和2.3.2.<!--MAGECLOUD-4649-->中使用不受支持的页面生成器版本时，可能会出现此问题

   - ![修复图标](../../assets/fix.svg) **MSI修补程序** — 修复了在使用默认库存设置管理库存时导致索引错误和性能问题的情况。<!--MAGECLOUD-4428-->

   - ![修复图标](../../assets/fix.svg) **新邮件接口的向后兼容性** — 修复了Adobe Commerce v2.3.3中引入的`Magento\Framework\Mail\EmailMessageInterface` PHP接口导致的向后不兼容问题。在此修补程序的范围内，新`EmailMessageInterface`继承自旧`MessageInterface`，Adobe Commerce核心模块将还原为依赖于`MessageInterface`。<!--MAGECLOUD-4422-->

   - ![修复图标](../../assets/fix.svg) **目录分页在Elasticsearch 6.x上不起作用** — 修复了搜索结果分页的一个严重问题，该问题影响使用Elasticsearch 6.x作为目录搜索引擎的客户。<!--MAGECLOUD-4448-->

## v2002.0.21

- ![新图标](../../assets/new.svg) **Docker更新**—

   - ![新图标](../../assets/new.svg) **新Docker映像** — 受版本2.3.3和更高版本支持<!-- MAGECLOUD-3345 -->

      - PHP版本7.3.<!-- MAGECLOUD-4017 -->

      - 清漆缓存6.2.0<!-- MAGECLOUD-4017 -->

   - ![新图标](../../assets/new.svg)添加支持在Docker环境中应用`.magento.app.yaml`中指定的自定义挂接配置。 以前，Docker环境仅支持默认挂接配置。<!-- MAGECLOUD-3505-->

   - ![新图标](../../assets/new.svg) Docker环境文件在Docker构建期间不再生成，并且`docker:config:convert`命令已弃用。 相应的数据现在存储在`docker-compose.yml`文件中。<!-- MAGECLOUD-3816-->

   - ![新图标](../../assets/new.svg) **已更新PHP映像** — 已将Node.js添加到PHP Docker映像以支持node、npm和grunt-cli功能。<!-- MAGECLOUD-3953 -->

- ![新图标](../../assets/new.svg) **环境变量更新**-

   - ![新图标](../../assets/new.svg)添加了&#x200B;**LOCK_PROVIDER**&#x200B;部署变量以配置锁定提供程序，该变量阻止启动重复的cron作业和cron组。 请参阅[部署变量](../environment/variables-deploy.md#lock_provider)主题中的变量说明。<!-- MAGECLOUD-4052 -->

   - ![新图标](../../assets/new.svg)添加了&#x200B;**CONSUMERS_WAIT_FOR_MAX_MESSAGES**&#x200B;环境变量，以配置在使用`CRON_CONSUMERS_RUNNER`环境变量管理cron作业时使用者如何处理来自消息队列的消息。 请参阅[部署变量](../environment/variables-deploy.md#consumers_wait_for_max_messages)主题中的变量说明。<!-- MAGECLOUD-4071 -->

   - ![修复图标](../../assets/fix.svg)修复了当`consumers_runner` cron作业在不同节点上启动同一使用者的多个实例时可能导致数据库死锁错误的问题。 现在，如果您在环境中启用了&#x200B;[**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner)&#x200B;部署变量，`consumers_runner`作业将使用`single-thread`选项在一个节点上启动每个使用者的一个实例。<!-- MAGECLOUD-3913 -->

   - ![修复图标](../../assets/fix.svg)修复了影响使用默认存储URL的&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;功能的问题。 现在，如果`config:show:default-url`命令无法获取基本URL，则使用MAGENTO_CLOUD_ROUTES变量中的URL。<!-- MAGECLOUD-3866 -->

- ![新图标](../../assets/new.svg)更新了`module:refresh`命令返回的日志信息。 现在，您可以在`cloud.log`文件中看到已启用的模块的详细列表。<!-- MAGECLOUD-2514 -->

- ![新图标](../../assets/new.svg)针对Adobe Commerce版本与已安装的服务(如Elasticsearch、[!DNL RabbitMQ]、Redis和DB)之间的兼容性问题，改进了版本兼容性验证和警告通知。<!-- MAGECLOUD-3535 -->

- ![新图标](../../assets/new.svg)已添加对RabitMQ版本3.8.<!-- MAGECLOUD-4674-->的支持

- ![新图标](../../assets/new.svg)更新了服务兼容性的交互式验证，以反映新的Adobe Commerce 2.3.3和2.2.10版本支持的版本。 有关推荐的版本，请参阅[安装指南](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)中的&#x200B;_系统要求_。<!-- MAGECLOUD-4018 -->

- ![修复图标](../../assets/fix.svg)改进了部署阶段的cron作业管理进程尝试停止已完成的cron作业时返回的日志消息，以澄清此问题不是错误。 已将日志级别从`INFO`更改为`DEBUG`。<!-- MAGECLOUD-3653-->

- ![修复图标](../../assets/fix.svg)修复了在运行`setup:upgrade`命令时未在`app:config:import`任务期间发生失败时中断部署过程的问题。<!-- MAGECLOUD-3806 -->

- ![新图标](../../assets/new.svg)已将文件处理程序的默认日志级别更改为`debug`以减少[!DNL Cloud Console]中显示的日志中的详细信息量，同时仍提供调试的详细信息。<!-- MAGECLOUD-3871 -->

- ![修复图标](../../assets/fix.svg)修复了在生成期间导致静态内容部署错误的问题。 安装和`ece-tools`配置转储后，如果在`config.php`文件中没有为管理员用户指定区域设置，则会发生错误。 现在，`config.php`文件中存在管理员用户的默认区域设置。<!-- MAGECLOUD-3957 -->

- ![修复图标](../../assets/fix.svg)修复了在未配置安全URL (https)的环境中执行`Undefined index error` CLI命令失败时出现的`magento-cloud`。 现在，如果安全URL不可用，ECE-Tools包将使用基本URL (http)。<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![新图标](../../assets/new.svg) **Docker更新**—

   - ![新图标](../../assets/new.svg)您现在可以在Docker环境中使用`ece-tools`包执行功能测试。 查看[应用程序测试](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing).<!-- MAGECLOUD-3129/3684 -->

   - ![新图标](../../assets/new.svg)添加了对使用`.magento.app.yaml`文件配置PHP模块的支持。 在[文件`.magento.app.yaml`中指定的任何](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions)PHP扩展将在Docker PHP容器中变得可用。<!-- MAGECLOUD-3357 -->

   - ![新图标](../../assets/new.svg)有新命令可用于改进Docker命令行体验。 查看Docker引用[`bin/magento-docker`.](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli)的<!-- MAGECLOUD-3569 -->部分

   - ![新图标](../../assets/new.svg)添加了使用Mutagen.io在本地主机和Docker之间进行开发期间同步文件的功能。<!-- MAGECLOUD-3559 -->

   - ![修复图标](../../assets/fix.svg)在使用Docker环境时更正了默认路径。 现在，当您使用SSH登录到Docker容器时，您按预期位于`/app`目录中的项目根目录。<!-- MAGECLOUD-3582 -->

   - ![修复图标](../../assets/fix.svg)已将钠库从版本1.0.11更新到版本1.0.18，并更新了Na PHP扩展。<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >云基础架构上的Adobe Commerce客户必须[提交Adobe Commerce支持票证](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket)，以便在升级到Adobe Commerce 2.3.2之前在专业生产和暂存环境中升级libna包。目前，您无法将入门环境升级到Adobe Commerce 2.3.2。

   - ![修复图标](../../assets/fix.svg)已将`analysis-icu`和`analysis-phonetic`个Elasticsearch插件添加到所有Docker图像。<!-- MAGECLOUD-3446 -->

   - ![修复图标](../../assets/fix.svg)改进的验证：为`docker:build`命令使用选项时，必须在使用选项时提供值。 此外，在使用`docker:build run`命令时添加了节点版本的验证。<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![新图标](../../assets/new.svg) **环境变量更新**—

   - ![新图标](../../assets/new.svg)添加了对使用[DATABASE_CONFIGURATION环境变量](../environment/variables-deploy.md#database_configuration)的数据库表前缀的支持。<!-- MAGECLOUD-2901 -->

   - ![new icon](../../assets/new.svg)添加了&#x200B;**FORCE_UPDATE_URLS**&#x200B;部署变量，以便在部署到Pro和Starter生产和暂存环境时更新基本URL。 查看[部署变量](../environment/variables-deploy.md#force_update_urls)内容中的定义。<!-- MAGECLOUD-3602 -->

   - ![新图标](../../assets/new.svg)添加了&#x200B;**TTFB_TESTED_PAGES**&#x200B;部署后变量以配置&#x200B;_到第一个字节的时间_&#x200B;页面测试以检查部署到云基础架构的站点上的应用程序性能。 查看[部署后变量](../environment/variables-post-deploy.md)中的变量说明。<!-- MAGECLOUD-3643 -->

   - ![修复图标](../../assets/fix.svg)修复了多线程SCD的问题，该问题会导致静态内容部署中出现随机失败。 解决方法是将&#x200B;**SCD_THREADS**&#x200B;变量设置为`1`。 您现在可以根据需要增加数量。 查看[部署变量](../environment/variables-deploy.md#scd_threads)和[生成变量](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->中的定义

   - ![修复图标](../../assets/fix.svg)您可以配置&#x200B;**WARM_UP_PAGES**&#x200B;环境变量以缓存单个页面、多个域和多个页面。 查看[部署后变量](../environment/variables-post-deploy.md#warm_up_pages)内容中的扩展定义。<!-- MAGECLOUD-3258 -->

- ![修复图标](../../assets/fix.svg)已将`pub/static/.htaccess`文件添加到排除列表。 由PHOENIX MEDIA GmbH的Bjorn Kraus提交的[修复](https://github.com/magento/ece-tools/pull/455)。<!-- MAGECLOUD-3545/Github#455 -->

- ![修复图标](../../assets/fix.svg)修复了在至少一个关键级别验证器返回错误时，所有验证消息显示为`Critical`的错误。<!-- MAGECLOUD-3178 -->

- ![修复图标](../../assets/fix.svg)修复了在数据库中不存在基础URL时导致部署失败的问题。<!-- MAGECLOUD-3075 -->

- ![新图标](../../assets/new.svg)向&#x200B;**`env:config:show`包中添加了新的**&#x200B;命令`ece-tools`，该包显示环境服务、路由或变量。 请参阅[服务、路由和变量](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables)。 [Vladimir Kerkhoff提交的功能](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![修复图标](../../assets/fix.svg)修复了在外壳重构后尝试安装Adobe Commerce 2.2.6或更早版本（具有`ece-tools`开发）时导致严重错误的问题。<!-- MAGECLOUD-3665 -->

- ![修复图标](../../assets/fix.svg)修复了导致Adobe Commerce 2.1.x和2.2.x安装失败的问题，并警告您使用已弃用的Carbon版本。<!-- MAGECLOUD-3704 -->

- ![修复图标](../../assets/fix.svg)已将shell输出的`cloud.log`日志级别从`info`降低为`debug`。<!-- MAGECLOUD-3277 -->

- ![修复图标](../../assets/fix.svg)已将`--remove-definers (-d)`选项添加到`ece-tools db-dump`命令以从转储文件中删除定义符。<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![修复图标](../../assets/fix.svg)修复了在部署期间覆盖`env.php`文件导致自定义配置丢失的问题。 此更新确保Adobe Commerce on cloud infrastructure使用每个部署更新`env.php`文件，同时保留自定义配置。<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![新图标](../../assets/new.svg) **Docker更新**—

   - ![新图标](../../assets/new.svg)现在，Docker环境支持.magento.app.yaml文件[.](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property)的<!-- MAGECLOUD-3150 -->crons属性中定义的cron配置

   - ![新图标](../../assets/new.svg) **新Docker容器** — 添加了[TLS终止代理容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container)，以便于通过HTTPS终止Varnish SSL。<!-- MAGECLOUD-2890 -->

   - ![新图标](../../assets/new.svg) **新Docker映像** — 添加了Node.js映像以支持Gulp和其他功能，如Jasmine JS单元测试。<!-- MAGECLOUD-3345 -->

   - ![新图标](../../assets/new.svg) **Docker生成模式** — 现在您可以选择在[生产模式或开发人员模式](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode)中启动Docker环境。 开发人员模式支持具有完全可写文件系统权限的活动开发。<!-- MAGECLOUD-3152/3511 -->

   - ![修复图标](../../assets/fix.svg)修复了在缓存针对不可用的服务配置时，导致Docker部署失败并出现`Name or service not known`错误的问题。 现在，您可以从[`.magento/services.yaml`文件](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)中删除服务。 Docker配置生成器自动更新`docker/config.php.dist`文件中的服务。<!-- MAGECLOUD-3369 -->

   - ![新图标](../../assets/new.svg)添加了对服务兼容性的交互式验证。 现在，如果请求的服务与Adobe Commerce版本或其他服务不兼容，_交互模式_&#x200B;将提示用户一条消息并选择继续。 查看可用于Docker的[服务版本](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers)。 使用`-n`选项跳过用于CICD的交互。<!-- MAGECLOUD-3251 -->

   - ![修复图标](../../assets/fix.svg)修复了Docker合成`db-dump`命令中擦除现有转储的问题。<!-- MAGECLOUD-3366 -->

   - ![修复图标](../../assets/fix.svg)修复了将Redis `session`、`default`和`page_cache`缓存存储分配到同一数据库ID的问题。<!-- MAGECLOUD-3172 -->

- ![新图标](../../assets/new.svg) **环境变量更新**—

   - ![新图标](../../assets/new.svg)新的&#x200B;**ELASTICSUITE\_CONFIGURATION**&#x200B;环境变量会在部署之间保留自定义的服务设置。 查看[部署变量](../environment/variables-deploy.md#elasticsuite_configuration)内容中的定义。<!-- MAGECLOUD-3205 -->

   - ![新图标](../../assets/new.svg)添加了&#x200B;**SCD_MAX_EXECUTION_TIMEOUT**&#x200B;环境变量，以便您可以增加从`.magento.env.yaml`文件完成静态内容部署的时间。 查看[部署变量](../environment/variables-deploy.md#scd_max_execution_time)、[生成变量](../environment/variables-build.md#scd_max_execution_time)和[全局变量](../environment/variables-global.md#scd_max_execution_time)中的定义。<!-- MAGECLOUD-2822 -->

      - ![新图标](../../assets/new.svg)已添加&#x200B;**MAGENTO_CLOUD_LOCKS_DIR**&#x200B;环境变量，以便为云基础架构上的锁定提供程序配置装入点的路径。 锁定提供程序阻止启动重复的cron作业和cron组。 Adobe Commerce版本2.2.5及更高版本支持此变量，并且会自动配置此变量。 查看[云变量](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->中的定义

      - ![修复图标](../../assets/fix.svg)更改了&#x200B;**SCD_THREADS**&#x200B;环境变量默认值，以根据检测到的CPU线程计数自动确定最佳值。 查看[部署变量](../environment/variables-deploy.md#scd_threads)和[生成变量](../environment/variables-build.md#scd_threads)中更新的定义。<!-- MAGECLOUD-3382 -->

- ![修复图标](../../assets/fix.svg)修复了在2002.0.16版的云基础架构上升级到Adobe Commerce时，导致错误的数据库隔离机制修补程序问题。<!-- MAGECLOUD-3383 -->

- ![修复图标](../../assets/fix.svg)添加了一个修补程序，该修补程序将&#x200B;_Google图像图表_&#x200B;替换为&#x200B;_图像图表_。 请参阅DevBlog文章[M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006)的Google图像图表弃用和更新。<!-- MAGECLOUD-3456 -->

- ![修复图标](../../assets/fix.svg)已添加[SEARCH_CONFIGURATION变量](../environment/variables-deploy.md#search_configuration)的验证。 未设置“engine”选项且不需要使用`_merge`时，部署失败。<!-- MAGECLOUD-3470 -->

- ![修复图标](../../assets/fix.svg)修复了在发生异常后公开敏感数据的问题。 现在，敏感信息已适当屏蔽。<!-- MAGECLOUD-3525 -->

- ![修复图标](../../assets/fix.svg)改进了Magento Open Source包的容错设置。 在Adobe Commerce无法从Redis `slave`实例读取数据的情况下，将从Redis `master`实例进行读取。 请参阅[REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>`ece-tools`版本2002.0.17包含一个重要的安全修补程序。 请参阅[技术资源： Magento Open Source修补程序](https://magento.com/tech-resources/download#download2288)。

- ![新图标](../../assets/new.svg) **服务更新** — 以下Adobe Commerce版本支持： 2.2.8及更高版本2.2.x、2.3.1及更高版本2.3.x

   - 添加了对Elasticsearch版本6.x.<!-- MAGECLOUD-3196 -->的支持

   - 添加了对Redis版本5.0的支持。

- ![新图标](../../assets/new.svg) **新Docker映像** — 已将以下服务添加到Docker内部版本：

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![新图标](../../assets/new.svg) **新环境变量** — 以前，SCD压缩存在硬编码超时。 现在可以使用&#x200B;**SCD_COMPRESSION_TIMEOUT**&#x200B;环境变量配置SCD压缩超时。 查看[生成变量](../environment/variables-build.md#scd_compression_timeout)和[部署变量](../environment/variables-deploy.md#scd_compression_timeout)内容中的定义。<!-- MAGECLOUD-2870 -->

- ![修复图标](../../assets/fix.svg)已将`--use-rewrites`选项添加到安装命令中，以便该命令对店面中生成的链接使用Web服务器重写以及管理员访问权限来改进安全性和客户体验。<!-- MAGECLOUD-3246 -->

- ![修复图标](../../assets/fix.svg)已向`var/log/install_upgrade.log`文件添加时间戳，以便该文件显示安装和升级事件的日期。<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![新图标](../../assets/new.svg) **Docker更新**—

   - 现在，在Docker环境中生成的默认服务配置与云模板中的默认配置相同。<!-- MAGECLOUD-3025 -->

   - 您可以使用`sendmail`服务从Docker环境发送邮件。<!-- MAGECLOUD-2907 -->

   - 添加了[将Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug)配置为在Cloud Docker环境中调试的功能。<!-- MAGECLOUD-2891 -->

   - 修复了在生成`docker-compose.yml`文件时有关Web服务权限的问题。<!-- MAGECLOUD-2883 -->

- ![新图标](../../assets/new.svg) **升级改进** — 添加了验证，以确认`autoload`文件中的`composer.json`属性在升级到Adobe Commerce v2.3之前包含所需的配置更改。请参阅[升级版本](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version)。<!-- MAGECLOUD-2392 -->

- ![新图标](../../assets/new.svg)部署静态内容的压缩过程现在包括本机生成或自定义的所有资源，并且在[`build:transfer`部分](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property)的开头的生成阶段发生。 以前，压缩过程在应用自定义缩小和静态资源捆绑之前进行。 由Rafael Garcia Lepper从Tryzens Limited提交的[修复](https://github.com/magento/ece-tools/pull/413)。<!-- MAGECLOUD-3104 -->

- ![修复图标](../../assets/fix.svg)修复了在配置其他数据库和服务关系后立即在部署期间发生的数据库连接错误。 此外，此修复还解决了在Commerce Reporting for Starter配置过程中发生的问题。 首先，此升级对于使用Commerce报表而言是“必须具有”的。<!-- MAGECLOUD-3035 -->

- ![修复图标](../../assets/fix.svg)修复了导致部署过程失败的数据库配置验证问题。<!-- MAGECLOUD-3003 -->

- ![修复图标](../../assets/fix.svg)已使用要与`symfony/yaml`PHP常量[一起使用的相应版本的](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)包更新约束。 使用版本低于3.2的`symfony/yaml`包时，常量分析不起作用。[由Vladimir Kerkhoff提交的修复](https://github.com/magento/ece-tools/pull/404)。<!-- MAGECLOUD-2956 -->

- ![新图标](../../assets/new.svg) **环境配置检查** — 添加了验证，用于检查PHP版本并在用户未使用最新的推荐版本时警告用户。<!--MAGECLOUD-2903-->

- ![修复图标](../../assets/fix.svg)修复了处理格式错误的JSON变量时出现的问题。 现在，如果JSON变量导致语法错误，则`cloud.log`文件中将显示警告，并且使用默认变量继续部署。<!-- MAGECLOUD-2851 -->

- ![修复图标](../../assets/fix.svg)修复了在禁用Redis服务后立即在部署期间发生的连接错误。<!-- MAGECLOUD-2747 -->

- ![新图标](../../assets/new.svg) **正在记录更改** — 已将以下生成和部署过程事件的[日志级别](../environment/log-handlers.md#log-levels)从`Info`更新为`Notice`：<!--MAGECLOUD-2925-->

   - 将`composer.json`中已安装的模块与`app/etc/config.php`文件中的共享配置设置进行协调的进程开始和结束

   - 配置验证过程的开始和结束

   - 生成类的`setup:di:compile`进程的开始和结束

- ![新图标](../../assets/new.svg) **新环境变量**—

   - **[RESOURCE_CONFIGURATION部署变量](../environment/variables-deploy.md#resource_configuration)** — 使用此变量将资源名称映射到数据库连接。<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION全局变量](../environment/variables-global.md#x_frame_configuration)** — 使用此变量更改在`X-Frame-Options`、`<frame>`或`<iframe>`中呈现Adobe Commerce页面的`<object>`标题配置。<!-- MAGECLOUD-3048 -->

- ![修复图标](../../assets/fix.svg) **环境变量更新** — 已更改以下环境变量：

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** — 添加了为为Adobe Commerce存储定义的所有域上的指定页面预加载缓存的功能。 以前，如果您的站点配置了多个域，则部署后进程未能在非默认域上预加载指定页面的缓存，并在部署后日志中返回以下错误： `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** — 已使用SCD压缩级别的正确默认值更新文档和示例`.magento.env.yaml`文件。 查看[生成变量](../environment/variables-build.md#scd_compression_level)和[部署变量](../environment/variables-deploy.md#scd_compression_level)内容中的定义。<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** — 已弃用此环境变量。 使用[SCD_MATRIX](../environment/variables-build.md#scd_matrix)控制主题配置。<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** — 修复了验证过程，以防止在SCD_MATRIX忽略包含不同字符大小写的主题值时发生问题。 查看[生成变量](../environment/variables-build.md#scd_matrix)和[部署变量](../environment/variables-deploy.md#scd_matrix)内容中的定义。<!-- MAGECLOUD-2904 -->

   - **管理员变量**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 提高了使用环境变量管理管理员用户凭据时的安全性。 在升级过程中，不能再使用ADMIN_EMAIL、ADMIN_USERNAME和ADMIN_PASSWORD环境变量覆盖管理员凭据。 如果无法访问管理员面板，请使用&#x200B;_忘记密码_&#x200B;功能或`admin:user:create` CLI命令来创建新的管理员用户。 查看[访问您的管理员面板](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin)。

      - 升级或应用修补程序时不再需要ADMIN_EMAIL。

## v2002.0.15

- ![新图标](../../assets/new.svg) **Docker更新**—

   - 现在，当`.magento.app.yaml`构建Docker环境`.magento/services.yaml`时，Docker生成器使用[和](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)配置文件中指定的服务。 您可以使用生成参数选择其他服务版本。<!-- MAGECLOUD-2888 -->

   - 添加了PHP 7.2映像 — 在Cloud Docker中添加了对PHP 7.2的支持；更新了[Launch Docker配置](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)以包含`docker:build --php`选项，从而指定与您的Adobe Commerce版本兼容的PHP版本。<!-- MAGECLOUD-2799 -->

   - 添加了基于PHP-CLI映像的[Cron容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli#cron-container)。<!-- MAGECLOUD-2565 -->

   - 已将以下服务添加到Docker版本：

      - [!DNL RabbitMQ] 3.5和3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7、2.4和5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2和4.0<!-- MAGECLOUD-2886 -->

- ![新图标](../../assets/new.svg) **使用PHP常量进行配置** — 已在[配置文件中添加对](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)PHP常量`.magento.env.yaml`的支持。<!-- MAGECLOUD- 2575 -->

- ![新图标](../../assets/new.svg) **新环境变量** — 默认情况下，只有生产环境启用了Google Analytics。 您可以使用[ENABLE_GOOGLE_ANALYTICS环境变量](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->在暂存环境和集成环境中启用Google Analytics

- ![修复图标](../../assets/fix.svg)修复了在重新部署后从`env.php`文件中删除自定义cron配置的问题。 现在，自定义cron配置安全地保留在`env.php`文件中。<!-- MAGECLOUD-2923 -->

- ![修复图标](../../assets/fix.svg)修复了生成、部署和部署后阶段的消息和[日志级别](../environment/log-handlers.md#log-levels)中的不一致。 将所有阶段和子阶段的开始和结束日志消息级别从&#x200B;**信息**&#x200B;增加到&#x200B;**通知**。 添加开始和结束日志消息（如果适用）。<!-- MAGECLOUD-2919 -->

- ![修复图标](../../assets/fix.svg)修复了在配置后阻止启动部署后阶段的cron进程的问题。 现在，如果您启用了部署后挂接，则会在部署后阶段的开始时再次启用cron进程。<!-- MAGECLOUD-2862 -->

- ![修复图标](../../assets/fix.svg)解决了在指定自定义数据库配置时阻止成功安装Adobe Commerce的问题。 以前，安装过程使用[MAGENTO_CLOUD_RELATIONSHIP变量](../environment/variables-cloud.md)的数据库配置，即使您在[DATABASE_CONFIGURATION环境变量](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->中指定了自定义连接信息也是如此

- ![修复图标](../../assets/fix.svg)已更正`config:dump`命令，以便该命令包含`system`文件的`config.php`部分中的每个网站区域设置。<!--MAGECLOUD-2740-->

- ![修复图标](../../assets/fix.svg)通过更正源基本URL引用，修复了在部署后阶段导致&#x200B;_预热错误_&#x200B;的问题。<!--MAGECLOUD-2797-->

- ![修复图标](../../assets/fix.svg)修复了在`setup:di:compile`过程中不正确地生成文件的问题，该问题影响Amazon支付模块。<!--MAGECLOUD-2850-->

## v2002.0.14

- ![新图标](../../assets/new.svg) **验证理想状态** — 现在，`ideal-state`向导会在每次部署期间验证当前配置，并提供有关更新配置的明确说明，以实现更快的零停机部署。<!--MAGECLOUD-2372-->

- ![修复图标](../../assets/fix.svg) **PCI合规性** — 已更新云基础架构上Adobe Commerce的消息协议，以便在与第三方消息服务连接时要求使用传输层安全性(TLS)版本1.2。 如果您使用的消息服务不支持TLS版本1.2，则必须升级您的服务。 否则，当您的Adobe Commerce应用程序尝试连接到消息服务器以发送电子邮件时，会显示以下错误消息： `Unable to connect via TLS`。<!--MAGECLOUD-2521-->

- ![新图标](../../assets/new.svg) **部署改进** — 添加了验证功能，可在暂存或生产环境启用了`dev`、`debug`或`debug_logging`选项时警告客户，以防止因过多的日志记录活动而导致的性能问题。<!--MAGECLOUD-2517-->

- ![修复图标](../../assets/fix.svg) **部署修复**—

   - 现在，维护模式在部署阶段开始时启用，在部署阶段结束时禁用。 如果部署失败，站点将保持维护模式，直到解决部署问题为止。 以前，即使部署失败，站点也会返回到生产模式。<!--MAGECLOUD-2603-->

      - 已重新处理部署阶段验证检查，将以下部署问题的错误级别从`CRITICAL`降级为`WARNING`，以便完成部署。 以前，这些问题会导致部署失败。

      - 环境配置包含不正确的部署或云变量值。

   - 云基础架构上的Elasticsearch版本与云基础架构上的Adobe Commerce支持的elasticsearch/elasticsearch模块的版本不兼容。 请参阅Adobe Commerce支持知识库中的[Elasticsearch疑难解答文章](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento)。<!--MAGECLOUD-2600-->

   - 修复了`app/etc/config.php`文件中的共享配置设置的问题，该问题在部署期间导致`recursion detected`错误。<!--MAGECLOUD-2173-->

- ![修复图标](../../assets/fix.svg) **Cron相关修复**—

   - 修复了在指定默认（1分钟）以外的cron频率时阻止作业运行的cron计划问题。<!--MAGECLOUD-2602-->

   - 修复了部署阶段中存在的一个问题，该问题会导致cron作业在部署期间继续运行，从而可能导致数据库锁定和其他严重问题。 现在，所有cron作业都将在部署阶段开始之前停止，并在部署完成后重新启动。&lt;！—MAGECLOUD—2537—>

   - 修复了2.2.x版中的cron作业工作流以解锁冻结的cron作业，以便在开始部署之前停止它们。 以前，冻结的cron作业导致部署停止。<!--MAGECLOUD-2501-->

- ![修复图标](../../assets/fix.svg)更改了`config.php`命令生成的`vendor/bin/ece-tools config:dump`文件的格式，以使用短数组语法和4空格缩进以符合Adobe Commerce编码标准。<!--MAGECLOUD-2527-->

- ![修复图标](../../assets/fix.svg)修复了当`.magento.env.yaml`包含Web配置的`{{ base_url }}`和`{{ unsecure_base_url }}`占位符而不是云基础架构项目上Adobe Commerce的默认URL配置时发生的部署错误。/<!--MAGECLOUD-2607-->

## v2002.0.13

- ![新图标](../../assets/new.svg) **启用零停机部署** — 现在，云基础架构上的Adobe Commerce在部署期间将包含所需数据库更改的请求排入队列，并在部署完成后立即应用更改。 请求最长可保留5分钟，以确保不会丢失任何会话。 查看[静态内容部署选项，以缩短Cloud](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud)上的部署停机时间。<!--MAGECLOUD-2169-->

- ![新图标](../../assets/new.svg) **云的Docker撰写** — 对[Docker设置和配置](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)进程进行了以下改进：

   - 添加了命令 — `docker:config:convert`，用于将PHP配置文件转换为Docker ENV格式以简化环境配置。 现在，将PHP配置文件复制到Docker目录，并将其转换为Docker ENV文件。 查看[Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - Adobe Commerce on cloud基础架构安装过程现在支持同时部署到只读和读写文件系统，以便更密切地模拟云文件系统。 请参阅[配置Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)。&lt;！—MAGECLOUD—2357—>

   - **Redis服务支持** — 添加了一个Redis映像，该映像部署到Docker容器并自动配置为与您的Docker安装配合使用。&lt;！—MAGECLOUD—2442—>

   - 在使用Cloud Docker [数据库容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#database-container)时，您现在具有数据库转储功能。 此外，您还可以使用[目录在主机计算机和容器之间](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container)共享文件`docker/mnt`。<!-- MAGECLOUD-2577 -->

   - **Varnish服务支持** — 添加了Varnish映像，该映像自动部署到Docker容器。 部署后，您可以按照Adobe Commerce最佳实践手动配置涂漆。 请参阅[配置和使用清漆](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish)。&lt;！—MAGECLOUD—2358—>

   - 安全站点访问 — 为访问Adobe Commerce存储和管理面板添加了SSL支持。&lt;！—MAGECLOUD—2360—>

- ![修复图标](../../assets/fix.svg) **改进了Adobe Commerce on cloud infrastructure扩展支持** — 将Adobe Commerce on cloud infrastructure [composer.json文件](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview)中guzzlehttp/guzzle包的最低版本要求降级为6.2版，以便`ece-tools`包与更多扩展兼容。<!--MAGECLOUD-2205-->

- ![新图标](../../assets/new.svg) **在构建阶段将自定义更改应用于Adobe Commerce应用程序** — 我们将该构建阶段拆分为两个单独的进程，这样您就可以在打包应用程序以进行部署之前，使用挂接将自定义更改应用于生成的静态内容。 _build :generate_进程生成代码、应用修补程序并生成静态内容。 _build:transfer_&#x200B;进程将生成的代码和静态内容传输到最终目标。 查看[应用程序挂接](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![修复图标](../../assets/fix.svg) **环境配置检查** — 改进了环境配置的验证，以便在云基础架构上构建和部署Adobe Commerce之前警告客户出现版本不兼容和配置错误。

   - 添加了特定于版本的验证，以标识不支持或弃用的环境变量和值。<!--MAGECLOUD-2183-->

   - 添加了Elasticsearch兼容性检查，以就Elasticsearch配置问题向用户发出警告。 现在，如果服务器上的Elasticsearch服务版本与Adobe Commerce不兼容，则部署将失败。 以前，即使Elasticsearch版本不兼容，部署也会成功，这会导致在站点部署后出现产品目录问题。<!--MAGECLOUD-2389-->

     您可以通过[提交支持票证](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)解决不兼容问题，以将Elasticsearch升级到兼容版本，或更改Adobe Commerce配置以指定兼容版本的Elasticsearch PHP客户端。

      - 对于Adobe Commerce版本2.1.x到2.2.2，请将Elasticsearch升级到版本2.4。

      - 对于Adobe Commerce版本2.2.3及更高版本，请将Elasticsearch升级到版本5.2。

      - 如果您有Elasticsearch 1.x或2.x，并且不想升级，请将composer.json中的Adobe Commerce Elasticsearch PHP客户端版本要求更新为`"elasticsearch/elasticsearch": "~2.0"`。

   - 改进了环境变量验证，以识别在生成、部署和部署后阶段可能会导致冲突的配置设置。 例如，如果静态内容部署的全局设置与生成或部署阶段的设置冲突，则在安装和升级过程中会显示警告消息。<!--MAGECLOUD-2156-->

- ![修复图标](../../assets/fix.svg) **环境变量更新** — 已更改以下环境变量：

   - **[SKIP_HTML_MINIFICATION全局变量](../environment/variables-global.md#skip_html_minification)** — 将默认值更改为`true`以启用随选HTML内容缩小，这可以最大限度地减少部署到暂存和生产环境时的停机时间。 零停机部署需要此配置。<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES部署变量](../environment/variables-deploy.md#clean_static_files)** — 添加了基于CLEAN_STATIC_FILES环境变量设置来管理生成阶段生成的静态内容的清理静态文件处理的功能。 以前，始终清理在生成阶段生成的静态内容文件。<!--MAGECLOUD-1506-->

- ![修复图标](../../assets/fix.svg) **日志记录** — 进行了以下更改以改进日志消息并减少日志大小：

   - 部署失败日志条目现在包括导致失败的操作的命令输出，即使您的环境配置未指定调试级别日志记录也是如此。 查看[`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - 添加了针对某些扩展所需的生成工厂无法正确生成时发生的部署失败的日志记录，因为文件系统处于只读状态。<!--MAGECLOUD-2209-->

   - 减少了部署日志大小，并修复了使用交互式进度条的安装命令导致的格式问题。<!--MAGECLOUD-2402-->

   - 消除了不必要的详细程度并更新了一些日志语句的优先级。<!--MAGECLOUD-2227-->

- ![修复图标](../../assets/fix.svg) **特定于Cron的修复**—

   - 已将历史记录生命周期的默认cron作业配置设置从3d （4320分钟）更改为1h （60分钟），以防止在cron队列填充过快时可能出现性能问题和部署失败。<!--MAGECLOUD-2427-->

   - 改进了部署阶段的cron作业管理过程，以防止数据库锁定和其他严重问题。 现在，所有cron作业在部署阶段停止，并在部署完成后重新启动。<!--MAGECLOUD-2445-->

   - 修复了Adobe Commerce 2.2.0及更高版本中cron作业为计划使用者而启动的锁定机制的问题，以防止cron作业启动重复的使用者。<!--MAGECLOUD-2464-->

- ![修复图标](../../assets/fix.svg)修复了在部署过程中引用压缩文件时导致[和](../environment/variables-intro.md)错误的`gzip`静态内容压缩进程`not overwritten` (`no such file or directory`)问题。<!-- MAGECLOUD-2182-->

- ![修复图标](../../assets/fix.svg)修复了在转储过程中，如果未指定存储区域设置，则`php ./vendor/bin/ece-tools config:dump`命令无法从`config.php`文件中删除冗余部分的问题。 现在，您可以轻松地在环境之间移动配置文件。 更新到`ece-tools` v2002.0.13后，使用改进的`config.php`命令重新生成较旧的`config:dump`文件。 查看商店设置的[配置管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings)。<!--MAGECLOUD-2444-->

- ![修复图标](../../assets/fix.svg)修复了在`.magento/routes.yaml`文件中的路由配置从[apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/)域重定向到`www`域时导致部署阶段错误的问题。<!--MAGECLOUD-2556-->

- ![修复图标](../../assets/fix.svg)修复了`_merge`变量的[`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration)选项的问题，如果更新的`engine`配置文件中未包含`.magento.env.yaml`参数，该问题会导致合并结果不正确。 现在，合并操作仅正确覆盖您在更新的`.magento.env.yaml`中指定的值，而无需您设置`engine`参数。<!--MAGECLOUD-2520-->

- ![修复图标](../../assets/fix.svg)修复了Redis配置问题，该问题导致在云基础架构版本2.2.1及更高版本上为Adobe Commerce错误地启用会话锁定，从而可能导致性能变慢和超时。 现在，默认情况下禁用会话锁定。 此问题是由于Redis会话处理程序包v1.3.4中引入的`disable_locking`参数的默认行为更改所致。 请参阅[colimollenhour/php-redis-session-abstract包](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![新图标](../../assets/new.svg) **云的Docker撰写** — 添加了命令 — `docker:build` — 以从云[存储库生成](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)Docker撰写`ece-tools`配置。<!-- MAGECLOUD-2250 -->

- ![新图标](../../assets/new.svg) **更改区域设置** — 现在无需导出和导入配置过程即可更改存储区域设置。 当应用程序处于生产状态并启用SCD_ON_DEMAND时，存储和管理区域设置选项可用。<!-- MAGECLOUD-2019 -->

- ![新图标](../../assets/new.svg) <!-- MAGECLOU-1998 -->**站点地图和Robots** — 创建了[工作流](../store/robots-sitemap.md)以添加`robots.txt`文件并为单个域配置生成`sitemap.xml`文件，而无需更改基础结构。

- ![新图标](../../assets/new.svg) **向导** — 添加了两个[向导](../deploy/smart-wizards.md)以帮助您进行云配置：<!-- MAGECLOUD-1910 -->

   - `ideal-state` — 配置理想状态以将部署停机时间降至最低

   - `master-slave` — 为数据库和Redis配置负载平衡

- ![新图标](../../assets/new.svg) **模块刷新** — 添加了Cloud命令 — `module:refresh` — 以启用已禁用或未显式启用的模块，类似于在生成期间自动启用的方式。<!-- MAGECLOUD-1521 -->

- ![新图标](../../assets/new.svg)添加了使用`_merge`CACHE[、](../environment/variables-deploy.md#cache_configuration)SESSION[、](../environment/variables-deploy.md#session_configuration)QUEUE[和](../environment/variables-deploy.md#queue_configuration)SEARCH[配置中的](../environment/variables-deploy.md#search_configuration)选项为服务选择合并或覆盖配置的功能。<!-- MAGECLOUD-2105 -->

- ![新图标](../../assets/new.svg) **环境配置示例文件** — 我们已将一个`.magento.env.yaml`示例文件添加到ECE-Tools包中，其中包含每个环境变量的详细说明和可能值。<!-- MAGECLOUD-1908 -->

   - 我们还为`.magento.env.yaml`配置添加了深层验证，以防止部署过程中因意外值而导致失败。 失败时，您现在会收到一条详细的错误消息，其开头为：`Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![新图标](../../assets/new.svg)已添加以下&#x200B;[**环境变量**](../environment/variables-intro.md)：

   - 现在，您可以使用新的[SCD_MATRIX](../environment/variables-deploy.md#scd_matrix)环境变量为每个主题定义多个区域设置，这会减少要部署的主题文件数量。<!-- MAGECLOUD-1501 -->

   - 添加了[DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration)环境变量以自定义部署的数据库连接。<!-- MAGECLOUD-2047 -->

   - 新的[MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level)变量覆盖所有输出流的最低日志记录级别，而不更改代码。<!-- MAGECLOUD-2129 -->

- ![修复图标](../../assets/fix.svg)修复了导致部署和部署后阶段之间出现停机时间的问题。 现在，部署后阶段在部署阶段结束后&#x200B;_立即_&#x200B;开始。

- ![修复图标](../../assets/fix.svg)修复了未从计划清除成功的cron作业（具有`status = success`的作业）的问题。<!-- MAGECLOUD-2268 -->

- ![修复图标](../../assets/fix.svg)修复了在部署阶段而非项目的部署后阶段清除缓存的`post_deploy`挂接的问题。<!-- MAGECLOUD-2113 -->

- ![修复图标](../../assets/fix.svg)修复了在使用具有多个区域设置的SCD时的问题，这会为每个区域设置生成相同的`js-translation.json`文件。<!-- MAGECLOUD-2034 -->

- ![修复图标](../../assets/fix.svg)已优化`db:dump`包中的`ece-tools`命令，以避免锁定表并提高速度。<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>ECE-Tools版本2002.0.11是2.2.4兼容性的必要条件。

- ![新图标](../../assets/new.svg) **配置到非主节点的只读连接** — 此版本添加了配置到非主节点的只读连接以接收只读通信量的功能（对于[MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)）。<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection)和<!--MAGECLOUD-143 -->

- ![新建图标](../../assets/new.svg) **配置向导** — 添加了一个向导以帮助验证您的静态内容部署配置。 查看[智能向导](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![新图标](../../assets/new.svg) **Symfony控制台支持** — 已添加对具有Adobe Commerce 2.3的Symfony控制台4的支持。<!-- MAGECLOUD-1966 -->

- ![修复图标](../../assets/fix.svg) **Cron计划优化** — 改进了队列管理并增强了日志记录以帮助调试cron相关问题。<!-- MAGECLOUD-1607 -->

- 如果![或](../../assets/fix.svg)值与现有管理员帐户相同，则`ADMIN_EMAIL`修复图标`ADMIN_USERNAME`部署验证失败。<!-- MAGECLOUD-1221 -->

- ![修复图标](../../assets/fix.svg)删除了2.2.x版本的SOLR支持。 2.1.x版本保留启用SOLR.<!-- MAGECLOUD-1282 -->的功能

- ![修复图标](../../assets/fix.svg) PRO项目的暂存和生产环境的首次安装现在包含Elasticsearch的不同索引前缀，以防止在识别属于每个环境的记录时可能发生冲突。<!-- MAGECLOUD-1489 -->

- ![修复图标](../../assets/fix.svg)修复了在静态内容部署期间中断旧体系结构的生成阶段的问题。<!-- MAGECLOUD-2021 -->

- ![修复图标](../../assets/fix.svg) **特定于Cron的改进** — 已重新处理cron实施：<!-- MAGECLOUD-1607 -->

   - 修复了导致cron队列快速填充的问题。 如今，它以更可靠的方式清理了过时的cron工作。

   - 重新组织了cron作业序列，以便单独线程中的所有作业在常规组之前启动。

   - 改进了日志记录，以更好地协助调试cron问题。

   - **注意** — 此版本解决了许多与cron相关的问题。 如果您当前在&#x200B;_m2修补程序_&#x200B;中使用一些与cron相关的修补程序，请删除它们。

- ![修复图标](../../assets/fix.svg) **特定于SCD的改进**—

   - 您可以在`VERBOSE_COMMANDS`生成`SCD_COMPRESSION_LEVEL`和de_ploy阶段&#x200B;_期间使用_&#x200B;和<!-- MAGECLOUD-1819 -->环境变量。

   - 修复了在遇到`SCD_COMPRESSION_LEVEL`环境变量的意外值时导致部署失败并出现随机错误的问题。 改进了配置验证以提供有意义的通知。 有关可接受的值，请参阅[`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level)。<!-- MAGECLOUD-2043 -->

   - 修复了`SCD_COMPRESSION_LEVEL`环境变量配置流的行为，以使覆盖按预期工作。<!-- MAGECLOUD-2044 -->

   - 修复了导致无法在`SCD_THREADS`文件`.magento.env.yaml`部署&#x200B;_阶段中配置_&#x200B;环境变量的问题。<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![新图标](../../assets/new.svg) **静态内容部署(SCD)** — 有一个新的替代部署过程可在请求时生成静态内容（按需）。 这通过生成最关键的资产来减少停机时间并改进缓存处理。<!-- MAGECLOUD-1285 -->

   - **新环境变量** — 已添加`SCD_ON_DEMAND`全局环境变量，以便在请求时生成静态内容。<!-- MAGECLOUD-1738 -->

   - **部署后挂接** — 为`post_deploy`文件添加了`.magento.app.yaml`挂接，该挂接清除缓存并在&#x200B;_容器开始接受连接后_&#x200B;预载（加温）缓存。 它仅适用于在[!DNL Cloud Console]中包含暂存和生产环境的Pro项目以及入门项目。 尽管不是必需的，但是它与`SCD_ON_DEMAND`环境变量协同工作。<!-- MAGECLOUD-1788 -->

- ![新图标](../../assets/new.svg) **优化** — 已优化部署期间移动或复制文件以提高部署速度并降低文件系统的负载。<!-- MAGECLOUD-1842 -->

- ![新图标](../../assets/new.svg) **部署日志记录** — 添加了启用Syslog和Graylog扩展日志格式(GELF)处理程序以便在部署过程中输出日志的功能。 查看[日志记录处理程序](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![新图标](../../assets/new.svg)已添加以下&#x200B;[**环境变量**](../environment/variables-intro.md)：

   - `CRYPT_KEY` — 移动数据库时向另一个环境提供密钥。<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Global_&#x200B;环境变量，该变量跳过复制`var/view_preprocessed`目录中的静态视图文件并在请求时生成缩小的HTML。<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_全局_&#x200B;环境变量，以便在请求时生成静态内容。<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` — 您可以列出要用于预加载缓存的页面。 在新的[部署后变量](../environment/variables-post-deploy.md)中可用。

- ![修复图标](../../assets/fix.svg)修复了本地应用的修补程序中断实例上的部署的问题。 现在，ECE-Tools可以检测到已应用了修补程序。<!-- MAGECLOUD-982 -->

- ![修复图标](../../assets/fix.svg)修复了JavaScript捆绑包与GZIP功能之间的冲突。 现在，这些功能可以正常配合使用。<!-- MAGECLOUD-1735 -->

- ![修复图标](../../assets/fix.svg)修复了在使用早期PHP 7.0.x版本时导致ECE-Tools CLI命令失败的问题。<!-- MAGECLOUD-1744 -->

- ![修复图标](../../assets/fix.svg)修复了阻止在多个线程中使用压缩策略的静态内容部署的问题。<!-- MAGECLOUD-1822 -->

- ![修复图标](../../assets/fix.svg)修复了导致管理员登录延迟的Redis会话锁定问题。 此外，此修复程序还可用于2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>您必须[升级Adobe Commerce on cloud infrastructure中继](../dev-tools/install-package.md#update-the-metapackage)以获取此更新以及所有未来更新。

- ![新图标](../../assets/new.svg) **ece-tools**— `ece-tools`包现在支持Adobe Commerce 2.1.x。<!-- MAGECLOUD-1086 -->

- ![新图标](../../assets/new.svg) **Redis配置** — 您现在可以使用环境变量[配置Redis](../environment/variables-deploy.md#cache_configuration)页面以及默认缓存和Redis会话存储。<!-- MAGECLOUD-1552 -->

- ![新图标](../../assets/new.svg) **搜索、AMQP和Redis服务改进** — 我们统一了服务配置流程，以便所有服务的行为现在都相同。 不再支持手动编辑`env.php`文件来配置服务。 您必须改用环境变量或`.magento.env.yaml`文件。<!-- MAGECLOUD-1437 -->

- ![修复图标](../../assets/fix.svg) **环境变量**—

   - `env:STATIC_CONTENT_THREADS`的使用已被弃用，将在未来版本中删除。 请改用[SCD_THREADS](../environment/variables-deploy.md#scd_threads)。<!-- MAGECLOUD-1507 -->

   - 已弃用`STATIC_CONTENT_EXCLUDE_THEMES`环境变量。 必须改用`SCD_EXCLUDE_THEMES`环境变量。<!-- MAGECLOUD-1640 -->

- ![修复图标](../../assets/fix.svg) **日志记录** — 我们简化了有关内置修补操作的日志记录。<!-- MAGECLOUD-1674 -->

- ![修复图标](../../assets/fix.svg)我们删除了`developer`模式支持和`APPLICATION_MODE`环境变量，因为它们导致了意外行为。<!-- MAGECLOUD-1615 -->

- ![修复图标](../../assets/fix.svg)我们修复了导致与Redis相关的静态内容部署失败的问题。 现在，多线程静态内容部署按设计运行。<!-- MAGECLOUD-1630 -->

- ![修复图标](../../assets/fix.svg)我们修复了导致用户无法在管理员中保存对配置字段的修改的问题，在运行`app:config:dump`命令后，这些字段标记为敏感。<!-- MAGECLOUD-1175 -->

- ![修复图标](../../assets/fix.svg)我们添加了对早期版本的`symfony/yaml`的支持以修复与某些包之间的冲突，这些包尚未与最新版本兼容。<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>我们在此版本中将`vendor/magento/ece-patches`与`vendor/magento/ece-tools`合并。 您不再需要单独更新`vendor/magento/ece-patches`包。

**新功能：**

- **已改进日志记录**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - 我们改进了日志消息，以便在构建或部署过程覆盖环境变量时提供更好的解释。
   - 您现在可以实时查看安装和升级进度。 跟踪`install_update.log`文件以查看进度。 例如，

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **新cron命令** — 您现在可以解锁特定的cron卡作业，而不是使用[`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451)命令停止并重新启动所有此类作业。 在2.1.<!-- MAGECLOUD-1367 -->中不可用

- **统一配置文件** — 您现在可以使用[`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml)文件配置生成和部署阶段。<!-- MAGECLOUD-1369 -->

- **备份配置文件** — 部署过程现在会在部署后自动创建`app/etc/env.php`和`app/etc/config.php`配置文件的备份。 我们还添加了[新CLI命令](https://support.magento.com/hc/en-us/articles/360033182871)以从备份中还原这些配置文件。<!-- MAGECLOUD-1372 -->

- **验证错误疑难解答** — 我们更改了当`config.php`不包含足够的数据以进行静态内容部署时，必须用来解决验证错误的命令。 以前，错误消息指示您运行`bin/magento app:config:dump`。 现在，您必须运行`php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **新环境变量** — 您现在可以使用环境变量将自定义[搜索](../environment/variables-deploy.md#search_configuration)和基于[AMQP的](../environment/variables-deploy.md#queue_configuration)服务连接到您的站点。<!-- MAGECLOUD-1410 -->

- 我们实施了智能修补。 现在，该包不基于Adobe Commerce在云基础架构版本上应用修补程序，而是基于修补的包版本应用修补程序。<!--MAGECLOUD-1090-->

**已解决的问题：**

- 我们修复了导致生成错误的日志记录问题。<!-- MAGECLOUD-1162 -->

- 我们修复了在交互模式下运行部署时导致超时异常的问题。<!-- MAGECLOUD-1389 -->

- 我们修复了在使用压缩策略生成静态内容时导致错误的问题。 在2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->中不可用

- 我们修复了导致部署脚本无法正确识别暂存环境和生产环境的问题。<!-- MAGECLOUD-1493 -->

- 我们修复了在安装和升级过程中导致网络问题中断数据库连接和导致失败的问题。<!-- MAGECLOUD-1520 -->

- 我们修复了阻止您多次使用`app:config:dump`导出配置文件的问题。 在2.1.<!--  MAGECLOUD-1567  -->中不可用

- 我们修复了导致&#x200B;_管理员_&#x200B;登录延迟的Redis会话&#x200B;_锁定_&#x200B;问题。 在2.1.<!--  MAGECLOUD-1582  -->中不可用

- 我们修复了与版本控制相关的实施问题，该问题会导致与其他基于编辑器的修补模块发生冲突。<!-- MAGECLOUD-1450 -->

- 我们修复了在导入期间导致PHP内存问题的问题。<!-- MAGECLOUD-1310 -->

- 已删除修补程序；修复了`colinmollenhour/credis` v1.6中的错误，以便在云基础架构2.2.1上启用对Adobe Commerce的支持。在2.1.<!-- MAGECLOUD-1033 -->中不可用

## v2002.0.7

**已解决的问题：**

- 我们删除了`var/view_preprocessed`符号链接以修复导致JavaScript缩小冲突的问题。<!-- MAGECLOUD-1454 -->

## v2002.0.6

**已解决的问题：**

- 我们修复了在文件或目录名称包含空格时导致`gzip`错误的问题。<!-- MAGECLOUD-1413 -->

- 我们修复了阻止部署脚本正确识别和启用模块依赖项的问题。<!-- MAGECLOUD-1424 -->

## v2002.0.5

**新功能：**

- **使用环境变量配置cron使用者** — 您现在可以使用新的`CRON_CONSUMERS_RUNNER`环境变量配置cron使用者。

- **配置扫描** — 我们现在在构建/部署过程中扫描关键组件，如果扫描失败，则停止该过程，这样可以防止由于站点处于维护模式而造成的不必要的停机。

- **生成/部署通知** — 我们添加了一个配置文件，您可以使用它[设置Slack和/或电子邮件通知](../environment/set-up-notifications.md)，以便在所有环境中执行生成/部署操作。

- **静态内容压缩** — 我们现在在生成和部署阶段使用[gzip](https://www.gnu.org/software/gzip/)压缩静态内容。 此压缩与Fastly压缩相结合，有助于减小存储的大小并提高部署速度。 如有必要，您可以使用[生成选项](../environment/variables-build.md)或[部署变量](../environment/variables-deploy.md)来禁用压缩。 有关更多信息，请参阅以下主题：

   - [应用程序环境变量](../application/variables-property.md)

   - [静态内容部署性能](../deploy/static-content.md)

   - [部署进程](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **配置管理** — 我们现在会在构建阶段在您的Git存储库中自动生成一个`app/etc/config.php`文件（如果尚不存在）。 自动生成的文件仅包含模块和扩展名的列表。 如果文件已存在，则构建阶段将继续正常进行。 如果稍后执行[配置管理](../store/store-settings.md)，则命令会更新文件，而无需其他步骤。 有关详细信息，请参阅[部署进程](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)。

- **数据库转储** — 我们添加了一个`magento/ece-tools` CLI命令，用于在所有环境中创建数据库转储。 对于Pro Plan生产环境，此命令仅从三个高可用性节点中的一个转储，因此转储期间写入其他节点的生产数据可能不会被复制。 我们建议先将应用程序置于维护模式，然后再在生产环境中执行数据库转储。 有关详细信息，请参阅[备份管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots)。

- **已取消Cron间隔限制** — 在us-3、eu-3和ap-3区域中配置的所有环境的默认Cron间隔为1分钟。 对于专业集成环境，所有其他区域的默认cron间隔为5分钟；对于专业暂存和生产环境，默认间隔为1分钟。 要修改现有cron作业，请在`.magento.app.yaml`中编辑您的设置，或者为生产/暂存环境创建支持票证。 有关详细信息，请参阅[设置cron作业](../application/crons-property.md#set-up-cron-jobs)。

**已解决的问题：**

- 我们修复了由于部署进程在静态内容部署之前调用`cache-clean`操作而导致部署时间较长的问题。<!-- MAGECLOUD-1327 -->

- 我们修复了在生产环境中部署的静态内容生成步骤期间导致错误的问题。<!-- MAGECLOUD-1322 -->

- 我们修复了某些`magento/ece-tools`命令无法将输出记录到`stderr`的问题。<!-- MAGECLOUD-1264 -->

- 我们修复了无法在分支中更新`env.php`中的基本URL值的问题。<!-- MAGECLOUD-1242 -->

- 我们修复了导致`magento setup:install`命令添加不安全的前缀(`http://`)来保护基本URL的问题。<!-- MAGECLOUD-1171 -->

- 我们修复了阻止修补程序错误导致部署失败的问题。<!-- MAGECLOUD-1170 -->

- 我们修复了导致`ece-tools`无法停止执行并在无法应用修补程序时引发异常的问题。<!-- MAGECLOUD-1152 -->

- 我们修复了在管理员中启用HTML缩小后加载店面时导致错误的问题。<!-- MAGECLOUD-1138 -->

## v2002.0.4

**已解决的问题：**

- 您现在可以通过SSH访问，在所有环境中使用CLI命令[手动重置卡住cron作业](https://support.magento.com/hc/en-us/articles/360033099451)。 部署过程自动重置cron作业。<!-- MAGECLOUD-1355 -->

## v2002.0.3

**已解决的问题：**

- 我们修复了由于Redis读取/写入时间过长而导致页面超时的问题。 您现在可以在Redis配置中使用`disable_locking`参数以防止出现此问题。<!-- MAGECLOUD-1311 -->

## v2002.0.2

**已解决的问题：**

- [!DNL RabbitMQ]配置进程现在将自动获取所有必需的参数。<!-- MAGECLOUD-1246 -->

## v2002.0.1

**新功能：**

- 云基础架构上的Adobe Commerce现在支持范围和[静态内容部署策略](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy)。 我们为静态内容部署策略添加了默认设置为`–s`的`quick`参数。 您可以使用环境变量[SCD_STRATEGY](../environment/variables-deploy.md)自定义这些策略，并将其用于生成和部署操作。 此变量支持选项`standard`、`quick`或`compact`。 如果您选择`compact`，我们将使用`STATIC_CONTENT_THREADS`覆盖`1`值，这可能会降低部署速度，尤其是在生产环境中。 在2.1.<!--- MAGECLOUD-1057 -->中不可用

- 我们在环境中创建了一个日志文件，用于捕获和编译生成和部署操作。 `var/log/cloud.log`文件位于根应用程序目录中。<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**已解决的问题：**

- 已重构`ece-tools`包，使其与Adobe Commerce在云基础架构2.2.0及更高版本上兼容。<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- 我们修复了导致`ece-tools`无法停止执行并在无法应用修补程序时引发异常的问题。<!-- MAGECLOUD-1186 -->

- 我们修复了在生成期间跳过依赖项注入(di)编译时导致引发异常的问题。<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- 我们修复了导致部署过程覆盖`env.php`文件中的自定义Redis配置的问题。<!-- MAGECLOUD-1019 -->

- 我们修复了导致重定向循环的问题，该问题导致默认安全管理员禁用重定向循环。<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>此包不再与云基础架构上的其他Adobe Commerce版本兼容，不应使用&#x200B;****。

### 初始版本

适用于Adobe Commerce on cloud infrastructure 2.2.0的`ece-tools`的初始版本。
