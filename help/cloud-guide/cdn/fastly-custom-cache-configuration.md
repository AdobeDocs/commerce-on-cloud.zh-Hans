---
title: 自定义缓存配置
description: 了解如何在Fastly服务设置完成后查看和自定义缓存配置设置。
feature: Cloud, Configuration, Iaas, Cache
exl-id: f6901931-7b3f-40a8-9514-168c6243cc43
source-git-commit: eaa9980c437a9398f0d20d3c27832aecffc78fd9
workflow-type: tm+mt
source-wordcount: '1898'
ht-degree: 0%

---

# 自定义缓存配置

在暂存环境和生产环境中设置和测试Fastly服务后，查看和自定义缓存配置设置。 例如，您可以更新设置以允许TLS将HTTP请求重定向到Fastly，更新清除设置以及启用基本身份验证以在开发期间对您的网站进行密码保护。

以下部分提供了配置某些缓存设置的概述和说明。 在[Fastly CDN Module for Magento 2](https://github.com/fastly/fastly-magento2/tree/master/Documentation)文档中查找有关可用配置选项的其他信息。

## 强制TLS

Fastly提供了用于将未加密请求(HTTP)重定向到Fastly的&#x200B;_强制TLS_&#x200B;选项。 为您的暂存或生产环境配置了[有效的SSL/TLS证书](fastly-configuration.md#provision-ssltls-certificates)后，您可以更新存储的Fastly配置以启用“强制TLS”选项。 请参阅Magento 2 _的_ Fastly CDN模块中的Fastly [Force TLS指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md)文档。

>[!NOTE]
>
>对于云基础架构存储上的Adobe Commerce，建议启用强制TLS选项。

## 扩展Fastly超时

Fastly服务配置为发送给管理员的HTTPS请求指定180秒的默认超时时段。 任何超出超时期间的请求处理都会返回503错误。 因此，在响应需要较长时间处理的请求或尝试执行批量操作时，您可能会收到503错误。

要完成耗时超过3分钟的批量操作，请更改&#x200B;_管理员路径超时_ value_以防止503错误。

>[!NOTE]
>
>如果您已在&#x200B;**商店** > **配置** > **高级** > **管理员** > **管理员基础URL**&#x200B;中的&#x200B;**自定义管理路径**&#x200B;字段中指定了自定义管理路径终结点，则还需要将该环境中的[ADMIN_URL变量](../environment/variables-admin.md#change-the-admin-url)设置为相同的值。 如果设置不同，则超时不起作用。
>
>要扩展Fastly UI中除管理员以外的Fastly超时参数，请参阅[增加长作业的超时](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md)。

**延长管理员的Fastly超时**：

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**，然后展开&#x200B;**全页缓存**。

1. 在&#x200B;_快速配置_&#x200B;部分中，展开&#x200B;**高级配置**。

1. 设置&#x200B;**管理员路径超时**&#x200B;值（以秒为单位）。 此值不能超过10分钟（600秒）。

>[!NOTE]
>
>**_管理员路径超时_**&#x200B;配置设置不控制Adobe Commerce之外的超时值，如Fastly WAF超时。 要调整Fastly WAF超时值，您必须打开Adobe支持票证以在Fastly服务中更新它。

1. 单击页面顶部的&#x200B;**保存配置**。

1. 重新加载页面后，在&#x200B;_Fastly配置_&#x200B;部分中选择&#x200B;**将VCL上传到Fastly**。

Fastly检索用于从`app/etc/env.php`配置文件生成VCL文件的管理员路径。

## 配置清除选项

Fastly在Magento缓存管理页面上提供了多种类型的清除选项，包括用于清除产品类别、产品资源和内容的选项。 启用后，Fastly会监视事件以自动清除这些缓存。 如果禁用清除选项，则可以在通过“高速缓存管理”页完成更新后手动清除快速高速缓存。

清除选项包括：

- **清除类别** — 在添加和更新单个产品时清除产品类别内容（不是产品内容）。 您可能希望将此项保持为禁用状态并启用清除产品，这将清除产品和产品类别。
- **清除产品** — 在保存对产品的单个修改时，清除所有产品和产品类别内容。 启用清除产品有助于在更改价格、添加产品选项以及产品库存缺货时立即向客户获取更新。
- **清除CMS页面** — 在更新页面并将页面添加到Adobe Commerce CMS时清除页面内容。 例如，在更新条款和条件或退货策略时，您可能要清除该项。 如果您很少进行这些更改，则可以禁用自动清除。
- **软清除** — 根据过时时间将内容更改为过时并清除。 除了过时的时间安排外，Fastly还会向客户提供过时的内容，并在后台更新内容。

![配置清除选项](../../assets/cdn/fastly-purge-options.png)

**配置Fastly清除选项**：

1. 在&#x200B;_Fastly配置_&#x200B;部分中，展开&#x200B;**高级配置**&#x200B;以显示清除选项。

1. 对于每个清除选项，选择&#x200B;**是**&#x200B;以启用自动清除，或选择&#x200B;**否**&#x200B;以禁用自动清除。

   禁用清除选项时，必须从&#x200B;_缓存管理_&#x200B;页面手动清除该类别的缓存。

1. 单击页面顶部的&#x200B;**保存配置**。

1. 重新加载页面后，在&#x200B;_Fastly配置_&#x200B;部分中选择&#x200B;**将VCL上传到Fastly**。

有关详细信息，请参阅[Fastly配置选项](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options)。

## 配置GeoIP处理

Fastly模块包括GeoIP处理，用于自动重定向访客或提供与所获取的国家代码匹配的商店列表。 如果您已经使用扩展进行GeoIP处理，则可能需要使用Fastly选项验证功能。

**要设置GeoIp处理**：

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**，然后展开&#x200B;**全页缓存**。

1. 在&#x200B;_快速配置_&#x200B;部分中，展开&#x200B;**高级配置**。

1. 向下滚动并选择&#x200B;**是**&#x200B;到&#x200B;**启用GeoIP**。 将显示其他配置选项。

1. 对于GeoIP操作，选择是使用&#x200B;**重定向**&#x200B;自动重定向访客，还是提供要从&#x200B;**对话框**&#x200B;中选择的商店列表。

1. 对于&#x200B;**国家/地区映射**，选择&#x200B;**添加**&#x200B;以输入两个字母的国家代码，以便与列表中的特定Adobe Commerce商店进行映射。

   ![添加GeoIP国家/地区地图](/help/assets/cdn/fastly-geo-code.png)

1. 单击页面顶部的&#x200B;**保存配置**。

1. 重新加载页面后，在&#x200B;_Fastly配置_&#x200B;部分中选择&#x200B;**将VCL上传到Fastly**。

>[!NOTE]
>
>当前的Adobe Commerce Fastly GeoIP模块实施不支持多个网站之间的重定向。

Fastly还为自定义地理位置编码提供了一系列[与地理位置相关的VCL功能](https://developer.fastly.com/reference/vcl/variables/geolocation/)。

## 启用Fastly Edge模块

Fastly Edge Modules是一个灵活的框架，它允许通过模板定义UI组件和关联的VCL代码。 通过这些模块，可以轻松地通过用户界面自定义和扩展Fastly服务配置，而不用使用自定义VCL片段。

Edge模块允许您启用特定功能（如CORS标头、Cloud Sitemap重写），并配置Adobe Commerce存储与其他CMS或后端之间的集成。

要访问Edge“模块”菜单以查看、配置和管理可用模块，请打开&#x200B;_启用Fastly Edge模块_&#x200B;选项。 请参阅Fastly CDN模块文档中的[Fastly Edge模块](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md)。

## 配置后端和源屏蔽

后端设置提供对Fastly性能的微调以及起源屏蔽和超时。 _后端_&#x200B;是一个特定的位置（IP或域），它配置了用于检查和提供缓存内容的Origin屏蔽和超时设置。

_原始屏蔽_&#x200B;将存储的所有请求路由到特定存在点(POP)。 在收到请求时，POP会检查缓存的内容并提供该内容。 如果未缓存，则将继续缓存Shield POP，然后缓存内容的原始服务器。 防护罩会减少直接流向原点的流量。

默认的Fastly VCL代码指定云基础架构网站上Adobe Commerce的原始屏蔽和超时默认值。 在某些情况下，您可能需要修改默认值。 例如，如果您收到第一字节时间(TTFB)错误，则可能需要调整&#x200B;_第一字节超时_&#x200B;值。

>[!NOTE]
>
>如果您的网站需要在功能上通过[Wordpress](fastly-vcl-wordpress.md)等后端集成交付，请自定义您的Fastly服务配置以添加后端并管理从Adobe Commerce商店到Wordpress的重定向。 有关详细信息，请参阅Fastly模块文档中的[Fastly Edge模块 — 其他CMS/后端集成](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md)。

**要查看后端设置配置**：

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**，然后展开&#x200B;**全页缓存**。

1. 展开&#x200B;**Fastly配置**&#x200B;部分。

1. 展开&#x200B;**后端设置**&#x200B;并选择齿轮以检查默认后端。 此时将打开一个模式窗口，其中显示当前设置以及更改这些设置的选项。

   ![修改后端](../../assets/cdn/fastly-backend.png)

1. 选择&#x200B;**Shield**&#x200B;位置（或数据中心）。

   项目的默认Fastly配置会将位置设置为最接近您的云服务区域。 如果需要对其进行更改，请选择靠近默认位置的位置。

1. 修改与屏蔽连接的超时值（以微秒为单位）、字节之间的时间和第一个字节的时间。 我们建议保留默认超时设置。

1. （可选）选择以&#x200B;**在编辑或保存后激活后端和屏蔽**。

1. 单击&#x200B;**上传**&#x200B;以保存更改并将其上传到Fastly服务器。

1. 在管理员中，选择&#x200B;**保存配置**。

有关详细信息，请参阅Fastly模块文档中的[后端设置指南](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md)。

## 基本身份验证

基本身份验证功能可保护您网站上的每个页面和资产
用户名和密码。 我们**不建议**激活基本
在生产环境中进行身份验证。 您可以在暂存环境中配置它
以在开发过程中保护您的站点。 请参阅Fastly CDN模块文档中的[基本身份验证指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md)。

如果添加用户访问权限并在暂存环境中启用基本身份验证，您仍可以
无需其他凭据即可访问管理员。

## 创建自定义VCL代码段

Fastly支持自定义版本的Varnish Configuration Language (VCL)以自定义Fastly服务配置。 例如，您可以使用带有边缘和访问控制列表(ACL)字典的VCL代码块来允许、阻止或重定向特定用户或IP地址的访问。

有关创建自定义VCL片段、边缘词典和ACL的说明，请参阅[自定义Fastly VCL片段](fastly-vcl-custom-snippets.md)。

>[!NOTE]
>
>在将自定义VCL代码、边缘词典和ACL添加到Fastly模块配置之前，请验证Fastly缓存服务是否可与默认配置配合使用。 查看[设置Fastly](fastly-configuration.md)。

## 管理域

对于Starter和Pro项目，您可以使用[!UICONTROL Domains]选项为存储添加和管理Fastly域配置。

- 对于入门项目，请转到[!DNL Cloud Console]中[!UICONTROL Domains]选项卡下的项目URL以添加项目URL。

- 对于Pro项目，请提交[Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以将该域添加到云项目配置。 支持团队还更新Adobe Commerce Fastly帐户配置以添加域。

**要从管理员管理Fastly域配置**：

{{admin-login-step}}

1. 选择&#x200B;**存储** >设置> **配置** > **高级** > **系统**&#x200B;并展开&#x200B;**全页缓存**。

1. 在管理员&#x200B;_Fastly配置_&#x200B;部分中，选择&#x200B;**域**。

1. 单击&#x200B;**管理域**&#x200B;以打开“域”页面。

1. 在云环境中添加商店的顶级和子域名。

   您只能指定已添加到云基础架构配置的域。

   ![为Starter](../../assets/cdn/fastly-starter-activate-domain.png)添加Fastly域配置

1. 单击&#x200B;**激活**&#x200B;以更新Fastly域配置。

>[!NOTE]
>
>如果已在另一个Fastly帐户上配置了同一域，则必须先提交Adobe Commerce支持工单以请求域委派，然后才能将域添加到Adobe Commerce。 查看[多个Fastly帐户和分配的域](fastly.md#multiple-fastly-accounts-and-assigned-domains)。

## 启用维护模式

使用&#x200B;_维护模式_&#x200B;选项可允许从指定的IP地址对您的站点进行管理访问，同时返回所有其他请求的错误页面。

**要启用具有管理访问权限的维护模式**：

1. 在管理员中打开&#x200B;_Fastly配置_&#x200B;部分。

1. 在&#x200B;_Edge ACL_&#x200B;部分中，使用管理IP地址更新`maint_allow`访问控制列表(ACL)，该地址可在存储处于维护模式时访问存储。

   ![更新IP维护模式允许列表](../../assets/cdn/fastly-maint-allowlist.png)

1. 在&#x200B;_维护模式_&#x200B;部分中，选择&#x200B;**启用维护模式**。

   启用维护模式后，除来自`maint_allowlist` ACL中IP地址的请求外，所有通信都将被阻止。 您可以更新`maint_allowlist`以更改ACL中的IP地址。

   有关详细的配置说明，请参阅Magento 2模块的Fastly CDN文档中的[维护模式指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md)。
