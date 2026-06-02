---
title: ECE工具包的错误消息
description: 查看在Adobe Commerce上云基础架构的构建、部署和部署后过程中可能发生的错误代码和消息的列表。
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
TQID: https://experienceleague.adobe.com/FpWmyt0POi3RgKQOf6ihQQVV6GazxRByu-4HdPIhTPY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 253
ht-degree: 0%

---

# ECE工具的错误消息

此错误消息参考提供了在Adobe Commerce上云基础架构的构建、部署和部署后过程中可能发生的错误疑难解答信息。

部署期间发生的所有严重错误消息和警告错误消息都会写入`var/log/cloud.log`和`/var/log/cloud.error.log`文件。 云错误日志文件仅包含来自最新部署的错误。 空文件表示部署成功，并且没有错误。

在`cloud.error.log`文件中，每个条目均采用JSON字符串的格式，以便于分析：

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

错误消息按部署阶段之一进行分类：生成、部署和部署后。 每个部分都提供一个关联错误的列表，其中包含每个错误的以下信息：

- **错误代码**：错误消息的Adobe Commerce分配的标识符
- **阶段**：指示在生成、部署或部署后阶段期间是否出现错误
- **步骤**：指示部署方案中可能返回错误的步骤。 如果&#x200B;_Step_&#x200B;列为空，则该错误是可通过多个步骤返回或在预处理操作期间返回的常规错误。 有关生成、部署和部署后步骤的详细信息，请参阅[基于方案的部署](../deploy/scenario-based.md)。
- **建议**：提供排除故障和解决错误的指导
- **标题（错误描述）**：总结错误原因的描述
- **类型**：指示错误是严重错误还是警告

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
