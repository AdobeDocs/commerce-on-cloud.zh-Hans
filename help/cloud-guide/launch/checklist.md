---
title: 启动项核对清单
description: 查看站点启动项的清单项目。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# 启动项核对清单

在部署到生产环境之前，请下载[Launch核对清单](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf)，并按照以下说明使用它，以确认您已完成所有必需的配置和测试。 请在[部署您的商店](../deploy/staging-production.md)上查看入门和专业版完整部署过程的概述。

## 完全在生产环境中测试

请参阅[测试部署](../test/staging-and-production.md)，以测试网站、商店和环境的各个方面。 这些测试包括验证Fastly、用户验收测试(UAT)和性能测试。

## TLS和Fastly

Adobe为每个环境提供Let&#39;s Encrypt SSL/TLS证书。 Fastly需要此证书才能通过HTTPS提供安全流量。

要使用此证书，您必须更新DNS配置，以便Adobe完成域验证并将证书应用于您的环境。 每个环境都有一个唯一的证书，该证书涵盖了部署在该环境中的云基础架构站点上Adobe Commerce的域。 我们建议在[Fastly设置过程](../cdn/fastly-configuration.md)中完成并更新配置。

## 使用生产设置更新DNS配置

准备好启动站点时，必须更新DNS配置以通过Fastly服务从生产环境路由流量。

**先决条件：**

- [在您的开发环境中设置和测试Fastly](../cdn/fastly-configuration.md#)

- 生产环境配置已更新为所有必需域

  通常，您会与客户技术顾问合作，添加存储所需的所有顶级域和子域。 要添加或更改生产环境的域，请[提交Adobe Commerce支持票证](https://support.magento.com/hc/en-us/articles/360019088251)。 等待确认您的项目配置已更新。

  在入门项目中，您必须将域添加到项目。 请参阅[管理域](../cdn/fastly-custom-cache-configuration.md#manage-domains)。

- 为您的生产环境配置的SSL/TLS证书。

  如果您在Fastly设置过程中为生产域添加了ACME质询记录，则当您更新DNS配置以将流量路由到Fastly服务时，Adobe会自动将SSL/TLS证书上传到您的生产环境。 如果您没有预配置证书，或者您更新了域，则Adobe必须完成域验证并配置证书，这最多可能需要12小时。

### 要更新站点启动项的DNS配置，请执行以下操作：

1. 更新生产站点的以下DNS配置：

   - 设置所有必要的重定向，尤其是当您从现有站点迁移时

   - 设置区域的根资源记录以处理主机名

   - 降低生存时间(TTL)的值以刷新DNS信息以将客户指向正确的生产存储

     建议在切换DNS记录时显着降低TTL值。 此值可告知DNS缓存DNS记录的时长。 缩短后，它会更快地刷新DNS。 例如，您可以在更新网站时将TTL值从三天更改为10分钟。 请注意，缩短TTL值会增加DNS基础架构的负载。 在网站启动后恢复先前较高的值。


1. 添加CNAME记录以将生产环境的子域指向Fastly服务`prod.magentocloud.map.fastly.net`，例如：

   | 域或子域 | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. 如果需要，添加A记录以将Apex域(`<domain-name>.com`)映射到以下Fastly IP地址：

   | Apex域 | ANAME |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>[RFC1034](https://www.rfc-editor.org/rfc/rfc1912) （**第2.4**节）中的DNS说明指出：
>_CNAME记录不允许与任何其他数据共存。 换言之，如果suzy.podunk.xx是sue.podunk.xx的别名，则不能同时具有suzy.podunk.edu的MX记录、A记录甚至TXT记录。_
>
>因此，子域的DNS记录应为`CNAME`类型，apex域（根域）应为`A`类型。 放弃此规则可能会导致邮件服务或DNS传播中断，因为您将失去添加其他记录（如MX或NS）的能力。 某些DNS提供商可能会通过使用内部自定义来绕过此要求，但遵循此标准可确保稳定性和灵活性（例如，更改DNS提供商）。

1. 更新基本URL。

   - 使用SSH登录到生产环境。

     ```bash
     magento-cloud ssh -e production
     ```

   - 使用CLI更改商店的基本URL。

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **注意**：您还可以从管理员更新基本URL。 请参阅&#x200B;_Adobe Commerce商店和购买体验指南_&#x200B;中的[商店URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)。

1. 请等待几分钟，以便网站更新。

1. 测试您的网站。

## 验证生产配置

完成最后阶段以验证一个或多个存储的生产配置。 您可以在生产环境中更新配置。 如果设置是只读的，您可能需要打开SSH连接并使用CLI命令更改配置，或在本地环境中更改配置。 完成更新后，您可以将更改部署到暂存环境和生产环境。

以下是建议的更改和检查：

- [已完成传出电子邮件的测试](../project/outgoing-emails.md)

- [管理员凭据和基本管理员URL的安全配置](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [为Web优化所有图像](../cdn/fastly-image-optimization.md)

- [检查HTML、JavaScript和CSS的缩小设置](../deploy/static-content.md)

## 验证Fastly缓存

- 测试和验证Fastly缓存是否可以在生产网站上正常工作。 有关详细的测试和检查，请参阅[快速测试](../test/staging-and-production.md#check-fastly-caching)。

- [确保您的生产环境中安装了最新版本的Fastly CDN Module for Commerce](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [确保已上传Fastly VCL代码的最新版本](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## 性能测试

我们建议您查看[Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)选项，作为启动前准备过程的一部分。

您还可以使用以下第三方选项进行测试：

- [围攻](https://www.joedog.org/siege-home/)：流量整形和测试软件将您的存储推到极限。 使用可配置的模拟客户端数量点击您的网站。 围困支持基本身份验证、Cookie、HTTP、HTTPS和FTP协议。

- [Jmeter](https://jmeter.apache.org/)：卓越的负载测试，有助于评估尖峰流量的性能，如闪存销售。 创建针对您的网站运行的自定义测试。

- [New Relic](https://support.newrelic.com/s/)（已提供）：通过跟踪每个操作（如传输数据、查询、Redis等）的逗留时间，帮助查找导致性能变慢的网站进程和区域。

- [WebPageTest](https://www.webpagetest.org/)和[Pingdom](https://www.pingdom.com/)：实时分析不同来源位置的网站页面加载时间。 Pingdom可能需要付费。 WebPageTest是免费工具。

## 安全配置

- [设置安全扫描](overview.md#set-up-the-security-scan-tool)

- 管理员用户的[安全配置](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- 管理员URL的[安全配置](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [删除云基础架构项目上的Adobe Commerce上不再存在的所有用户](../project/user-access.md)

- [配置双重身份验证](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## 性能监控

您可以使用New Relic服务对Pro和Starter环境进行性能监控。 在Pro计划客户中，我们提供Adobe Commerce的托管警报警报策略，以使用New Relic APM和基础架构代理监控应用程序和基础架构性能。 有关使用这些服务的详细信息，请参阅[使用托管警报监视性能](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)。

### 下一步

[启动步骤](steps.md)
