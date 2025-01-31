---
title: 保护块
description: 了解Adobe Commerce在云基础架构上的保护性阻止功能，以及它如何保护您的站点免受已知安全漏洞的攻击。
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# 保护块

云基础架构上的Adobe Commerce具有保护性阻止功能，可在某些情况下限制对具有安全漏洞的网站的访问。 这种部分阻止方法可防止利用已知的安全漏洞。 过时的软件通常包含漏洞，因此通过部分阻止对这些站点的访问来防御这些漏洞是非常重要的。

## 保护块如何工作

Adobe Commerce维护一个开放源码软件中已知安全漏洞签名的数据库，这些软件通常部署在云基础架构上。 安全检查仅分析开源项目中的已知漏洞；它无法检查您编写的自定义项。

安全扫描运行：

- 在将新代码推送到Git时
- 当数据库添加了新漏洞时

如果在应用程序中检测到严重漏洞，它会拒绝Git推送。

有两种类型的块运行：

1. **完成块** — 用于开发网站。 `git push`随附的错误消息提供了有关漏洞的详细信息。

1. **部分块** — 用于生产网站，允许网站保持大部分联机。 根据漏洞的性质，可能会从GET请求中删除请求的各个部分，例如查询字符串、Cookie或任何其他标头。 所有其他请求可能会被完全阻止，例如登录、表单提交或产品结账。

一旦解决了安全风险，就会自动取消阻止。 应用安全升级后不久就会删除块，该安全升级会删除漏洞。

## 选择退出保护块

该保护块旨在保护您免受在云基础架构上部署Adobe Commerce的软件中的已知漏洞的攻击。

但是，您可以通过向[`.magento.app.yaml`](../application/configure-app-yaml.md)添加以下内容来选择退出保护块：

```yaml
   preflight:
      enabled: false
```

例如，您可以明确选择退出某个特定的检查：

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
