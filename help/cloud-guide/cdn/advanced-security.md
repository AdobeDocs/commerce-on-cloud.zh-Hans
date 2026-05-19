---
title: Adobe Commerce高级安全性
description: 了解Advanced Security如何在Adobe Commerce on Cloud Infrastructure中添加机器人管理、高级速率限制和第7层DDoS保护。
feature: Cloud, Configuration, Security
exl-id: 7aeb189f-be69-45d5-8163-4748424083c0
source-git-commit: 3cc2b8aa0c70e56288de270c8bdf9a317ef22cc7
workflow-type: tm+mt
source-wordcount: '1986'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security]是与[!DNL Adobe Commerce on Cloud Infrastructure]一起使用的产品，可让您的在线商店保持快速、可用和安全。 这有助于在流量高峰期事件和自动攻击期间保护收入、减少停机时间，并维护客户信任。

[!DNL Adobe Commerce on Cloud Infrastructure]包含内置[第3层和第4层DDoS保护](./fastly.md#ddos-protection)以及[Web应用程序防火墙(WAF)](./fastly-waf-service.md)。 在[责任分担模型](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)中，第7层DDoS检测、机器人保护和主动IP阻止是商家责任，而[!DNL Adobe Commerce Advanced Security]旨在解决这些责任。

[!DNL Advanced Security]通过Fastly支持的边缘安全功能扩展了店面保护，该功能提供了机器人管理、高级速率限制和第7层DDoS保护，是整合了网络边缘的规模、性能和安全性的统一边缘平台的一部分。

>[!NOTE]
>
>[!DNL Advanced Security]仅可用于[!DNL Adobe Commerce on Cloud Infrastructure] (PaaS)项目。

## 核心功能

[!DNL Adobe Commerce Advanced Security]包括以下附加保护：

- **[机器人管理](https://docs.fastly.com/products/bot-management)** — 识别并减少Web应用程序上不需要的机器人活动。 机器人管理服务将合法机器人（搜索引擎爬虫、社交媒体机器人）与恶意机器人区分开来，在网络边缘提供实时分类，并包含阻止、允许、质询或速率限制流量的选项。

- **[DDoS保护](https://docs.fastly.com/products/fastly-ddos-protection)** — 提供第7层（应用层）DDoS保护，该保护超出了所有[!DNL Adobe Commerce on Cloud Infrastructure]项目中包含的现有第3层和第4层保护。 DDoS Protection Service吸收了大规模体积攻击，并确保在分布式拒绝服务(DDoS)事件期间应用程序保持连续可用性，从而保护了在流量高峰期间的收入。

- **[高级速率限制](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** — 提供可配置的速率限制规则，保护特定URL、API端点和应用程序资源不受滥用。 高级速率限制服务超出了Fastly CDN模块提供的[基本速率限制](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)，可针对特定流量模式和攻击向量，从而减少基础架构紧张和云成本。

>[!NOTE]
>
>[!DNL Advanced Security]配置当前需要提交支持票证。 计划在未来版本中通过管理员UI进行自助配置。 有关详细信息，请参阅[请求 [!DNL Advanced Security]](#request-advanced-security)。

## 威胁覆盖

[!DNL Advanced Security]保护店面免受一系列自动化和应用层威胁。

![Adobe Commerce安全栈栈中的高级安全定位](../../assets/advanced-security.svg)

### 机器人滥用

- **凭据填充** — 自动尝试使用数据泄露中盗用的凭据登录。
- **帐户接管** — 尝试对客户帐户进行未经授权的访问的机器人。
- **帐户创建滥用** — 自动创建虚假帐户以进行欺诈或滥用。
- **信用卡测试** — 对您的付款处理器测试盗用的信用卡号码的机器人。
- **内容抓取** — 从店面自动提取产品数据、定价或内容。
- **库存囤积** — 将产品保存在购物车中的机器人以防止合法购买。

### AI机器人管理

- **AI爬虫检测** — 标识和管理未经同意而刮取内容以训练大型语言模型的AI爬虫。
- **AI获取器控件** — 控制实时AI 搜索结果中使用的AI获取器。
- **可配置AI机器人策略** — 使用可配置信号类型区分验证和可疑的AI机器人以执行策略。

### 应用层攻击

- **第7层DDoS攻击** — 针对绕过第3层和第4层内置保护的应用层的分布式攻击。 [!DNL Advanced Security]在到达原始服务器之前在边缘吸收这些体积攻击。
- **URL和API滥用** — 针对特定URL或API端点的攻击分布在大量IP地址中，单个IP拦截无效。
- **缓存无效攻击** — 包含人为设计的查询参数的请求，这些查询参数旨在绕过CDN缓存并覆盖原始服务器。

### 其他功能

- **动态挑战** — 自动将最佳挑战分配给可疑通信。 利用私有访问令牌(PAT)无缝验证部分请求，而不影响用户体验。
- **欺骗技术** — 通过向攻击者返回虚假信息来解决帐户接管尝试问题，从而减少攻击并破坏其大规模操作能力。

## 选择正确的保护

使用以下指南确定[!DNL Advanced Security]是否是您店面保护需求的正确解决方案，还是现有的保护或替代解决方案更合适。

### 何时使用[!DNL Advanced Security]

以下情形最好使用[!DNL Advanced Security]来处理：

| 方案 | [!DNL Advanced Security]如何帮助 |
|---|---|
| 您的网站遇到机器人驱动的攻击，例如凭据填充、内容刮取或库存囤积 | 机器人管理在自动威胁到达您的应用程序之前识别并缓解这些威胁 |
| 您需要第7层DDoS保护超出内置的第3层和第4层保护范围 | DDoS Protection吸收绕过网络级保护的应用程序层攻击 |
| 特定URL或API端点被无法被IP阻止的高流量分布式流量定向 | 高级速率限制为特定端点和流量模式提供精细化的控制 |
| 您希望管理访问您店面内容的AI爬虫和提取器 | 机器人管理包括可配置的AI机器人检测和执行策略 |
| 您需要一个支持Adobe的边缘安全解决方案与您现有的Fastly CDN集成 | [!DNL Advanced Security]在同一个Fastly Edge平台上运行，该平台已为您的店面提供服务 |

### 何时使用现有保护

以下情形最适合现有的保护：

| 方案 | 推荐的方法 |
|---|---|
| 单个IP或一组可识别的IP向您的站点大量发送请求 | 使用Commerce管理员或Fastly API阻止IP。 使用内置[第3/4层DDoS保护](./fastly.md#ddos-protection)和现有的[IP 列入阻止列表](./fastly-vcl-blocking.md) VCL代码段。 |
| 您需要阻止SQL注入、跨站点脚本(XSS)或其他OWASP十大威胁 | 包含的[WAF服务](./fastly-waf-service.md)会自动阻止这些威胁。 |
| 可以使用基本VCL阻止规则控制DDoS攻击模式 | 使用Adobe Commerce中已提供的现有[自定义VCL代码片段](./fastly-vcl-custom-snippets.md)。 |

### 何时使用替代保护

以下情形最适合使用可补充[!DNL Advanced Security]的替代保护：

| 方案 | 推荐的方法 |
|---|---|
| 您需要事务级欺诈评分或支付欺诈预防 | 使用专门的防欺诈平台。 [!DNL Advanced Security]在边缘网络级别进行保护，不评估单个付款交易。 |
| 您需要身份和访问管理(IAM) | 实施专用的IAM解决方案。 用户身份验证和会话管理仍由客户负责。 |
| 您需要静态或动态应用程序安全测试(SAST/DAST) | 使用专用的应用程序安全测试工具。 未提供代码级漏洞扫描。 |
| 您需要超出速率限制的综合API安全性（例如架构验证或API网关功能） | 考虑一个专用的API安全平台。 |
| 您需要法规合规性工具，如PCI扫描或SOC报告 | 使用专用的合规性管理工具。 |

>[!TIP]
>
>如果您当前使用第三方机器人保护提供程序，则合并到[!DNL Advanced Security]可以降低操作复杂性，并消除跨提供程序的不一致的安全覆盖范围。 请联系您的Adobe客户团队以评估您的项目的[!DNL Advanced Security]。

## 安全栈栈定位

[!DNL Advanced Security]适合更广泛的Adobe Commerce安全体系结构，是附加的基于边缘的保护层。 它与WAF以及已包含在[!DNL Adobe Commerce on Cloud Infrastructure]中的第3/4层DDoS保护一起工作，并且不会取代。 以下各节将阐明它与现有保护机制的关系，以及客户应承担的责任。

### 包含保护

[!DNL Adobe Commerce on Cloud Infrastructure]包括以下安全功能：

- **[Web应用程序防火墙(WAF)](./fastly-waf-service.md)** — 针对SQL注入、跨站点脚本(XSS)和其他开放Web应用程序安全项目(OWASP)十大威胁的受管保护。 仅在生产环境中可用。
- **[第3层和第4层DDoS保护](./fastly.md#ddos-protection)** — 内置针对网络层攻击的保护，例如SYN泛洪、UDP泛洪、基于ICMP的攻击和TCP级别的攻击。 已使用Fastly CDN自动启用。
- **[SSL/TLS证书](./fastly-configuration.md#provision-ssltls-certificates)** — 用于安全HTTPS流量的经域验证的加密证书。
- **[源遮蔽](./fastly.md#origin-cloaking)** — 确保通过Fastly的所有流量路由，阻止对源服务器的直接访问。
- **[基于VCL的安全片段](./fastly-vcl-custom-snippets.md)** — 用于IP阻塞、列入允许列表和请求过滤的自定义Varnish配置语言(VCL)规则。

### [!DNL Advanced Security]

[!DNL Advanced Security]提供了比[!DNL Adobe Commerce on Cloud Infrastructure]附带的内置保护更多的保护，但需要额外付费：

- **机器人管理** — 基于Edge的机器人检测和缓解与AI机器人管理。
- **第7层DDoS保护** — 应用层DDoS吸收和防御。
- **高级速率限制**—URL和API端点的粒度速率控制。
- **动态挑战和欺骗技术** — 自动挑战分配和帐户接管缓解。

### 客户责任

- **欺诈预防** — 交易级欺诈评分和支付欺诈检测。
- **身份和访问管理** — 客户身份验证、授权和会话管理。
- **应用程序安全测试**—SAST/DAST和漏洞扫描。
- **自定义安全配置** — 基于VCL的规则、IP规则和允许列表。
- **合规性工具** — PCI扫描、SOC合规性报告和法规审核工具。
- **应用程序级强化** — 基于令牌的API身份验证、查询参数规范化和缓存策略设计。

有关Adobe和客户安全责任的完整概述，请参阅[责任分担模型](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)。

## 常见攻击模式和保护

下表将常见攻击模式映射到Adobe Commerce安全栈栈中的相应保护层。

| 攻击模式 | 类型 | 保护 |
|---|---|---|
| 单个IP或一组可识别IP，发送大量请求 | DDoS +机器人 | 使用Commerce管理员或Fastly API阻止IP。 内置的第3/4层DDoS保护可在网络边缘过滤此流量。 |
| 对分布在大量IP上的特定URL或API的攻击 | DDoS +机器人 | **[!DNL Advanced Security]**：高级速率限制限制限制每个URL的请求卷。 机器人管理识别和阻止分布式机器人流量。 |
| 对REST API端点的自动攻击，而无需适当的身份验证 | 机器人+ DDoS | 验证API端点是否使用基于令牌的身份验证。 如果令牌受损，则轮换凭据。 **[!DNL Advanced Security]**：高级速率限制可以保护公开的端点。 |
| 使用操纵的查询参数的防缓存攻击 | 机器人+ DDoS | 从缓存键中排除不必要的查询参数。 在应用程序级别规范和限制查询参数。 **[!DNL Advanced Security]**： Bot Management检测并阻止自动的防缓存流量。 |
| SQL注入或跨站点脚本(XSS)尝试 | WAF | 包含的[WAF服务](./fastly-waf-service.md)使用托管安全规则自动阻止这些威胁。 |

### WAF阻止行为

以下WAF行为适用于所有[!DNL Adobe Commerce on Cloud Infrastructure]项目，无论是否启用了[!DNL Advanced Security]。 随附的WAF服务对常见的攻击信号使用以下阻止行为：

- 立即阻止&#x200B;**SQL注入**&#x200B;请求，即使对于单个匹配请求也是如此。
- 来自已知恶意IP的以下威胁信号识别的请求会被立即阻止：后门、攻击工具、CMDEXE、Log4J JNDI、遍历和XSS。
- 如果来自显示上述威胁信号的非恶意IP的请求超过以下阈值，则会将其阻止：

| 间隔 | 阈值 | 检查频率 |
|---|---|---|
| 1分钟 | 50个请求 | 每20秒 |
| 10分钟 | 350个请求 | 每3分钟 |
| 1小时 | 1,800个请求 | 每20分钟 |

## 请求[!DNL Advanced Security]

要请求[!DNL Advanced Security]：

>[!NOTE]
>
>[!DNL Advanced Security]需要额外付费，并且需要有效的[!DNL Adobe Commerce on Cloud Infrastructure] (PaaS)订阅。

1. 请联系您的Adobe客户团队或Adobe销售代表，讨论您项目的[!DNL Advanced Security]。

1. 购买[!DNL Advanced Security]后，[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，请求[!DNL Advanced Security]启用。 包括您的[!DNL Adobe Commerce on Cloud Infrastructure]项目ID和需要启用的环境（例如，生产和暂存）。

1. Adobe在您的Fastly服务上激活[!DNL Advanced Security]并配置初始保护策略。 启用通常在提交票证后的几个工作日内完成。

1. 您收到确认[!DNL Advanced Security]处于活动状态，以及有关为您的环境启用的保护的详细信息。

>[!NOTE]
>
>对[!DNL Advanced Security]的配置更改当前需要[提交支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 计划在未来版本中通过管理员UI进行自助配置。

## 限制

[!DNL Advanced Security]提供边缘层店面保护。 以下功能不可用，最好使用补充性解决方案来处理：

- **交易级别的欺诈得分**—[!DNL Advanced Security]不评估单个付款交易的欺诈风险。 使用专用的防欺诈平台进行交易级评分。
- **身份和访问管理(IAM)**—[!DNL Advanced Security]不管理用户身份验证、授权或会话管理。 这些仍然是客户的责任。
- **静态和动态应用程序安全测试(SAST/DAST)**—[!DNL Advanced Security]不包括代码级漏洞扫描或渗透测试。
- **API安全** — 虽然高级速率限制可以保护API端点免遭滥用，但是未提供诸如架构验证和API网关管理等全面的API安全功能。
- **全面防欺诈**—[!DNL Advanced Security]侧重于边缘层店面保护，不是完整的欺诈管理平台。
- **合规性工具**—[!DNL Advanced Security]不提供PCI扫描、SOC合规性报告或法规审核功能。
