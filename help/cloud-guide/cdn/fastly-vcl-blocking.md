---
title: 用于阻止请求的自定义VCL
description: 使用带有自定义VCL代码片段的Edge访问控制列表(ACL)，按IP地址阻止传入请求。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# 用于阻止请求的自定义VCL

您可以使用适用于Magento2的Fastly CDN模块创建一个包含要阻止的IP地址列表的Edge ACL。 然后，您可以将该列表与VCL代码段一起使用来阻止传入的请求。 该代码检查传入请求的IP地址。 如果与ACL列表中包含的IP地址匹配，Fastly将阻止请求访问您的站点并返回`403 Forbidden error`。 允许访问所有其他客户端IP地址。

**先决条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要阻止的客户端IP地址列表

## 创建Edge ACL以阻止客户端IP地址

您可以创建一个Edge ACL来定义要阻止的IP地址列表。 创建ACL后，您可以在自定义VCL代码片段中使用它来管理对暂存或生产站点的访问。

通过在两个环境中创建同名的Edge ACL来管理暂存和生产站点的访问权限。 VCL代码片段代码适用于这两个环境。

1. 登录到管理员。
1. 导航到&#x200B;**商店** >设置> **配置** > **高级** > **系统** > **全页缓存** > **快速配置**。
1. 展开&#x200B;**Edge ACL**&#x200B;部分。
1. 单击&#x200B;**添加ACL**&#x200B;以创建列表。 列入阻止列表在此示例中，将列表命名为“”。
1. 在列表中输入IP地址值。 添加到此列表的任何客户端IP地址都将被阻止，并且无法访问该站点。
1. 如果需要，可选中&#x200B;**否定**&#x200B;复选框。

在VCL代码片段中按名称引用Edge ACL。

## 列入阻止列表为创建自定义VCL

>[!NOTE]
>
>此示例向高级用户展示如何创建VCL代码段来配置自定义阻止规则以上载到Fastly服务。 列入阻止列表 列入允许列表您可以使用Fastly CDN中针对Magento2模块提供的[阻止](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)功能，根据Adobe Commerce管理员的国家/地区配置或。

定义Edge ACL后，可以使用它创建VCL代码片段，以阻止对ACL中指定的IP地址的访问。 您可以在暂存环境和生产环境中使用相同的VCL代码片段，但必须分别将该代码片段上传到每个环境。

列入阻止列表以下自定义VCL代码片段（JSON格式）显示了使用与ACL中的地址匹配的客户端IP地址阻止传入请求的逻辑。

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

在基于此示例创建代码片段之前，请查看值以确定是否需要进行任何更改：

- `name`： VCL代码片段的名称。 在此示例中，我们使用了名称`blocklist`。

- `priority`：确定VCL代码片段的运行时间。 优先级为`5`以立即运行并检查管理员请求是否来自允许的IP地址。 该代码片段在任何默认MagentoVCL代码片段(`magentomodule_*`)的优先级为50之前运行。 根据您希望代码片段运行的时间，将每个自定义代码片段的优先级设置为高于或低于50。 优先级较低的代码片段首先运行。

- `type`：指定VCL代码片段的类型，以确定代码片段在生成的VCL代码中的位置。 在本例中，我们使用`recv`，它将VCL代码插入`vcl_recv`子例程中、样板VCL的下方和任何对象的上方。 有关代码片段类型的列表，请参阅[Fastly VCL代码片段引用](https://docs.fastly.com/api/config#api-section-snippet)。

- `content`：要运行的VCL代码片段，用于检查客户端IP地址。 如果IP位于Edge ACL中，则会阻止其访问，并显示整个网站的`403 Forbidden`错误。 允许访问所有其他客户端IP地址。

查看并更新环境的代码后，使用以下任一方法将自定义VCL代码段添加到Fastly服务配置中：

- [从Admin](#add-the-custom-vcl-snippet)添加自定义VCL代码片段。 如果您可以访问管理员，则建议使用此方法。 （需要[Fastly版本1.2.58](fastly-configuration.md#upgrade-fastly-module)或更高版本。）

- 将JSON代码示例保存到文件（例如，`blocklist.json`）中，然后[使用Fastly API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)上载它。 如果您无法访问管理员，请使用此方法。

## 添加自定义VCL代码片段

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**。

1. 展开&#x200B;**全页缓存** > **Fastly配置** > **自定义VCL代码片段**。

1. 单击&#x200B;**创建自定义代码片段**。

1. 添加VCL代码片段值：

   - **名称** — `blocklist`

   - **类型** — `recv`

   - **优先级** — `5`

   - 添加&#x200B;**VCL**&#x200B;代码片段内容：

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. 单击&#x200B;**创建**&#x200B;以生成名称模式为`type_priority_name.vcl`的VCL代码片段文件，例如`recv_5_blocklist.vcl`

1. 重新加载页面后，单击&#x200B;*Fastly配置*&#x200B;部分中的&#x200B;**将VCL上传到Fastly**&#x200B;以将文件添加到Fastly服务配置。

1. 上传后，根据页面顶部的通知刷新缓存。

Fastly在上传过程中验证VCL代码的更新版本。 如果验证失败，请编辑自定义VCL代码片段以解决该问题。 然后，再次上传VCL。

## 阻塞请求的其他VCL示例

以下示例显示如何使用内联条件语句而不是ACL列表来阻止请求。

>[!WARNING]
>
>在这些示例中，VCL代码的格式为JSON有效负荷，该有效负荷可以保存到文件中并在Fastly API请求中提交。 您可以从Admin](#add-the-custom-vcl-snippet)提交[VCL代码片段，或使用Fastly API作为JSON字符串提交。 要防止在将Fastly API与JSON字符串一起使用时发生验证错误，必须使用反斜杠对特殊字符进行转义。

>[!NOTE]
>如果要从Admin提交VCL代码片段，请从示例VCL代码中提取各个值，并将它们输入到相应的字段中。 例如：
>- 名称： `<name of the VCL>`
>- 动态： `<0/1>`
>- 类型： `<type>`
>- 优先级：`<priority>`
>- 内容： `<content>`

请参阅Fastly VCL文档中的[使用动态VCL代码片段](https://docs.fastly.com/vcl/vcl-snippets/)。

### VCL代码示例：按国家/地区代码分块

此示例使用双字符ISO 3166-1国家/地区代码来表示与IP地址关联的国家/地区。

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>您可以使用云基础架构管理员上的Adobe Commerce中的Fastly [Blocking](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)功能来按国家/地区代码或国家/地区代码列表配置阻止，而不使用自定义VCL代码片段。

### VCL代码示例： Block by HTTP User-Agent请求标头

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
