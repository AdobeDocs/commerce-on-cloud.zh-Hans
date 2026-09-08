---
title: 调查行动手册
description: 了解如何使用Adobe Commerce流量分析调查CDN带宽超额、搜索机器人和爬虫负载以及恶意流量，并了解何时上报。
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# 调查行动手册

[!DNL Adobe Commerce Traffic Insights]应用旨在帮助您调查以下问题：

- 带宽超额
- 爬虫加载
- 恶意流量

或者，您可以在手动缓解不足时请求[高级安全性：本机机器人管理、第7层DDoS和速率限制](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)、Adobe的本机升级路径。 每个步骤都引用显示症状的构件，以便您从量度转到具体操作。

>[!WARNING]
>
>本页上的建议只是指南。 在部署之前，请始终针对您自己的流量验证任何阻止规则。

## CDN带宽过量

在考虑带宽超额之前，请了解带宽的计费方式。 与[!DNL Adobe Commerce on Cloud Infrastructure]帐户捆绑的&#x200B;**所有** Fastly服务（包括每个生产&#x200B;**和**&#x200B;暂存环境）的流量计入合同中的常见用量和年度津贴。 从&#x200B;**Bandwidth > Total Bandwidth**&#x200B;开始，然后按内容类型&#x200B;**将卷归为** Bandwidth，按域详细信息&#x200B;**归为** Bandwidth。

### 媒体内容

有些商店因其目录而合法地提供很大一部分带宽作为媒体。 如果&#x200B;**按内容类型划分的带宽**&#x200B;显示大量媒体带宽，请考虑以下缓解措施：

- 尝试使用[Fastly有损转换](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion)来提供较小、质量较低的图像。
- 调查[Fastly Deep Image Optimization](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization)以在内容交付网络(CDN)端生成调整大小的图像。

### 大文件

某些站点包含大型文件或特定的繁重响应，例如，企业资源规划(ERP)集成或导出。 使用&#x200B;**URL By Bandwidth**&#x200B;查看&#x200B;**BW**&#x200B;和&#x200B;**平均大小**&#x200B;列以查找这些大型文件。 您可以将&#x200B;**路径段lvl 1按带宽**&#x200B;用于更高级别的视图。

### 重型404s

找不到Adobe Commerce **404页面**&#x200B;通常是主题样式的繁重页面(~1.5 MB)和&#x200B;**不可缓存**，因此重复的404可能会产生异常流量。 即使是像`favicon.ico`这样微不足道的缺失资源，也可能变成大量`404`页面而不是小文件。 使用&#x200B;**按域详细信息的带宽**、**按带宽的URL**、**按带宽的顶级IP**&#x200B;和&#x200B;**按IP子网统计的数据**&#x200B;中的&#x200B;**404**&#x200B;和&#x200B;**404 BW**&#x200B;列来查找始终生成404卷的客户端、IP和URL。 然后减少或限制该访问，例如，返回轻量级`403`。

### 低FPC命中率

[!DNL Adobe]建议启用Fastly [屏蔽](https://www.fastly.com/documentation/guides/concepts/shielding/)，以便主CDN缓存聚合器提供源，从而减少从离客户端最近的本地存在点([POP](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/))到达它的请求。 请参阅[检查您的配置](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding)。

POP到客户端和屏蔽到POP的流量单独计数，在压缩客户端响应时，屏蔽到POP的流量[不压缩](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge)以保留Edge Side Include ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/))支持。 这意味着低全页缓存(FPC)命中率会大幅提升动态页面的带宽。 使用&#x200B;**FPC命中率**、**按域划分的FPC统计信息**&#x200B;和&#x200B;**CDN网络段带宽**&#x200B;确认症状。

低点击率通常由大量搜索引擎爬虫驱动（请参阅[搜索机器人和爬虫](#search-bots-and-crawlers)）。 另一种缓解措施是[向爬虫](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/)提供过时的缓存（如果可用）。 如果原因是广泛、频繁的缓存无效，请使用&#x200B;**按标记缓存无效**&#x200B;和&#x200B;**按顶级URL的FPC存留期**&#x200B;来查找流失的标记/URL。

## 搜索机器人和爬虫

要衡量爬虫影响，请从&#x200B;**按带宽划分的已知机器人**&#x200B;和&#x200B;**已知机器人影响详细信息**&#x200B;开始，以查看哪些机器人最活跃，然后[按特定机器人筛选](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use)，以仅研究其请求。

### 请求过多

搜索机器人发送过多请求的最常见原因是在分析带有`<meta name="robots" content="index,follow">`的页面时发生。 机器人可以近乎无穷无尽的循环跟踪顶部导航和分层导航链接。 请考虑以下选项来解决此问题：

>[!WARNING]
>
> 在限制爬虫活动之前，请咨询搜索引擎优化(SEO)专家。 重新培训可能会对您的SEO产生负面影响。

- 将`nofollow`添加到顶部导航和分层导航链接，例如`<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`。
- 将页面Meta标记更改为`index,nofollow` — 作为常用的[设计配置设置](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt)或具有自定义扩展名的每个页面类型。 保持`sitemap.xml`准确，以便机器人始终具有要索引的最新页面列表。
- 更新`robots.txt`以阻止路径，资源机器人不应访问。
- 请注意，`crawl-delay`指令不是官方机器人排除协议的一部分，但它确实适用于某些机器人，例如Bingbot、Slurp、SEMrushBot和其他一些机器人。 GoogleBot忽略该指令。
- 添加速率限制规则。 Fastly模块中存在本机[滥用爬虫保护](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection)。 对于具有单个速率限制的用户代理正则表达式，[自定义清漆配置语言(VCL)代码片段](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets)可以返回`429` （请求过多）或`405` （不允许使用方法），以实现更精细的控制。 查看爬虫文档以了解首选方法和响应代码。 请参阅Fastly的[速率限制VCL指南](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/)。
- AI和大型语言模型(LLM)爬虫是一个日益增长的特殊情况。 它们并不总是能够一致地标识自身，因此VCL用户代理规则可能会滞后。 Adobe的[高级安全性](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)加载项具有[本机机器人管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)，它可以与边缘的可疑AI爬虫和获取器区分开来，仅使用VCL无法区分它们。

### 阻止不需要的爬虫

如果某些搜索引擎产生了大量流量，并且对业务并不重要，则可以完全阻止它们：

- 某些机器人在重新读取并更新其解析规则后，会在1-2天后跟进`robots.txt`更改。
- 如果爬虫忽略了`robots.txt`，请使用自定义VCL代码片段([example](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent))阻止它。 有些爬虫明确说明这是频率控制的首选或唯一方法。

## 恶意脚本和抓取程序

使用流量分析应用程序确定常见的攻击方向，并根据需要按焦点区域进行过滤。 如果带有红色标记的请求主要来自某些IP、子网或地理位置（**个按请求数排列的顶级IP**、**个按IP子网排列的统计信息**、**个按国家/地区排列的统计信息**），请考虑使用自定义Fastly VCL阻止它们。

每个云基础架构项目都有一个自动保护基准，无论您进行任何配置。 随附的Web应用程序防火墙(WAF)会立即阻止SQL注入和已知恶意IP信号（后门、攻击工具、CMDEXE、Log4J-JNDI、遍历、XSS），并在其他非恶意IP超过50个请求/分钟、350个请求/10分钟或1,800个请求/小时后设置速率限制。 该基线是&#x200B;**WAF响应请求**&#x200B;以及此应用程序表中的WAF信号列所指示的内容。 这些列中的尖峰并不一定意味着您未受到保护。

- 关注凭据填充、帐户接管、虚假帐户创建、卡测试、内容刮取和库存/购物车囤积。 这些机器人驱动的滥用模式出现在&#x200B;**机器人活动和请求分析**&#x200B;选项卡中。 点击登录、帐户、结帐或目录端点的高容量、低多样性流量是要在按请求计数&#x200B;**和**&#x200B;已知机器人影响详细信息&#x200B;**列出的**&#x200B;顶级IP中查找的签名。
- 使用[Google reCAPTCHA](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/captcha/security-google-recaptcha)保护签出和签出API端点免受机器人攻击。
- 使用Fastly模块的本机速率限制[路径保护](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection)。
- 在逗号分隔的`Sigsci_Tags`字段中检查[下一代WAF信号](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/)，并将相关的信号匹配合并到目标阻止规则中。 可疑请求的值可能类似于`BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`。 WAF在自动开始阻止之前使用`SITE-FLAGGED-IP`标记的IP的阈值已满。 由WAF响应的&#x200B;**WAF攻击和异常信号**、**WAF机器人信号**&#x200B;和&#x200B;**请求**&#x200B;小组件以及IP、子网和国家/地区表中的WAF列反映了这些信息。
- 请参阅Adobe关于[阻止Fastly级别](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level)Adobe Commerce的恶意流量的文章以了解常见方法。
- 对于手动阻止不可行的复杂情况，例如持续的机器人营销活动、跨多个IP/API的攻击或第7层分布式拒绝服务(DDoS)，请首先考虑Adobe的[高级安全性](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)加载项（请参阅[本机机器人管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)）。 它和你的店面在同一个飞天族的边上开着。 如果您需要超出其范围的功能，建议使用具有原生Fastly集成的第三方托管机器人缓解服务，例如[Datadome](https://docs.datadome.co/docs/module-fastly)或[HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/)（以前称为PerimeterX）。 所有这些选项都会增加额外成本。

## 高级安全性：本机机器人管理、第7层DDoS和速率限制

前几节将讨论如何使用流量分析应用程序的数据和手动Fastly VCL。 对于不足的情况，如持续的或不断发展的机器人营销活动、第7层（应用层）DDoS或大量跨多个IP和API端点的滥用，Adobe提供了[高级安全性](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)。

高级安全性是[!DNL Adobe Commerce on Cloud Infrastructure]的付费附加组件，在已为店面提供服务的同一Fastly平台上添加了边缘机器人管理（包括AI爬虫和提取器检测）、第7层DDoS保护和高级速率限制。 请参阅[高级安全性](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/advanced-security)，了解全部功能、当前限制以及如何请求它。

购买并启用后，使用流量分析应用程序验证高级安全性是否正常工作。 其决策通过&#x200B;**WAF攻击和异常信号**、**WAF机器人信号**&#x200B;和&#x200B;**WAF响应请求**&#x200B;后面的相同`Sigsci_Tags`和`Agent_response`字段报告。 比较启用前后的这些构件，以确认其对您的流量执行了主动操作。
