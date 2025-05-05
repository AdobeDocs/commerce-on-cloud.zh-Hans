---
title: 云基础架构安全性
description: 了解Adobe如何确保Adobe Commerce在云基础架构上的安全。
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# 安全性

Adobe Commerce [专业计划体系结构](pro-architecture.md)旨在提供高度安全的环境。 每个客户都部署到自己的独立服务器环境中，与其他客户分开。 生产环境的安全详细信息如下所述。

## Web浏览器

进出云环境的大部分流量来自消费者的Web浏览器。 消费者流量可使用适用于网站所有页面的HTTPS进行保护（使用共享SSL证书或客户自己的SSL证书收费）。 始终使用HTTPS提供结账和帐户页面。 最佳实践是提供HTTPS下的所有页面。

## 内容交付网络

Fastly提供内容交付网络(CDN)和分布式拒绝服务(DDoS)保护。 Fastly CDN有助于隔离对原始服务器的直接访问。 公共DNS仅指向Fastly网络。 Fastly DDoS解决方案可以抵御高中断性的第3层和第4层攻击，以及更复杂的第7层攻击。 可以使用基于整个HTTP/HTTPS请求以及基于客户端和请求条件（包括标头、Cookie、请求路径和客户端IP，或地理位置等指标）的自定义规则来阻止第7层攻击。

查看[Fastly服务概述](../cdn/fastly.md)。

## Web应用程序防火墙

Fastly Web Application Firewall (WAF)用于提供额外的保护。 Fastly基于云的WAF使用来自商业和开放源代码(如OWASP核心规则集)的第三方规则。 此外，还采用了特定于Adobe Commerce的规则。 保护客户免受关键应用程序层攻击，包括注入攻击和恶意输入攻击、跨站点脚本攻击、数据导出、HTTP协议违规和其他OWASP十大威胁。

WAF规则由Adobe Commerce更新，如果检测到新的漏洞，则允许Managed Services在软件修补程序之前“虚拟修补”安全问题。 Fastly WAF不提供速率限制或机器人检测服务。 如果需要，客户可以许可与Fastly兼容的第三方机器人检测服务。

请参阅[Web应用程序防火墙(WAF)](../cdn/fastly-waf-service.md)。

## 虚拟专用云

Adobe Commerce Pro计划生产环境配置为Virtual Private Cloud (VPC)，因此生产服务器是孤立的，进出云环境的能力有限。 只允许与云服务器的安全连接。 SFTP或rsync等安全协议可用于文件传输。

客户可以使用SSH隧道保护与应用程序的通信。 访问AWS PrivateLink需要支付额外费用。 与这些服务器的所有连接都使用AWS安全组进行控制，该虚拟防火墙限制与环境之间的连接。 客户的技术资源可以使用SSH访问这些服务器。

## 加密

Amazon Elastic Block Store (EBS)用于存储。 所有EBS卷都使用AES-256算法进行加密，这意味着数据在静止时加密。 该系统还加密CDN和源之间以及源服务器之间的传输中的数据。 客户密码以哈希形式存储。 敏感凭据（包括支付网关凭据）使用SHA-256算法进行加密。

当数据不处于静止状态或不在服务器之间传输时，Adobe Commerce应用程序不支持列或行级加密或加密。 客户可以从应用程序中管理加密密钥。 系统使用的密钥存储在AWS密钥管理系统中，必须由Managed Services管理才能提供部分服务。

## 端点检测和响应

[!DNL CrowdStrike Falcon]，轻量级下一代端点检测和响应(EDR)代理安装在Adobe内的所有端点（包括服务器）上。 EDR代理通过实时持续监控和收集来保护Adobe数据和系统，从而实现快速威胁识别和响应。

## 渗透测试

Managed Services会使用开箱即用的应用程序定期对Adobe Commerce云系统进行渗透测试。 客户负责其定制应用程序的任何渗透测试。

## 支付网关

Adobe Commerce需要支付网关集成，在这种情况下，信用卡数据将从消费者的浏览器直接传递到支付网关。 卡数据从不在任何Adobe Commerce Pro计划Managed Services环境中可用。 电子商务应用程序对交易的操作通过使用来自网关的对交易的引用而完成。

## Adobe Commerce应用程序

Adobe会定期测试核心应用程序代码是否存在安全漏洞。 为客户提供缺陷和安全问题的修补程序。 产品安全团队按照OWASP应用程序安全指南验证Adobe Commerce产品。 使用多种安全漏洞评估工具和外部供应商来测试和验证合规性。 安全工具包括：

- Veracode静态和动态扫描
- RIPS源代码扫描
- Trustwave和Alert Logic的漏洞扫描服务
- Burp Suite Pro
- OWASPZAP
- 和SqlMap

每两周会使用这些工具扫描整个代码库。 通过直接电子邮件、应用程序中的通知以及[安全中心](https://helpx.adobe.com/cn/security.html)中的通知，客户将收到有关安全修补程序的通知。

客户必须确保根据PCI准则在发布后30天内将这些修补程序应用到其定制应用程序中。 Adobe还提供[安全扫描工具](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/security/security-scan)，使商家能够定期监视其网站，并接收有关已知安全风险、恶意软件和未经授权的访问的更新。 安全扫描工具是一项免费服务，可在任何版本的Adobe Commerce上运行。

为了鼓励安全研究人员识别和报告漏洞，除了内部测试之外，Adobe Commerce还设立了[漏洞奖励计划](https://hackerone.com/magento)。 此外，如果需要，还向客户提供了应用程序的完整源代码以供他们自行审查。

## 只读文件系统

所有可执行代码都部署在只读文件系统映像中，这大大减少了可用于攻击的表面。 部署过程会创建一个Squash-FS映像，以减少将PHP或JavaScript代码注入系统或修改Adobe Commerce应用程序文件的机会。

## 远程部署

将可执行代码导入Managed Services生产环境的唯一方法是通过预配过程运行它。 预配包括将源代码从源存储库推送到启动部署过程的远程存储库。 对该部署目标的访问是受控的，因此您可以完全控制谁可以访问部署目标。 将应用程序代码部署到非生产环境的所有操作都由客户控制。

## 记录

所有AWS活动都登录到AWS CloudTrail。 操作系统、应用程序服务器和数据库日志存储在生产服务器上，并存储在备份中。 所有源代码更改都记录在Git存储库中。 部署历史记录在Adobe Commerce [Project Web界面](../project/overview.md)中可用。 所有支持访问都将被记录，支持会话也被记录。

查看[查看和管理日志](../test/log-locations.md)。

## 敏感数据

敏感数据既可能包含消费者的个人信息，也可能包含Managed Services客户的机密数据。 保护敏感的客户和消费者数据是Adobe Commerce Managed Services的一项重要任务。 Managed Services和Adobe客户都负有与个人身份信息有关的法律义务。 除了架构的安全功能外，还有其他控制来限制敏感数据的分发和访问。

客户拥有自己的数据，并控制该数据位于何处。 客户指定其生产和开发实例的存放位置。 此外，他们还指定用于Adobe Commerce报表环境与Commerce的位置，以及该Adobe Commerce报表应用程序是否有权访问个人身份信息。 生产实例可以位于大多数AWS区域，而开发环境和Adobe Commerce报表环境当前可以在美国或欧盟找到。

敏感数据可能通过Fastly CDN服务器网络，但不会存储在Fastly网络中。 Managed Services产品中的所有合作伙伴都有合同义务确保敏感数据的保护。 Managed Services不会将敏感的客户或消费者数据从客户指定的位置移动。

作为提供Managed Services服务中包含的服务的一部分，Managed Services员工需要访问包含敏感数据的生产系统。 这些员工接受保护敏感客户和消费者数据的培训。 只有需要访问的员工才能访问这些系统。 这些员工只能访问提供这些服务所需的时间。

## 通用数据保护条例

《通用数据保护条例》(GDPR)是一个法律框架，为欧盟(EU)境内个人的个人信息收集和处理设置了准则。 无论站点位于何处，欧盟访客都可以访问站点，这些法规均适用。

必须通知访客网站从他们那里收集的数据，并明确同意收集信息。 如果网站持有的个人数据被破坏，网站必须通知访客。

运营站点的商家或公司必须拥有专门的数据保护人员，负责监督站点的数据安全性。 如果访客请求删除其数据，则应联系数据隐私官员（或网站管理团队）。

GDPR要求收集的任何个人身份信息（例如姓名、种族和出生日期）进行匿名化或匿名化。

>[!NOTE]
>
>本页概述了有关GDPR的考虑事项。 有关Adobe Commerce如何存储个人信息的详细信息，请参阅&#x200B;_[安全和合规性指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/security-and-compliance/overview)_。 要确定您的企业应如何遵守任何法律义务，请咨询您的法律顾问或参阅[正式文本](https://eur-lex.europa.eu/eli/reg/2016/679/oj)。

## 备份

在过去24小时的操作中，每小时执行一次备份。 在24小时之后，将使用AWS EBS快照服务按计划保留备份。 请参阅[保留策略](pro-architecture.md#retention-policy)。

该服务在冗余存储上创建独立的备份。 由于EBS卷是加密的，因此备份也是加密的。 此外，Managed Services还根据客户请求执行按需备份。

您的Managed Services备份和恢复方法使用与完整系统备份相结合的高可用性体系结构。 每个项目（所有数据、代码和资产）都将在三个单独的AWS可用区中进行复制；每个区都有一个单独的数据中心。

请参阅[快照和备份管理](../storage/snapshots.md)。
