---
title: 适用于Commerce的云组件
description: 请参阅云组件包的最新改进列表。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: b90959335c91dd0631d270ebb522524cf1db6ff0
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 适用于Commerce的云组件

[Cloud Components](https://github.com/magento/magento-cloud-components)包为部署在Cloud Infrastructure上的站点提供扩展的Adobe Commerce核心功能。 此软件包是对ECE-Tools软件包的依赖项。 这些发行说明介绍了此包的最新改进，此包是[Cloud Tools Suite for Commerce](cloud-tools-suite.md)的组件。

`magento/magento-cloud-components`包使用以下版本序列： `<major>.<minor>.<patch>`

发行说明包括：

- ![新图标](../../assets/new.svg)新功能
- ![修复图标](../../assets/fix.svg)修复和改进

<!--Add release notes below-->

## v1.1.3 {#latest}

发行日期： 2025年8月7日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 已添加PHP 8.4和修复的功能测试。<!-- MCLOUD-13313 -->

## v1.1.2

发布日期： 2025年6月3日

- ![修复图标](../../assets/fix.svg) **改进与2.4.8的兼容性** — 更新了第三方库以更好地与2.4.8<!-- MCLOUD-13707	 - -->兼容

## v1.1.1

发行日期： 2025年2月6日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 添加了对PHP 8.4.<!-- MCLOUD-13148	 - -->的支持
- ![修复图标](../../assets/fix.svg) **修复缓存预热问题** — 修复了缓存预热期间类别URL的问题。<!-- MCLOUD-12454 - -->


## v1.1.0

发行日期： 2024年10月7日

- ![修复图标](../../assets/fix.svg) **重构的代码** — 删除了对旧的PHP版本7.4、7.3、7.2和相关库的支持。<!-- MCLOUD-9278 - -->
- ![修复图标](../../assets/fix.svg) **升级了单色版本** — 添加了对单色版本3.6.<!-- MCLOUD-12855 - -->的支持

## v1.0.14

发行日期： 2024年4月8日

- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.3的支持。

## v1.0.13

发行日期： 2023年3月10日

- ![新图标](../../assets/new.svg) **对PHP 8.2**&#x200B;的增强支持 — 修复了某些PHP 8.2.x版本存在的兼容性问题，以支持Commerce 2.4.6。

## v1.0.12

发行日期： 2022年9月13日

- ![修复图标](../../assets/fix.svg) **预热错误** — 修复了当管理员中的页面可见性设置为[不可单独显示](../environment/variables-post-deploy.md#warm_up_pages)[**时尝试**&#x200B;预热的](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure)问题，该问题导致部署日志中出现`ERROR: Warming up failed: <link to page>`错误。<!-- MCLOUD-9134 -->

## v1.0.11

发行日期： 2022年8月4日

- ![修复图标](../../assets/fix.svg) **添加了对Symfony 5.4兼容性的支持** — 修复了与Symfony 5.4兼容性的问题。<!-- AC-3550 -->

## v1.0.10

发行日期： 2022年3月10日

- ![新图标](../../assets/new.svg) **支持PHP 8.1** — 添加了对PHP 8.1的支持并放弃对PHP 7.1的支持。

## v1.0.9

发行日期： 2021年10月25日

- ![修复图标](../../assets/fix.svg) **更新单一日志** — 已将`monolog`包所需的最低版本更新为`^2.3`。<!-- ACMP-1263 -->

## v1.0.8

发行日期： 2021年7月29日

- ![修复图标](../../assets/fix.svg) **从自动生成的URL中删除尾随斜杠** — 从缓存预热期间生成的类别页面URL中删除尾随斜杠。<!--MCLOUD-7192-->

## v1.0.7

发行日期： 2020年9月9日

- ![新图标](../../assets/new.svg) **日志记录改进** — 减小`cache.log`文件的大小以提高性能。<!--MCLOUD-6859-->

- ![修复图标](../../assets/fix.svg)修复了缓存配置值中导致`php bin/magento cache:evict` CLI命令失败的类型错误。

## v1.0.6

发行日期： 2020年8月5日

- ![新图标](../../assets/new.svg) **提高Redis性能** — 添加了用于删除过期的Redis密钥的`./bin/magento cache:evict`命令，该命令可减少Redis内存使用率以提高性能。<!--MCLOUD-6023-->

- ![修复图标](../../assets/fix.svg)为修复性能问题，删除了上下文&#x200B;*中* New Relic日志的支持。<!--MCLOUD-6422-->

## v1.0.5

发布日期： 2020年6月25日

- ![修复图标](../../assets/fix.svg)修复了magento/magento-cloud-components版本1.0.4中引入的问题，该问题导致刷新缓存操作在部署阶段失败，中断部署过程。

## v1.0.4

发布日期： 2020年6月25日

- ![新图标](../../assets/new.svg) **在上下文中实施了New Relic日志** —Adobe Commerce生成的应用程序日志现在显示在New Relic的跟踪中，以改善故障排除功能。<!--MCLOUD-6029-->

- ![新图标](../../assets/new.svg) **已改进日志记录** — 已添加日志记录以跟踪缓存失效和完全重新索引事件。<!--MCLOUD-6157-->

## v1.0.3

发行日期： 2020年2月27日

- ![修复图标](../../assets/fix.svg)修复了兼容性问题，以支持使用旧PHP版本的`ece-tools` 2002.0.x发行版。

## v1.0.2

发行日期： 2020年2月6日

- ![新图标](../../assets/new.svg)扩展了`WARM_UP_PAGES`环境变量的功能，以支持特定产品页面的缓存预加载。 有关详细的功能说明，请参阅[部署后变量](../environment/variables-post-deploy.md#warm_up_pages)主题。<!--MAGECLOUD-4444-->

- ![修复图标](../../assets/fix.svg)修复了在使用`WARM_UP_PAGES`功能填充缓存时，无效的存储URL导致部署后挂接失败的问题。 仅在URL重写被禁用时才会发生此问题。<!-- MAGECLOUD-4094 -->

## v1.0.1

发行日期： 2019年7月23日

- ![修复图标](../../assets/fix.svg)修复了影响使用默认存储URL的&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;功能的问题。 现在，如果`config:show:default-url`命令无法获取基本URL，则使用MAGENTO_CLOUD_ROUTES变量中的URL。<!-- MAGECLOUD-3866 -->

## v1.0.0

发布日期： 2019年6月12日

这是[`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components)包的第一个版本，它是`ece-tools`包版本2002.0.20及更高版本的新依赖项。

- ![new icon](../../assets/new.svg)添加了使用正则表达式模式来配置&#x200B;**WARM_UP_PAGES**&#x200B;环境变量的功能，以便缓存单个页面、多个域和多个页面。 查看[部署后变量](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
