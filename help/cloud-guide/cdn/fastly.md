---
title: Fastly服务概述
description: 了解云基础架构上Adobe Commerce中包含的Fastly服务如何帮助您优化和保护Adobe Commerce站点的内容交付操作。
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: 429b6762-0b01-438b-a962-35376306895b
TQID: https://experienceleague.adobe.com/Lq2rzR14xlcj5y3ycfAWGHEKAIxboekZX8YtyOJPQXA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
subfeature_v2:
  - id: f2261633-201d-46c5-8a66-999e70527a83
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 1637
ht-degree: 0%

---

# Fastly服务概述

>[!WARNING]
>
>要维护部署在Cloud平台上的Adobe Commerce站点的PCI合规性，请在您的Starter主分支、 Pro生产和暂存环境中设置Fastly。 如果在Headless部署中使用Adobe Commerce，我们强烈建议您使用Fastly缓存GraphQL响应。 请参阅&#x200B;*GraphQL开发人员指南*&#x200B;中的[使用Fastly缓存](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly)。

Fastly提供以下服务，以优化和保护Adobe Commerce在云基础架构项目上的内容交付操作。 这些服务包含在云基础架构的Adobe Commerce中，无需支付额外费用。

- **内容交付网络(CDN)** — 基于涂漆的服务，可在您设置的后端数据中心中缓存您的网站页面、资产、CSS等。 当客户访问您的网站和商店时，请求会点击Fastly以更快地加载缓存页面。 CDN服务提供以下功能：

- **缓存管理** — 将您的网站页面、资产、CSS等内容缓存在您为降低带宽负载和成本而设置的后端数据中心中

   - 使用[Fastly自定义VCL片段](fastly-vcl-custom-snippets.md)（符合Varnish 2.1）来修改缓存响应请求的方式

   - 设置[GeoIP服务支持](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [强制将未加密的请求转移到TLS](fastly-custom-cache-configuration.md#force-tls)

   - [自定义Fastly超时](fastly-custom-cache-configuration.md#extend-fastly-timeout)设置，以防止批量操作请求出现503响应

   - 创建[自定义错误响应页面](fastly-custom-response.md)

- **安全性** — 为Adobe Commerce站点启用Fastly服务后，可以使用其他安全功能来保护您的站点和网络：

   - [Web应用程序防火墙](fastly-waf-service.md) (WAF) — 托管的Web应用程序防火墙服务，可提供PCI兼容的保护来阻止恶意流量，以免破坏云基础架构网站和网络上的生产Adobe Commerce。 WAF服务仅在专业和入门生产环境中可用。

   - [分布式拒绝服务(DDoS)保护](#ddos-protection) — 内置DDoS保护可抵御常见的第3层和第4层攻击，如Ping of Death、Smurf攻击和其他基于ICMP的洪水攻击。 内置保护不包括针对第7层攻击的保护。 请参阅[DDoS保护](#ddos-protection)。

   - [SSL/TLS证书](fastly-configuration.md#provision-ssltls-certificates) — Fastly服务需要SSL/TLS证书才能通过HTTPS提供安全流量。

     Adobe Commerce为每个暂存和生产环境提供了一个经过域验证的Let&#39;s Encrypt SSL/TLS证书。 Adobe Commerce在Fastly设置过程中完成域验证和证书配置。

- **源遮蔽** — 安全功能，可确保所有流量流过Fastly并阻止对源服务器的直接访问。 请参阅下面的[源遮蔽](#origin-cloaking)部分。

- **[图像优化](fastly-image-optimization.md)** — 将图像处理和调整负载卸载到Fastly服务，以便服务器可以更高效地处理订单和转换。

- **[Fastly CDN和WAF日志](../monitor/new-relic-service.md#new-relic-log-management)** — 对于云基础架构Pro项目上的Adobe Commerce，您可以使用New Relic日志服务来查看和分析Fastly CDN和WAF日志数据。

## 源遮蔽 {#origin-cloaking}

源遮蔽是一项安全功能，可阻止非快速流量基于云基础架构源访问Adobe Commerce。 所有请求都必须遵循此强制路径：

**Fastly >负载平衡器> Adobe Commerce应用程序实例**

使用此路径可确保Fastly Web Application Firewall (WAF)和负载平衡器上的内部WAF检查所有流量。 源位置遮蔽可保护您的站点免受直接访问尝试并降低DDoS攻击的风险。

### 启用状态

自2021年以来，已在云基础架构项目的所有Adobe Commerce上完全启用源身份遮蔽。\
2021年后设置的项目默认包含此配置。\
**无需执行任何操作**&#x200B;即可请求启用原始遮蔽。

#### 哪些源遮蔽块

源地址遮盖会阻止对源地址基础设施的任何直接访问，例如：

```
mywebsite.com.c.abcdefghijkl.ent.magento.cloud
mcstaging2.mywebsite.com.c.abcdefghijkl.dev.ent.magento.cloud
mcstagingX.mywebsite.com.c.abcdefghijkl.X.dev.ent.magento.cloud
```

通过公共域发送的请求继续正常工作，包括REST API流量。 示例：

```
mywebsite.com/rest/default/V1/integration/admin/token
mywebsite.com/rest/default/V1/orders/
mywebsite.com/rest/default/V1/products/
mywebsite.com/rest/default/V1/inventory/source-items
```

#### 对服务行为的影响

- **传出IP地址未更改。**
- **REST API不受影响。** Fastly不缓存API调用。
- **部署和停机时间不受影响。**
- 如果项目具有多个暂存环境，则&#x200B;**源遮盖适用于所有这些环境**。

## 适用于Magento 2的Fastly CDN模块

云基础架构上Adobe Commerce的Fastly服务使用安装在以下环境中的Magento 2&rbrack;的&lbrack;Fastly CDN模块： Pro Staging and Production， Starter Production （`master`分支）。

在初始配置或升级Adobe Commerce项目时，Adobe会在暂存和生产环境中安装最新版本的Fastly CDN模块。 当Fastly发布模块更新时，您会在管理员中收到有关环境的通知。 Adobe建议您更新环境以使用最新版本。 查看[快速升级](fastly-configuration.md#upgrade-the-fastly-module)。

## Fastly服务帐户和凭据

云基础架构项目上的Adobe Commerce未获得专用的Fastly帐户。 Fastly服务在注册到Adobe的集中帐户中进行管理，并且管理功能板仅供云支持团队访问。

相反，每个暂存和生产环境都具有唯一的Fastly凭据（API令牌和服务ID），以便从Commerce管理员配置和管理Fastly服务。 Fastly API可用于执行Fastly服务的高级管理，这需要凭据才能提交这些请求。

在项目配置期间，Adobe会将您的项目添加到云基础架构上Adobe Commerce的Fastly服务帐户，并将Fastly凭据添加到暂存环境和生产环境的配置中。 查看[获取Fastly凭据](fastly-configuration.md#get-fastly-credentials)。

### 更改Fastly API标记

提交Adobe Commerce支持票证，以便在验证失败/已过期[&#128279;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials)或您认为其已受到侵害时，颁发新的Fastly API令牌凭据。

当您收到新令牌时，请更新您的暂存或生产环境以使用新令牌。

**要更改Fastly API令牌凭据**：

1. [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，请求新的Fastly API凭据。

   在云基础架构项目ID和需要新凭据的环境中包含您的Adobe Commerce。

1. 收到新API令牌后，请更新管理员中[Fastly凭据配置](fastly-configuration.md#test-the-fastly-credentials)中的API令牌值，或从[[!DNL Cloud Console] 环境变量](../project/overview.md#configure-environment)中更新。

1. [测试新凭据](fastly-configuration.md#test-the-fastly-credentials)。

1. 更新凭据后，提交Adobe Commerce支持票证以删除旧的API令牌。

### 多个Fastly帐户和分配的域

Fastly只允许您将一个Apex域和关联的子域分配给一个Fastly服务和帐户。 如果您现有的Fastly帐户链接了用于Adobe Commerce网站的相同顶点和子域，则您有以下选项：

- 在云基础架构项目环境中为您的Adobe Commerce请求Fastly服务凭据之前，从现有帐户中删除Apex和子域。 请参阅Fastly文档中的[使用域]。

  使用此选项将Apex域和所有子域链接到Adobe Commerce在云基础架构上的Fastly服务帐户。

- 提交Adobe Commerce支持票证以请求域委派，以便将Apex和子域链接到不同的帐户。

  如果您的Apex域中包含Adobe Commerce和非Adobe Commerce站点的多个子域，并且您想要将这些子域关联到不同的Fastly帐户，请使用此选项。

#### 请求域委派

*方案1：*

Apex域（`testweb.com`和`www.testweb.com`）链接到现有的Fastly帐户。 您在云基础架构项目上配置了Adobe Commerce，子域如下： `mcstaging.testweb.com`和`mcprod.testweb.com`。 您不希望将Apex域移动到云基础架构上Adobe Commerce的Fastly服务帐户。

提交[Fastly支持票证]，请求将子域从现有Fastly帐户委派给云基础架构上Adobe Commerce的Fastly帐户。 在票证中包含您的Adobe Commerce项目ID。

委派完成后，可以将您的项目子域添加到云基础架构上Adobe Commerce的Fastly服务帐户。 查看[获取Fastly凭据](fastly-configuration.md#get-fastly-credentials)。

*方案2：*

Apex域（`testweb.com`和`www.testweb.com`）已链接到云基础架构Fastly服务帐户上的Adobe Commerce。 您要为其他Fastly帐户的`service.testweb.com`和`product-updates.testweb.com`子域管理Fastly服务。

提交Adobe Commerce支持票证，请求将云基础架构Fastly服务帐户上的Adobe Commerce中的子域委派给Fastly帐户。 在票证中包含Fastly帐户的服务ID。

## DDoS保护

DDOS保护内置于Fastly CDN服务中。 一旦您为Adobe Commerce站点启用了Fastly服务，Fastly就会过滤所有Web和管理员流量以检测和阻止潜在的攻击。

- 对于针对第3层或第4层的攻击，Fastly服务会根据端口和协议过滤掉流量，仅检查HTTP或HTTPS请求。 ICMP 、 UDP和其他网络发起的攻击将在我们的网络边缘上删除。 这包括反射和放大攻击，它们使用SSDP或NTP等UDP服务。 通过提供这种级别的保护，我们有效地阻止了多种常见的攻击，如Ping of Death 、 Smurf攻击和其他基于ICMP的洪水。

  Fastly在缓存层管理TCP级别的攻击。 此策略为每个客户端提供了必要的规模和环境，以应对SYN洪水攻击及其许多变体，包括TCP栈栈、资源攻击和Fastly系统中的TLS攻击。

>[!NOTE]
>
>与Adobe Commerce集成的Fastly CDN服务不包含针对第7层攻击的保护。 有关防御第7层攻击的提示，请参阅&#x200B;*Adobe Commerce知识库*&#x200B;中的[检查DDoS攻击](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli)和[如何阻止恶意攻击](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level)。

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Checking for DDoS attacks]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[适用于Magento 2的Fastly CDN模块]: https://github.com/fastly/fastly-magento2

[Fastly支持票]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[How to block malicious traffic]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[使用域]: https://docs.fastly.com/en/guides/working-with-domains
