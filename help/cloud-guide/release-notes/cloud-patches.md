---
title: Commerce的云修补程序
description: 请参阅云修补程序包的最新改进列表。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: a4933ed231f22becd9b0188c6c51f1e5ae5384eb
workflow-type: tm+mt
source-wordcount: '2543'
ht-degree: 0%

---

# Commerce的云修补程序

[云修补程序](https://github.com/magento/magento-cloud-patches)包提供了一组必需的修补程序，这些修补程序改进了所有Adobe Commerce版本与云环境的集成，并支持快速交付关键修补程序。

Commerce的云修补程序软件包依赖于ECE-Tools软件包，并在安装或更新ECE-Tools软件包时安装和更新。 您还可以使用和管理Commerce的Cloud Patches作为独立软件包，将修补程序应用到不在Cloud平台上的Adobe Commerce项目。 以下发行说明介绍了此包的最新改进。

>[!TIP]
>
>为确保您的项目具有所有必需的修补程序，请更新到[最新版本的ece-tools](../dev-tools/update-package.md)。

>[!NOTE]
>
>有关将修补程序应用到项目的说明，请参阅[应用修补程序](../development/apply-patches.md)。

`magento/magento-cloud-patches`包使用以下版本序列： `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.12 {#latest}

发行日期： 2025年11月13日

- ![修复图标](../../assets/fix.svg) **Symfony包** — 已添加对最新Symfony YAML包的支持。<!-- MCLOUD-14020 -->
- ![修复图标](../../assets/fix.svg) **修补程序** — 启用JS缩小和捆绑时，[签出的修复失败](https://experienceleague.adobe.com/zh-hans/docs/experience-cloud-kcs/kbarticles/ka-27997)问题，如&#x200B;*Commerce知识库*&#x200B;中所述。
- ![修复图标](../../assets/fix.svg) **改进的类别视图** - MCLOUD-13752：改进类别视图。<!-- MCLOUD-13752 | MCLOUD-14139  -->

## v1.1.11

发行日期： 2025年9月9日

- 适用于CVE-2025-54236的![修复图标](../../assets/fix.svg) **WebAPI**-Fix。<!-- MCLOUD-14016 -->

## v1.1.10

发行日期： 2025年8月7日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 已添加功能测试。<!-- MCLOUD-13312 -->

## v1.1.9

发布日期： 2025年6月9日

- ![修复图标](../../assets/fix.svg) **改进的类别视图** — 改进类别视图。<!-- MCLOUD-13752	 - -->
- ![修复图标](../../assets/fix.svg) **已改进管理缓存**-Improved-Admin-cache-efficiency CVE-2025-47110。<!-- MCLOUD-13753	 - -->

## v1.1.8

发布日期： 2025年6月3日

- ![修复图标](../../assets/fix.svg) **改进与2.4.8的兼容性** — 更新了第三方库以更好地与2.4.8<!-- MCLOUD-13707	 - -->兼容

## v1.1.7

发行日期： 2025年5月5日

- ![新图标](../../assets/new.svg) **已将Commerce 2.4.4的修补程序更新为2.4.8** — 这是在1.1.7[中发布的](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch)CVE-2025-24434<!-- MCLOUD-13619 -->的更新修补程序

## v1.1.6

发行日期： 2025年4月24日

- ![新图标](../../assets/new.svg) **已将Commerce 2.4.4的修补程序更新为2.4.7** — 这是在1.1.4[中发布的](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)CVE-2025-24434<!-- MCLOUD-13240 -->的更新修补程序

## v1.1.5

发行日期： 2025年4月15日

- ![新图标](../../assets/new.svg) **已添加B2B 1.5.2**&#x200B;的修补程序 — 修复了ACP2E-3833与B2B模块1.5.2和MariaDB 10.6<!-- MCLOUD-13605	-->的问题

## v1.1.4

发行日期： 2025年2月13日

- ![新图标](../../assets/new.svg) **已添加Commerce 2.4.4到2.4.7**&#x200B;的修补程序 — 此更新修补程序[CVE-2025-24434](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)。<!-- MCLOUD-13240	 - -->

## v1.1.3

发行日期： 2025年2月6日

- ![新图标](../../assets/new.svg) **PHP 8.4** — 添加了对PHP 8.4.<!-- MCLOUD-13149	 - -->的支持

## v1.1.2

发行日期： 2024年11月5日

- ![修复图标](../../assets/fix.svg) **已添加Commerce 2.4.4到2.4.7**&#x200B;的修补程序 — 此更新修复了在使用B2B模块时Adobe Commerce存在的严重[CVE-2024-45115](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73)漏洞。<!-- MCLOUD-12980 - -->

## v1.1.1

发行日期： 2024年11月5日

- ![修复图标](../../assets/fix.svg) **已添加Commerce 2.4.4到2.4.7**&#x200B;的修补程序 — 此更新修补了严重的[CVE-2024-34102](https://experienceleague.adobe.com/zh-hans/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting漏洞。<!-- MCLOUD-12980 - -->

## v1.1.0

发行日期： 2024年10月7日

- ![修复图标](../../assets/fix.svg) **重构的代码** — 删除了对旧PHP版本(7.4、7.3、7.2)和相关库的支持。<!-- MCLOUD-9278 - -->
- ![修复图标](../../assets/fix.svg) **升级了单色版本** — 添加了对单色版本3.6.<!-- MCLOUD-12855 - -->的支持
- ![修复图标](../../assets/fix.svg) **Patch for Application Server** — 解决GraphQL Application Server的已知问题。 具体而言，版本2.4.7中的`CatalogGraphQl\\Model\\Config\\AttributeReader`包含错误，该错误可能会导致GraphQL请求根据过时的属性配置检索响应。<!-- ACPT-1876 -->

## v1.0.27

发行日期： 2024年5月21日

- **支持PHP 8.3** — 此修补程序解决了PHP 8.3与编辑器包版本之间的兼容性错误。

## v1.0.26

发行日期： 2024年4月8日

- ![新图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.3的支持。

## v1.0.25

发行日期： 2024年1月16日

- **缓存改进** — 此修补程序增强了Adobe Commerce版本2.4.4及更高版本的布局缓存效率，从而显着减少了内存使用量。<!-- MCLOUD-11514 -->
- **CRON作业改进** — 此修补程序修复了以下问题：丢失的作业不必要地等待Adobe Commerce版本2.4.4及更高版本的CRON作业锁定。<!-- MCLOUD-11329 -->

## v1.0.24

发行日期： 2023年9月15日

- **性能改进** — 此修补程序通过将Adobe Commerce 2.4.6的相同部署配置加载次数减少到2.4.6-p1<!-- MCLOUD-10604 -->，修复了一个影响性能的问题

## v1.0.23

发行日期： 2023年7月31日

- **已删除修补程序MCLOUD-10604** — 此修补程序已移至QPT。<!-- MCLOUD-10736 -->

## v1.0.22

发布日期： 2023年6月19日

- **增强的QPT CLI向导/输出** — 向QPT CLI向导/输出添加了警告，提醒您在存在依赖关系时验证修补程序详细信息和要求。<!-- ACP2E-1963 -->
- **已添加Commerce 2.4.6的修补程序：**
   - 修复了`regexp cache tag`验证。<!-- MCLOUD-10226 -->
   - 通过减少相同部署配置加载的次数来提高性能。<!-- MCLOUD-10604 -->
- **为Commerce 2.3.7添加了2.4.6**&#x200B;的修补程序 — 修复了导致`catalog_product_entity_*`表的随机值递增而不是递增1的问题。<!-- MCLOUD-10032 -->
- **为Commerce 2.4.0添加了2.4.6**&#x200B;的修补程序 — 修复了说明`The file can't be deleted. Warning!unlink: No such file or directory`的错误，此错误是在从管理员刷新JS/CSS缓存时发生的。<!-- MCLOUD-10279 -->

## v1.0.21

发行日期： 2023年3月10日

- **对PHP 8.2**&#x200B;的增强支持 — 修复了某些PHP 8.2.x版本存在的兼容性问题，以支持Commerce 2.4.6。

## v1.0.20

发行日期： 2022年10月27日

- **添加了二级缓存改进修补程序** — 此修补程序修复了刷新Commerce版本2.4.0和2.4.1的本地二级缓存时出现的问题。<!-- MCLOUD-7845 -->

## v1.0.19

发行日期： 2022年9月13日

- **对PHP 8.1**&#x200B;的增强支持 — 修复了某些PHP 8.1.x版本的兼容性问题。

## v1.0.18

发行日期： 2022年8月11日

Adobe Commerce 2.4.5的关键修补程序：

- **使用Braintree付款的订单问题** — 此修补程序解决了阻止管理员发出新订单或重新订购的关键问题。<!-- MCLOUD-9137 -->

请参阅[启用Braintree付款时，管理员无法创建订单/重新订单](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html?lang=zh-Hans)。

## v1.0.17

发行日期： 2022年5月24日

修复了`patches.json`文件中安全修补程序的约束。

## v1.0.16

发行日期： 2022年3月31日

Adobe Commerce 2.3.3-p1及更高版本的关键修补程序：

更新了修补程序以解决导致未经身份验证的远程代码执行的&#x200B;**关键**&#x200B;漏洞。<!-- MCLOUD-8479 -->

请参阅[Adobe安全公告APSB22-12](https://helpx.adobe.com/cn/security/products/magento/apsb22-12.html)。

## v1.0.15

发行日期： 2022年3月10日

- **支持PHP 8.1** — 添加了对PHP 8.1的支持并放弃对PHP 7.0和7.1的支持。
- **已添加Adobe Commerce 2.3.3**&#x200B;的修补程序 — 产品页面上显示的货币已固定。

## v1.0.14

发行日期： 2022年2月13日

Adobe Commerce 2.3.3-p1及更高版本的关键修补程序：

添加了修补程序，以解决导致远程代码执行未经身份验证的&#x200B;**关键**&#x200B;漏洞。<!-- MCLOUD-8461 -->

请参阅[Adobe安全公告APSB22-12](https://helpx.adobe.com/cn/security/products/magento/apsb22-12.html)。

## v1.0.13

发行日期： 2021年10月25日

- **更新独白** — 已将`monolog`包所需的最低版本更新为`^2.3`。<!-- ACMP-1263 -->
- **不兼容的PHP方法** — 修复了Adobe Commerce版本2.4.3和2.3.7-p1.<!-- AC-384 -->的不兼容的PHP方法
- **PHP错误** — 修复了尝试应用修补程序时发生的`PHP error 'Undefined variable: errorMessage' ...`错误。<!-- ACP2E-138 -->

## v1.0.12

发行日期： 2021年8月12日

Adobe Commerce 2.4.3和2.3.7-p1的关键修补程序：

- **API速率限制问题** — 此修补程序更正了默认速率限制，该限制导致Web API无法处理数组中超过20个项目的请求。 此修补程序将提高速率限制的默认值。 请参阅Adobe Commerce [2.4.3发行说明](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

发行日期： 2021年7月29日

- **修复了应用B2B分层导航修补程序导致的问题** — 对于已应用B2B分层导航修补程序的客户，此修复程序解决了在切换商店视图后在“搜索”页面上显示的`Undefined offset`错误。<!--MCLOUD-5287-->

- **Paypal结帐修补程序** — 修复了PayPal Express显示先前下订单价格的Adobe Commerce 2.3.7问题。<!--MC-42674-->

- **修补程序类别支持** — 添加了对处理分配给质量修补程序的修补程序类别和源来源的支持。 类别允许客户在使用[Quality Patches Tool](https://github.com/magento/quality-patches)和全站点分析工具(SWAT)时使用过滤器和排序功能更快地查找修补程序。<!--MC-38577-->

## v1.0.10

发行日期： 2021年5月10日

- **与Adobe Commerce 2.3.7的兼容性** — 解决了在Adobe Commerce 2.3.7上安装的编辑器依赖项冲突。<!--MC-42131-->
- **修复了多次应用捆绑修补程序导致的问题** — 多次应用捆绑修补程序（包括其他已弃用的修补程序）可能会还原所包含的已弃用包。 现在，所有修补程序仅应用一次。 再次尝试应用同一包时，会显示一条消息，指出已应用该修补程序。<!--MC-41912-->
- **B2B分层导航修补程序** — 修复了另一个问题，该问题在用户启用B2B共享目录时阻止分层导航显示所有产品选项。<!--MCLOUD-7742-->

## v1.0.9

发行日期： 2021年2月1日

- **B2B分层导航修补程序** — 修复了在启用B2B共享目录时，分层导航无法显示所有产品选项的问题。<!--MCLOUD-6923-->
- **与PHP 7.4**&#x200B;的兼容性 — 修复了PHP 7.4的云修补程序兼容性问题。<!--MCLOUD-7367-->
- **已弃用的修补程序将变得可见** — 修复了一个云修补程序问题，该问题导致在应用包含已弃用修补程序的全部内容的替换修补程序之后，已弃用的修补程序在修补程序表中变得可见。 如果应用的修补程序组合了多个其他修补程序，则可能会发生这种情况。<!--MC-40626-->
- **应用修补程序时静默失败** — 修复了`git apply`命令在某些环境中静默应用修补程序失败的云修补程序问题。<!--MC-40529-->

## v1.0.8

发行日期： 2020年10月14日

- **magento/magento-cloud-patches的兼容性更新** — 更新了`symfony`文件中的`semver`和`composer.json`版本约束，以便与Adobe Commerce 2.4.1及更高版本兼容。<!--MCLOUD-7111-->

## v1.0.7

发行日期： 2020年10月14日

- **适用于Adobe Commerce 2.3.0的Redis修补程序到2.3.5、2.4.0** — 更新了Redis修补程序，以支持在实施2级缓存时将产品添加到类别中。<!--MCLOUD-6659-->

- **Braintree VBE修补程序** — 修复了在管理员尝试查看Braintree结算报告时生成错误的问题。<!--MCLOUD-6684-->

- 现在，如果Git在主机系统上不可用，`ece-patches apply`命令使用Unix `patch`命令来应用修补程序。<!--MCLOUD-7069-->

## v1.0.6

发行日期：

- **适用于Adobe Commerce 2.3.0 - 2.3.4**&#x200B;的Redis修补程序 — 优化通信并改进性能
   - 缩小Redis和Adobe Commerce之间的网络传输量
   - 修复Redis加载和写入操作的争用情况
   - 重写基本缓存适配器以处理保存时的错误
   - 降低Redis CPU消耗<!--MCLOUD-6139-->

- **适用于Adobe Commerce 2.3.0 - 2.3.5**&#x200B;的Redis修补程序 — 提高性能并修复错误
   - 修复了缓存锁定实施以防止无限锁定
   - 改进当前锁定机制
   - 实施已签名的锁定以防止对并行请求进行解锁
   - 修复了Redis写入操作中发生的以下错误： `OOM command not allowed when used memory > maxmemory`
   - 修复了在产品更新`cat_p`期间运行的<!--MCLOUD-6110-->标记对干净缓存的处理

- 修复了在使用Adobe Commerce v2.2.6或2.3.5（不包括此模块）的Adobe Commerce基础架构项目中将所需的`amzn/amazon-pay-module`修补程序应用到时导致错误的问题。 现在，如果未安装模块，修补过程将跳过`amzn/amazon-pay-module`修补程序。<!--MCLOUD-6588-->

## v1.0.5

发布日期： 2020年6月26日

- **Redis性能改进** — 将Redis优化功能添加到Adobe Commerce版本2.3.3和2.3.4。这些修复包含在Adobe Commerce版本2.3.5版本中。<!--MCLOUD-5771-->

- **New Relic日志丰富程序** — 添加支持在New Relic版本1.0.4的云组件中引入的Commerce日志记录功能改进所需的单色处理器界面。部署Adobe Commerce 2.1.x时需要此修补程序。如果未应用该修补程序，则生成将在`di:compile`进程中失败。<!--MCLOUD-6029-->

## v1.0.4

发行日期： 2020年5月12日

- **Amazon Pay结帐** — 修复了Amazon Pay付款构件存在的问题，该问题阻止客户在结帐过程中在&#x200B;_审核与付款_&#x200B;步骤中更改付款方式。<!--MCLOUD-5930-->

- **类别页面上的产品显示** — 修复了导致产品无法在&#x200B;_显示所有页面_&#x200B;视图的类别页面上显示的问题。<!--MCLOUD-5684-->

- **页面生成器图像上传** — 修复了页面生成器界面问题，该问题有时在将图像上传到图像库时会导致以下错误： `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **禁止不必要的站点地图生成警告** — 在站点地图生成期间发生错误时添加重试尝试，并在可以自动恢复错误的情况下跳过客户电子邮件通知。<!--MCLOUD-3025-->

- **网站性能改进** — 修复了`Magento\Framework\App\DeploymentConfig\Reader::load`函数的性能问题，该函数会定期经历影响网站性能的长时间加载。<!--MCLOUD-5650-->

- 更新了支付方式修补程序的修补程序分配，以支付模块为目标，而非Magento基本包(magento/magento2-base)，这样只有当支付模块存在时才应用支付修补程序。<!--MCLOUD-5666-->

- 更新了修补程序以与Magento Open Source兼容。<!--MCLOUD-5701-->

## v1.0.3

发行日期： 2020年4月28日

- 添加了对“部署期间已禁用FPC”修补程序的修复，以支持Adobe Commerce 2.3.5。

## v1.0.2

发行日期： 2020年2月27日

此版本包括以下补丁程序和关键修复：

- **magento/magento-cloud-patches的兼容性更新**

   - 更新了`symfony`文件中的`semver`和`composer.json`版本约束以与Adobe Commerce 2.4及更高版本兼容。<!--MAGECLOUD-5127-->

   - 更新了`composer.json`中的约束，以与`ece-tools` 2002.0.22及更高版本2002.0.x兼容。

- **PayPal Express签出** — 发布于2020年2月12日，此修补程序解决了影响使用PayPal Express签出的订单的问题，其中订单的送货地址指定了已手动输入文本字段，而不是从“送货”页面的下拉菜单中选择的国家/地区区域。 请参阅修补程序下载页面上的完整修补程序说明。

- **应用程序部署修复** — 添加了一个修补程序，用于修复在部署过程中禁用了全页缓存的问题。 此修补程序适用于Adobe Commerce 2.3.2及更高版本。

- 异步/批量API的&#x200B;**Scope参数** — 更新了此修补程序以修复`composer.json`文件中的语法错误。 此补丁适用于Magento Open Source 2.3.1和2.3.2。请参阅修补程序下载页面上的完整修补程序说明。

## v1.0.1

发行日期： 2020年2月6日

我们已从magento/magento-cloud-patches v1.0.1版本的软件下载页面中获取所有Magento Open Source 2.x修补程序。 如果以前将任何修补程序复制到项目中，请删除它们以避免冲突。

此版本包括以下补丁程序和关键修复：

- **修复cron死锁并改进cron锁定**—

   - 修复了由于`cron_schedule`表中的状态值不正确而导致某些cron作业无法运行的问题。 现在，我们使用Adobe Commerce锁定框架来检查和更新cron作业状态，而不是使用`cron_schedule`表。 以错误状态结束的Cron作业将在下次cron运行期间重试，而不是等待24小时。

   - 添加&#x200B;_重试_&#x200B;操作，以避免在更新`cron_schedule`表中的数据时发生死锁。

- **更新`magento/magento-cloud-patches`以包含Magento Open Source 2.x的所有可用修补程序** — 更新magento/magento-cloud-patches程序包以包含软件下载页面上可用的所有Magento Open Source 2.x修补程序。 如果您之前已将任何Magento Open Source修补程序复制到Adobe Commerce on cloud infrastructure项目中，请将其删除以避免冲突。<!--MAGECLOUD-4606-->

- **Elasticsearch目录分页修复** — 使用更有效的修复程序替换了magento/magento-cloud-patches v1.0中提供的Elasticsearch目录分页修补程序。<!--MAGECLOUD-4847-->

- **Page Builder修补程序** — 在Commerce 1.0.0的Cloud修补程序中，我们捆绑了Page Builder修补程序，以解决已知的Page Builder远程代码执行(RCE)漏洞，初始修补程序基于Adobe Commerce 2.3.3。我们更新了这些修补程序，并基于Adobe Commerce 2.3.4进行了更加稳定的实施，包括用于修复问题的多个优化。<!--MAGECLOUD-4884-->

  如果您有magento/magento-cloud-patches 1.0.0包，则仍会受Page Builder RCE漏洞问题的保护。 如果更新到1.0.1或更高版本，则实施同一修复的效果会更好。

## v1.0.0

发行日期： 2019年11月14日

这是[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)包的第一个版本，它是`ece-tools`包版本2002.0.22或更高版本的新依赖项。

此版本包括以下补丁程序和关键修复：

- **适用于2.3.1.x和2.3.2.x版的Page Builder安全修补程序** — 修复了Page Builder预览中的问题，该问题允许未经身份验证的用户访问某些模板化方法，这些方法可用于触发通过网络执行的任意代码(RCE)，从而导致全局信息泄漏。 在Adobe Commerce版本2.3.1和2.3.2.<!--MAGECLOUD-4649-->中使用不受支持的页面生成器版本时，可能会出现此问题

- **MSI修补程序** — 修复了在使用默认库存设置管理库存时导致索引错误和性能问题的情况。<!--MAGECLOUD-4428-->

- **新邮件接口的向后兼容性** — 修复了Adobe Commerce v2.3.3中引入的`Magento\Framework\Mail\EmailMessageInterface` PHP接口导致的向后不兼容问题。在此修补程序的范围内，新`EmailMessageInterface`继承自旧`MessageInterface`，Adobe Commerce核心模块将还原为依赖于`MessageInterface`。<!--MAGECLOUD-4422-->

- **目录分页在Elasticsearch 6.x上不起作用** — 修复了搜索结果分页的一个严重问题，该问题影响使用Elasticsearch 6.x作为目录搜索引擎的客户。<!--MAGECLOUD-4448-->
