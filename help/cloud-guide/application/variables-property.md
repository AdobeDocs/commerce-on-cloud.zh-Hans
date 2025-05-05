---
title: Variables属性
description: 使用variables属性自定义 [!DNL Commerce] 应用程序的存储配置选项。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Variables属性

您可以使用基于应用程序的环境变量来自定义存储配置。 这些变量使用特定的语法。 请参阅&#x200B;_配置指南_&#x200B;中的[覆盖配置设置](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=zh-Hans)。

`.magento.app.yaml`文件中包含的以下环境变量对于[!DNL Commerce]应用程序的特定版本是必需的。

对于Adobe Commerce 2.2.x到2.3.x，需要执行以下操作：

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

对于Adobe Commerce 2.4.x，请设置以下变量：

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
