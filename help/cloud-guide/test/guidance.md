---
title: 测试指南
description: 了解在云基础架构上启动Adobe Commerce的测试类型和最佳实践。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 测试指南

在云基础架构项目上配置和自定义Adobe Commerce后，最佳实践是在启动商店网站之前彻底测试您的应用程序。 测试有助于正确管理对群集规模的期望，并根据未来的业务需求进行适当扩展。

## 功能测试

在开发过程中，请务必对云基础架构项目上的Adobe Commerce执行端到端功能测试。 有关在Docker环境中执行功能测试，请参阅以下指南：

- **应用程序测试** — 使用[Magento功能测试框架(MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/)在Cloud Docker环境中测试应用程序。

- **代码测试** — 使用[PHP的代码欺骗测试框架](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/)验证用于贡献到Cloud包存储库的代码。

## 启动前的最佳实践

将以下测试类型视为在站点启动之前执行的最佳实践：

- **负载测试** — 执行负载测试以了解系统在预期负载下的行为。 例如，测试应用程序上并发的活动用户数，让每个用户在设置的持续时间内执行特定数量的事务。 此测试揭示重要业务关键事务（如数据库或应用程序服务器行为）的响应时间。 负载测试有助于确定瓶颈。

- **压力测试** — 询问系统内的容量上限，以确定在当前负载大大超过预期最大值时，系统是否能够充分执行。

- **安全扫描**—Adobe为您的站点提供免费的[安全扫描工具](../launch/overview.md#set-up-the-security-scan-tool)。

- **渗透测试** — 是计算机系统上的授权模拟网络攻击，旨在评估系统的安全性。 渗透测试有助于识别弱点或漏洞，包括未经授权的各方获取系统功能和数据的可能性。

>[!WARNING]
>
>客户不得对AWS基础设施或AWS服务本身进行任何安全评估。 如果在安全评估中发现的任何AWS服务中存在安全问题，请立即[联系AWS安全部门](mailto:aws-security@amazon.com)。 查看[AWS客户渗透测试政策](https://aws.amazon.com/security/penetration-testing/)。
