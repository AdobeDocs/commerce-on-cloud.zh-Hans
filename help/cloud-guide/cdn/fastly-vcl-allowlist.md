---
title: 用于允许请求的自定义VCL
description: 使用Fastly Edge ACL列表和自定义VCL代码片段，过滤传入请求并允许按IP地址访问Adobe Commerce站点。
feature: Cloud, Configuration, Security
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# 用于允许请求的自定义VCL

您可以使用带有自定义VCL代码片段的Fastly Edge ACL列表过滤传入的请求并允许按IP地址访问。 ACL列表指定要允许的IP地址。

创建一个允许列表以限制对暂存环境的访问，以便只允许内部开发人员和已批准的外部服务来自指定IP地址的请求。 您还可以创建一个允许列表，以确保对暂存环境和生产环境管理员的访问权限。

以下示例显示如何使用带[Fastly访问控制列表(ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls)的自定义VCL代码片段来保护对云基础架构项目环境上的Adobe Commerce管理员的访问。 将自定义VCL代码段添加到云环境时，Fastly仅允许来自ACL中包含的IP地址的请求。

>[!TIP]
>
>对于不应公开访问的暂存和集成环境，请使用[[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface)中提供的HTTP访问控制选项按IP地址管理对整个站点的访问。

**先决条件：**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要包含在允许列表中的客户端IP地址列表

## 创建Edge ACL以允许客户端IP地址

Edge ACL创建IP地址列表来管理对站点的访问。 在此示例中，您创建了一个Edge ACL，并添加了一个允许访问项目环境管理员的客户端IP地址列表。

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**。

1. 展开&#x200B;**全页缓存** > **Fastly配置** > **Edge ACL**。

1. 创建ACL容器：

   - 单击&#x200B;**添加ACL**。

   - 在&#x200B;*ACL容器*&#x200B;页面上，输入&#x200B;**ACL名称**—`allowlist`。

   - 选择&#x200B;**更改后激活**&#x200B;以将更改部署到您正在编辑的Fastly服务配置版本。

   - 单击&#x200B;**上传**&#x200B;将ACL附加到您的Fastly服务配置。

1. 添加允许访问管理员的IP地址列表：

   - 单击`allowlist` ACL的设置图标。

   - 为每个客户端IP地址添加并保存&#x200B;*IP值*。

   - 单击&#x200B;**取消**&#x200B;返回系统配置页。

1. 单击&#x200B;**保存配置**。

1. 根据页面顶部的通知刷新缓存。

## 创建自定义VCL代码片段以保护管理员访问权限

以下自定义VCL代码片段（JSON格式）显示了逻辑，用于筛选发送给管理员的请求，并在客户端IP地址与`allowlist` ACL中的地址匹配时允许访问。

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

在此示例中[创建自定义代码片段](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html?lang=zh-Hans#add-the-custom-vcl-snippet)之前，请查看值以确定是否需要进行任何更改。 然后在相应的字段中输入每个值，例如在“类型”字段中输入值`type`，在“内容”字段中输入值`content`。

- `name` — VCL代码片段的名称。 对于此示例，`allowlist`。

- `priority` — 确定VCL代码片段的运行时间。 优先级为`5`立即运行并检查管理员请求是否来自允许的IP地址。 该代码片段在任何默认MagentoVCL代码片段(`magentomodule_*`)的优先级为50之前运行。 根据您希望代码片段运行的时间，将每个自定义代码片段的优先级设置为高于或低于50。 优先级较低的代码片段首先运行。

- `type` — 指定在版本化VCL代码中插入代码片段的位置。 此VCL是`recv`代码片段类型，它将代码片段添加到默认Fastly VCL代码下方的`vcl_recv`子例程中以及任何对象的上方。

- `content` — 要运行的VCL代码段。 在此示例中，代码将过滤对管理员的请求，并允许在客户端IP地址与`allowlist` ACL中的地址匹配时进行访问。 如果地址不匹配，请求将被`403 Forbidden`错误阻止。

  如果管理员的URL已更改，请将示例值`/admin`替换为环境的URL。 例如，`/company-admin`。

在代码示例中，使用[原始屏蔽](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)时，条件`!req.http.Fastly-FF`很重要。 请勿删除或编辑此代码。

查看并更新环境的代码后，使用以下任一方法将自定义VCL代码段添加到Fastly服务配置中：

- [从Admin](#add-the-custom-vcl-snippet)添加自定义VCL代码片段。 如果您可以访问管理员，则建议使用此方法。 (需要Magento2版本1.2.58[&#128279;](fastly-configuration.md#upgrade)或更高版本的Fastly CDN模块。)

- 将JSON代码示例保存到文件（例如，`allowlist.json`）中，然后[使用Fastly API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)上载它。 如果您无法访问管理员，请使用此方法。

## 添加自定义VCL代码片段

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**。

1. 展开&#x200B;**全页缓存** > **Fastly配置** > **自定义VCL代码片段**。

1. 单击&#x200B;**创建自定义代码片段**。

1. 添加VCL代码片段值：

   - **名称** — `allowlist`

   - **类型** — `recv`

   - **优先级** — `5`

   - 添加&#x200B;**VCL**&#x200B;代码片段内容：

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. 单击&#x200B;**创建**&#x200B;以生成名称模式为`type_priority_name.vcl`的VCL代码片段文件，例如`recv_5_allowlist.vcl`

1. 重新加载页面后，单击&#x200B;*Fastly配置*&#x200B;部分中的&#x200B;**将VCL上传到Fastly**&#x200B;以将文件添加到Fastly服务配置。

1. 上载完成后，根据页面顶部的通知刷新缓存。

Fastly在上传过程中验证VCL代码的更新版本。 如果验证失败，请编辑自定义VCL代码片段以解决该问题。 然后，再次上传VCL。

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
