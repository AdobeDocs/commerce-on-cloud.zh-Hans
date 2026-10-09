---
title: 设置静态文件的缓存
description: 了解如何在[!DNL Commerce]应用程序配置文件中设置缓存存储选项。
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: 'https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%
---
# 设置静态文件的缓存

在`.magento.app.yaml`配置文件中使用`expires`键设置媒体和静态文件的缓存TTL（生存时间）。

>[!NOTE]
>
>在更新生产环境之前，请务必在暂存环境中测试更改。 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/zh-hans/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)，以获得在这些环境中更新配置的帮助。

1. 在`.magento.app.yaml`文件的[`web`属性](web-property.md)中指定TTL时间（秒）。 您可以在`locations`下或在`"/media"`和`"/static"`下添加`expires`键。

   要防止缓存过期，请使用`expires: -1`键值对。 请参阅以下示例：

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```

