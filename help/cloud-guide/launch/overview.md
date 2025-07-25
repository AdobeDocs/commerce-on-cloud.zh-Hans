---
title: 站点启动
description: 了解如何开始站点启动准备工作。
exl-id: 95abc7aa-ed4d-44f7-96aa-517c646bc00d
source-git-commit: 38ac38d4edd0f317155d0d4537021a29a21d5761
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# 站点启动

在集成和暂存环境中完成部署和测试后，即可开始站点启动准备工作。 首先，您应该先完成所有开发和测试，然后再在生产环境中工作。 准备好发射了吗？ 查看清单、最佳实践和启动网站的最终步骤。

如果您在部署和测试暂存环境之前查看了此信息，请考虑先在下一部分中查看在暂存环境中测试的好处。 暂存是一种运行在相似硬件、配置、架构和服务上的准生产环境。 它可以减少停机时间，使您的扩展、服务、自定义配置和商家用户验收测试重要组件发布您的网站和商店。

## 为何要在集成、暂存和生产环境中进行全面测试？

我们强烈建议在集成、暂存和生产环境中进行测试，因为要确保您的自定义代码、主题、扩展和第三方集成全部协作以运行存储，这项工作非常复杂。 以下是您可以在更新生产环境之前在集成和暂存环境中完成测试时发现并解决的常见问题：

- 暂存支持所有生产服务、功能、数据库数据、技术栈栈、体系结构等。 它镜像生产，这意味着，如果在暂存环境中发生错误，则在生产环境中发生错误之前，您将收到警告。

- 在本地集成环境中工作的代码在暂存环境和生产环境中可能无法工作。

- 集成环境不支持在暂存和生产环境中提供的某些服务，例如Fastly和New Relic。

- [使用暂存中的各种工具全面测试](../test/guidance.md)您的站点，评估负载、压力、性能和站点资源。

- 由于集成环境可能仅填充了测试数据的数据库，与类似生产的环境不匹配，因此，在暂存或生产环境中进行测试时，您可能会发现其他错误或意外行为。

## 站点启动先决条件

您需要以下信息和资源来准备站点启动：

- 用于配置DNS的CNAME记录信息

- 要添加到证书的所有店面域的列表

- SSL/TLS证书

作为Adobe Commerce云基础架构订阅的一部分，Adobe提供了由让我们加密颁发的域验证的SSL/TLS证书。 每个Pro Production、Staging和Starter Production (`master`)环境都有一个唯一的证书，该证书涵盖了该环境中的所有域和子域。 这些证书会在您更新用于开发和生产的DNS配置后自动配置并上传到您的站点。 请参阅[预配SSL/TLS证书](../cdn/fastly-configuration.md#provision-ssltls-certificates)。

>[!NOTE]
>
>如果您要为公司部署自己的扩展验证SSL证书，而不使用Let&#39;s Encrypt证书，请联系您的CTA或[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)。

## 设置安全扫描工具

>[!NOTE]
>
>安全扫描工具使用以下公共IP地址：
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>将这些IP地址添加到网络防火墙规则中的允许列表，以允许该工具扫描您的站点。 该工具仅向端口80和443发送请求。

安全扫描工具可让您定期监视商店网站，并接收已知安全风险、恶意软件和过期软件的更新。 此工具是一项免费服务，适用于云基础架构上的所有实施和版本的Adobe Commerce。 您可通过您的[Commerce Marketplace帐户](https://account.magento.com/customer/account/login)访问该工具。

- 监视站点安全状态和应用的安全更新

- 接收安全更新和特定于站点的通知

>[!NOTE]
>
>Adobe建议使用安全扫描工具来取代其他第三方工具，以确保在调查结果调查期间提供最佳服务质量。

有关设置和使用安全扫描工具的信息，请参阅[用户指南](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/security/security-scan)。 通常，在开始用户验收测试(UAT)时即开始使用此工具。

您扫描的每个站点都必须通过“安全扫描”选项卡进行注册。 在注册过程中，您必须接受免责声明，然后才能开始扫描。 您可以控制计划，并授权用户在每次扫描完成时接收通知。 您可以计划特定循环日期和时间的扫描，也可以根据需要运行扫描。

安全扫描工具使用多个用户代理字符串来模拟实际的恶意软件活动。 您可能会在Analytics或访问日志中看到以下用户代理：

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## 扫描您的网站

1. 访问您的[Commerce Marketplace帐户](https://account.magento.com/customer/account/login)。

1. 单击“安全扫描”选项卡，然后选择&#x200B;**转到安全扫描**。

1. 在站点的&#x200B;_操作_&#x200B;列中，选择&#x200B;**运行扫描**。 通知状态将显示计划的扫描。

### 要复查报告，请执行以下操作：

1. 报告完成后，将显示通知。

1. 在网站行中，从&#x200B;**报告**&#x200B;列中选择要查看的报告。 顺序从最新到最旧。

报告列出了问题，包括扫描失败、未识别结果和成功扫描。 每个条目都提供了扫描的详细信息、要调查的问题列表以及要采取的操作。 其中一些操作可能需要下载和安装安全修补程序。 将所需的任何修补程序添加到本地工作站上的开发分支中，然后再将其添加到生产分支。

扫描结果包括一个标签，用于描述扫描通过或失败状态以及所执行检查的详细信息：

- “失败”表示网站包含严重漏洞。

- “未识别”表示您的团队或托管提供商需要更深入的审查，以确定是否需要执行进一步操作。

扫描结果还为每个失败的安全测试提供了建议的修复步骤。 安全扫描结果受到保护，只有注册的用户才能查看。 只有站点注册过程中指定的用户才会收到扫描完成通知。

## 准备启动您的站点

当您准备好开始站点启动流程时，请参阅以下内容：

- [启动项核对清单](checklist.md)

- [启动步骤](steps.md)
