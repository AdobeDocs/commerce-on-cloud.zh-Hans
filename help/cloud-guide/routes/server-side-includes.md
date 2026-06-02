---
title: 服务器端包括
description: 了解如何在云基础架构上将服务器端包含与Adobe Commerce结合使用。
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# 服务器端包括

[Server-side include](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI)是HTML页面中的指令，在页面呈现时在服务器上进行计算。 通过SSI，您可以将动态生成的内容添加到现有HTML页面，而无需提供整个页面。

您可以在`.magento/routes.yaml`中按每个路由激活或停用SSI；例如：

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI允许您在HTML响应指令中包含导致服务器填写HTML的部分内容，并遵循任何现有的[缓存配置](caching.md)。

以下示例显示如何在页面顶部插入动态日期控件，在底部插入每600秒更新一次的日期控件：

将以下内容添加到任何页面，如`/index.php`：

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

将以下内容添加到`time.php`：

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

浏览到添加控件的页面。 刷新页面几次，并注意页面顶部的时间会更改，而底部的时间每600秒更改一次。
