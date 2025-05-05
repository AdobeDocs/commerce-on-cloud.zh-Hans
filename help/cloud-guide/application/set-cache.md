---
title: 设置静态文件的缓存
description: 了解如何在 [!DNL Commerce] 应用程序配置文件中设置缓存存储选项。
feature: Cloud, Configuration, Cache, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# 设置静态文件的缓存

在`.magento.app.yaml`配置文件中使用`expires`键设置媒体和静态文件的缓存TTL（生存时间）。

>[!NOTE]
>
>在更新生产环境之前，请务必在暂存环境中测试更改。 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)，以获得在这些环境中更新配置的帮助。

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
