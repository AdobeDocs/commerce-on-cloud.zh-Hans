---
title: 自定义VCL以绕过Fastly缓存
description: 通过创建自定义VCL代码片段以绕过Fastly缓存，对到源服务器的请求流量进行故障诊断。
feature: Cloud, Configuration, Cache
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 自定义VCL以绕过Fastly缓存

您可以创建一个自定义VCL代码段来绕过Fastly缓存，这样您就可以对到源服务器的请求流量进行故障排除。 例如，您可以创建一个代码片段以确定网站问题是由缓存还是标头疑难解答引起的。

您可以配置此代码片段，以绕过针对来自特定IP地址或URL的请求的快速缓存。

>[!NOTE]
>
>将自定义VCL配置合并到生产环境之前，请确保在暂存环境中测试代码。

**先决条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**根据IP地址或URL绕过Fastly缓存**：

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**。

1. 展开&#x200B;**全页缓存** > **Fastly配置** > **自定义VCL代码片段**。

1. 单击&#x200B;**创建自定义代码片段**。

1. 添加VCL代码片段值：

   - **名称** — `bypass_fastly`

   - **类型** — `recv`

   - **优先级** — `5`

   - **VCL**&#x200B;代码片段内容 — 

     以下示例绕过Fastly获取特定IP地址：

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     以下示例绕过特定URL模式的Fastly：

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     要获得精确的URL匹配项，请使用`==`运算符而不是`~`运算符。 有关详细信息，请参阅[Fastly VCL引用]。

1. 单击&#x200B;**创建**。

   ![创建Fastly Bypass VCL代码片段](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. 重新加载页面后，在&#x200B;*Fastly配置*&#x200B;部分中单击&#x200B;**将VCL上传到Fastly**。

1. 上载完成后，根据页面顶部的通知刷新缓存。

   Fastly在上传过程中验证更新的VCL版本。 如果验证失败，请编辑自定义VCL代码片段以修复所有问题。 然后，再次上传VCL。

添加VCL代码段后，可以使用cURL命令将来自指定IP地址或URL的请求提交到源服务器，如以下示例所示：

```bash
curl -svo /dev/null www.example.com/index.html
```

然后，检查响应以排除未缓存内容的问题。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL引用]: https://docs.fastly.com/vcl/
