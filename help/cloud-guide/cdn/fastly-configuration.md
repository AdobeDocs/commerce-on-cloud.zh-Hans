---
title: 配置Fastly服务
description: 了解如何为您的Adobe Commerce项目设置和配置Fastly服务。
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: f9ce1e8b-4e9f-488e-8a4d-f866567c41d8
source-git-commit: 867abffd6cbed6e026c20b646ff641cc6ab40580
workflow-type: tm+mt
source-wordcount: '2063'
ht-degree: 0%

---

# 配置Fastly服务

云基础架构暂存和生产环境上的Adobe Commerce需要Fastly。

Fastly与Varnish合作，提供快速缓存功能以及用于静态资产的内容交付网络(CDN)。 Fastly还提供了一个Web应用程序防火墙(WAF)，以保护您的站点和云基础架构。 为了保护您的站点和云基础架构免受恶意流量和攻击，请通过Fastly路由所有传入的站点流量。

>[!NOTE]
>
>Fastly在集成环境中不可用。

请完成以下步骤，在站点开发过程的早期启用、配置和测试快速访问，以启用对站点的安全访问。

- 获取暂存和生产环境的Fastly凭据
- 启用Fastly CDN缓存
- 上传Fastly VCL片段
- 更新DNS配置以将流量路由到Fastly服务
- 测试Fastly缓存

>[!NOTE]
>
>启用并验证初始Fastly配置后，您可以自定义配置。 例如，可以启用其他选项，如图像优化、边缘模块和自定义VCL代码。 请参阅[自定义缓存配置](fastly-custom-cache-configuration.md)。

在项目配置期间，Adobe将您的项目添加到云基础架构上Adobe Commerce的[Fastly服务帐户](fastly.md#fastly-service-account-and-credentials)，并为Starter `master`和Pro暂存和生产环境创建Fastly帐户凭据。 每个环境都有唯一的凭据。

您需要Fastly凭据才能从Adobe Commerce管理员配置Fastly CDN服务并提交Fastly API请求。

## Fastly管理员仪表板访问权限

在云基础架构上使用Adobe Commerce，您无法直接访问Fastly管理仪表板。

您必须使用Adobe Commerce管理员来查看和更新环境的Fastly配置。 如果您无法在管理员中使用Fastly功能解决问题，请提交[Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

## 获取Fastly凭据

使用以下方法查找并保存环境的Fastly服务ID和API令牌：

**要查看您的Fastly凭据**：

>[!NOTE]
>
>请勿在支持票证、公共论坛或任何公共位置共享您的API令牌。 此外，绝不要将API令牌提交到代码存储库 — 存储库应仅包含没有敏感信息的不可变文件。
>
>Adobe Commerce支持已有权访问必要的密钥，因此在寻求帮助时，您无需提供API令牌。
>
>如果您的API令牌曾公开共享或附加到支持票证，则将被视为泄露。 在这种情况下，将需要Adobe为您生成一个新令牌。
>
>相关：验证Fastly凭据时出现[错误](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials#solution)

对于Pro和Starter项目，查看凭据的方法不同。

- IaaS挂载的共享目录 — 在Pro项目上，使用SSH连接到您的服务器并从`/mnt/shared/fastly_tokens.txt`文件中获取Fastly凭据。 暂存环境和生产环境具有唯一的凭据。 您必须获取每个环境的凭据。

- 本地工作区 — 从命令行中，使用`magento-cloud` CLI将[列出并查看](../environment/variables-cloud.md#viewing-environment-variables) Fastly环境变量。

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console] — 在[环境配置](../project/overview.md#configure-environment)中检查以下环境变量。

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>如果您找不到暂存或生产环境的Fastly凭据，请联系您的Adobe客户技术顾问(CTA)。

## 启用Fastly缓存

您需要以下组件来启用和配置Fastly服务：

- 暂存和生产环境中安装的适用于Magento 2模块[&#128279;](fastly.md#fastly-cdn-module-for-magento-2)的Fastly CDN的最新版本。 查看[快速升级](#upgrade-the-fastly-module)。

- 云基础架构暂存和生产环境上的Adobe Commerce的[Fastly凭据](#get-fastly-credentials)

**要在暂存和生产中启用Fastly CDN缓存**：

{{admin-login-step}}

1. 单击&#x200B;**存储** >设置> **配置** > **高级** > **系统**，然后展开&#x200B;**全页缓存**。

   ![展开以选择Fastly](../../assets/cdn/fastly-menu.png)

1. 在&#x200B;_缓存应用程序_&#x200B;部分中，从&#x200B;**使用系统值**&#x200B;中删除所选内容，然后从下拉列表中选择&#x200B;**Fastly CDN**。

   ![选择Fastly](../../assets/cdn/fastly-enable-admin.png)

1. 展开&#x200B;**Fastly配置**&#x200B;并[选择缓存选项](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module)。

1. 配置缓存选项后，单击页面顶部的&#x200B;**保存配置**。

1. 根据通知清除缓存。

1. 导航回&#x200B;**商店** > **设置** > **配置** > **高级** > **系统** > **Fastly配置**，以继续配置Fastly。

### 测试Fastly凭据

1. 在管理员中，导航到&#x200B;**商店** >设置> **配置** > **高级** > **系统** > **快速配置**。

1. 如果需要，请为您的项目环境添加&#x200B;**Fastly服务ID**&#x200B;和&#x200B;**API令牌**&#x200B;值。

   ![Fastly凭据管理员](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >请勿选择链接以创建Fastly API令牌。 请改用Adobe[&#128279;](#get-fastly-credentials)提供的Fastly凭据（服务ID和API令牌）。

1. 单击&#x200B;**测试凭据**。

1. 如果测试成功，请单击&#x200B;**保存配置**，然后清除缓存。

   如果测试失败，请验证正确的服务ID和API令牌值是否与当前环境的凭据匹配。

   如果测试再次失败，请提交Adobe Commerce支持工单或联系您的Adobe客户代表。 对于Pro项目，请包含生产网站和暂存网站的URL。 对于入门项目，请包含您的`Master`和暂存站点的URL。

>[!NOTE]
>
>有关更改暂存或生产环境的Fastly API令牌凭据的说明，请参阅[更改Fastly凭据](fastly.md#change-fastly-api-token)。

### 将VCL上传到Fastly

启用Fastly模块后，将默认[VCL代码](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)上传到Fastly服务器。 此代码提供了一系列VCL代码片段，这些代码片段指定了配置设置，以便为云基础架构上的Adobe Commerce启用缓存和其他Fastly CDN服务。

>[!NOTE]
>
>必须先将Fastly VCL代码初始上传到Adobe Commerce暂存和生产站点，Fastly缓存服务才能正常工作。

**要上传Fastly VCL**：

1. 在&#x200B;_Fastly配置_&#x200B;部分中，单击&#x200B;**将VCL上传到Fastly**，如下图所示。

   ![将Magento VCL上传到Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. 上载完成后，根据页面顶部的通知刷新缓存。

## 配置SSL/TLS证书

Adobe提供了一个域验证的Let’s Encrypt SSL/TLS证书，为来自Fastly的安全HTTPS流量提供服务。 Adobe为每个Pro Production、Staging和Starter Production环境提供一个证书，以保护该环境中的所有域。 有关提供的证书的详细信息，请参阅云基础架构上的[Adobe Adobe Commerce SSL (TLS)证书](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html)。

>[!NOTE]
>
>您可以提供自己的TLS或SSL证书，而不是使用Adobe提供的Let&#39;s Encrypt证书。 但是，此过程需要额外的设置和维护工作。 要选择此选项，请提交Adobe Commerce支持工单，或与Adobe一起在云基础架构环境中将自定义的托管证书添加到您的Adobe Commerce。

要为Adobe Commerce环境启用SSL/TLS证书，Adobe自动化将完成以下步骤：

- 验证域所有权
- 设置一个让我们加密SSL/TLS证书，该证书覆盖商店的指定顶级域和子域
- 在站点上线时将证书上传到云环境

此自动化要求您更新站点的DNS配置以提供域验证信息。 使用以下方法中的&#x200B;**one**：

- **DNS验证** — 对于实时站点，使用指向Fastly服务的CNAME记录更新您的DNS配置
- **ACME质询CNAME记录** — 使用Adobe为环境中的每个域提供的ACME质询CNAME记录更新您的DNS配置

>[!TIP]
>
>如果您有一个非活动的生产域，请使用ACME质询CNAME记录进行域验证。 提前将记录添加到您的DNS配置可让Adobe在站点启动之前为SSL/TLS证书配置正确的域。 在启动到生产环境之前，必须使用Adobe提供的CNAME记录替换这些占位符记录。

域验证完成后，Adobe会配置让我们加密TLS/SSL证书，并将其上传到实时暂存或生产环境。 此过程可能需要12小时。 我们建议您提前几天完成DNS配置更新，以防止网站开发和网站启动出现延迟。

## 使用开发设置更新DNS配置

在初始Fastly设置过程中，您可以使用以下URL在暂存环境和生产环境中配置和测试Fastly缓存：

- 对于Pro暂存和生产：

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- 仅用于入门级生产：

   - `mcprod.<your-domain>.com`

在配置项目后，这些默认的预生产URL将可用。 `"your-domain"`的值是您在载入流程中指定的域名。

>[!NOTE]
>
>您无法在入门项目上为非生产环境指定自定义域。

要将流量从您的商店URL路由到Fastly服务，请更新您的DNS配置。 当您更新配置时，Adobe会自动配置所需的SSL/TLS证书并将它们上传到您的云环境。 此配置最多可能需要12小时。

>[!NOTE]
>
>准备好启动生产站点时，必须再次更新DNS配置以将生产域指向Fastly服务并完成其他配置任务。 请参阅[启动项核对清单](../launch/checklist.md)。

**先决条件：**

- 启用Fastly模块。
- 上传默认的Fastly VCL代码。
- 向Adobe提供每个环境的顶级域和子域的列表，或提交Adobe Commerce支持工单。
- 等待确认指定的域已添加到云环境。
- 在入门项目中，将域添加到您的Fastly服务配置。 请参阅[管理域](fastly-custom-cache-configuration.md#manage-domains)。
- 有关更新DNS配置的信息，请向您的[DNS注册机构](https://lookup.icann.org/)查询您的域服务的正确方法。

**要更新用于开发的DNS配置**：

1. 通过添加CNAME记录`prod.magentocloud.map.fastly.net`将预生产URL指向Fastly服务，例如：

   | 域或子域 | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   当CNAME记录处于活动状态时，Adobe会设置证书并上传SSL/TLS证书。

   >[!NOTE]
   >
   >如果您计划为生产站点使用Apex域(`your-domain.com`)，则必须配置DNS地址记录（A记录）以指向Fastly服务器IP地址。 请参阅[使用生产设置更新DNS配置](../launch/checklist.md#to-update-dns-configuration-for-site-launch)。


1. 添加ACME质询CNAME记录以进行域验证和预配置生产SSL/TLS证书，例如：

   | 域或子域 | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >本例中的ACME挑战记录是占位符，并不打算配置您的Adobe Commerce暂存和生产站点。 联系Adobe以获取项目的正确ACME挑战记录信息。

   添加CNAME记录后，Adobe会验证域并为环境配置SSL/TLS证书。 当您更新DNS配置以将来自这些域的流量路由到Fastly服务时，Adobe会将证书上传到环境。

1. 更新Adobe Commerce基本URL。

   - 使用SSH登录到生产环境。

     ```bash
     magento-cloud ssh
     ```

   - 使用Cloud CLI更改商店的基本URL。

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >作为使用Cloud CLI的替代方法，您可以从[管理员](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)更新基本URL

1. 重新启动Web浏览器。

1. 测试您的网站。

## 测试Fastly缓存

完成DNS配置更改后，请使用[cURL](https://curl.se/)命令行工具验证Fastly缓存是否正常工作。

**检查响应标头**：

1. 在终端中，使用以下`curl`命令测试您的实时网站URL：

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   如果尚未设置静态路由或完成实时站点上域的DNS配置，请使用`--resolve`标志，该标志绕过DNS名称解析。

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. 在响应中，验证[标头](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers)以确保Fastly正常工作。 您应在响应中看到以下唯一标头：

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

如果标头没有正确的值，请参阅[解决在响应标头中发现的错误](fastly-troubleshooting.md#curl)以获取故障排除帮助。

## 升级Fastly模块

Fastly更新了Magento 2模块的Fastly CDN，以解决问题、提高性能并提供新功能。
我们建议您将暂存和生产环境中的Fastly模块更新为[最新版本](https://github.com/fastly/fastly-magento2/blob/master/VERSION)。

更新模块后，必须上传VCL代码以将更改应用于Fastly服务配置。

>[!WARNING]
>
> 如果已使用自定义版本自定义了默认Fastly VCL代码，则升级Fastly模块将覆盖所做的更改。 如果添加了具有唯一名称的自定义VCL代码段，则在升级过程中会保留这些更改。 作为最佳实践，请升级暂存环境并验证更改，然后再将更改应用于生产环境。

**检查Magento 2**&#x200B;的Fastly CDN模块的版本：

1. 更改为云环境的根目录。

1. 使用Composer检查安装的版本。

   ```bash
   composer show *fastly*
   ```

1. 如果未安装[最新版本](https://github.com/fastly/fastly-magento2/releases)，请完成升级Fastly模块的步骤。

**升级Fastly模块**：

1. 在本地集成环境中，使用以下模块信息[升级Fastly模块](../store/extensions.md#upgrade-an-extension)。

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. 将更新推送到暂存环境。

1. 登录到暂存环境的管理员以[上传VCL代码](#upload-vcl-to-fastly)。

1. 在Adobe Commerce暂存网站上[验证Fastly服务](fastly-troubleshooting.md#verify-or-debug-fastly-services)。

在暂存站点上验证Fastly服务后，请在生产环境中重复升级过程。

>[!TIP]
>
> 如果您在Adobe Commerce环境中遇到Fastly服务问题，请参阅[Adobe Commerce Fastly疑难解答程序](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html)。
