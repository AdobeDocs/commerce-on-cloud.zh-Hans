---
title: Variables属性
description: 使用variables属性自定义 [!DNL Commerce] 应用程序的存储配置选项。
feature: Cloud, Configuration
exl-id: 9909d4a1-d9c8-492b-a5cc-cfbf17b5b7a8
TQID: https://experienceleague.adobe.com/bNdcqNxAAE1E1Qe2C-y1LWpFX-8Kzuwc9rJYEnbRz0U
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 85
ht-degree: 0%

---

# Variables属性

您可以使用基于应用程序的环境变量来自定义存储配置。 这些变量使用特定的语法。 请参阅&#x200B;_配置指南_&#x200B;中的[覆盖配置设置](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html)。

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

