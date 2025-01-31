---
title: 自定义错误和维护页面
description: 了解如何自定义在对Fastly源服务器的请求失败时显示的默认错误页面。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# 自定义错误和维护页面

当对Fastly源的请求失败时，Fastly会返回具有基本格式和通用消息传递的默认响应页面，这会让用户感到困惑。 例如，当对Fastly源的请求因503错误而失败时，Fastly返回以下默认错误页面。

![Fastly默认错误页](../../assets/cdn/fastly-503-example.png)

您可以更新Adobe Commerce存储配置，将一些默认响应页面替换为消息传送更友好的页面，并改进了HTML样式，如以下示例所示。

![Fastly自定义错误页](../../assets/cdn/fastly-new-error-page.png)

目前，您可以为云基础架构项目上的Adobe Commerce自定义以下Fastly响应页面。

- [服务器错误 — 内部服务器错误、超时或站点维护中断（错误代码500或更高）](#customize-the-503-error-page)
- [WAF阻止当WAF检测到可疑请求流量时发生的事件（403禁止访问）](#customize-the-waf-error-page)

**HTML编码要求：**

自定义页面的HTML代码必须满足以下要求：

- 内容最多可包含65,535个字符。
- 指定HTML源中的所有CSS内联。
- 使用base64捆绑HTML页面中的图像，以便即使Fastly处于离线状态也能显示这些图像。 查看css-tricks网站](https://css-tricks.com/data-uris/)上的[数据URI。

## 自定义503错误页面

在以下情况下，客户会看到默认的503错误页面：

- 当对Fastly源的请求返回大于500的响应状态时
- 当Fastly源关闭时，例如超时、维护活动或健康问题

您可以通过调整以下HTML代码以包含样式来匹配您的Adobe Commerce商店主题，并根据需要修改标题和消息传送来自定义默认页面。

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
         <title>503</title>
   </head>
   <body>
      <p>Service unavailable</p>
   </body></html>
```

验证修改的源是否正确显示在浏览器中。 然后，将自定义HTML代码添加到Fastly配置。

将自定义响应页面添加到Fastly配置：

{{admin-login-step}}

1. 选择&#x200B;**商店** > **设置** > **配置** > **高级** > **系统**。

1. 在右窗格中，展开&#x200B;**全页缓存** > **Fastly配置** > **自定义合成页面**。

   ![编辑503错误页](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. 选择&#x200B;**设置HTML**。

1. 将自定义响应页面的源代码复制并粘贴到HTML字段中。

   ![更新503错误页](../../assets/cdn/fastly-customize-503-response.png)

1. 选择页面顶部的&#x200B;**上传**&#x200B;以将自定义HTML源上传到Fastly服务器。

1. 选择页面顶部的&#x200B;**保存配置**&#x200B;以保存更新的配置文件。

1. 刷新缓存。

   - 在页面顶部的通知中，选择&#x200B;*缓存管理*&#x200B;链接。

   - 在“缓存管理”页上，选择&#x200B;**刷新Magento缓存**。

## 自定义WAF错误页面

当对Fastly源的请求失败并出现`403 Forbidden`错误(由[WAF](fastly-waf-service.md)阻止事件引起)时，客户会看到以下默认的WAF错误页面。

![WAF错误页](../../assets/cdn/fastly-waf-403-error.png)

以下代码示例显示了默认页面的HTML源：

```html
<html>
  <head>
    <title>Magento 403 Forbidden</title>
  </head>
  <body>
    <p>The requested URL was rejected.</p>
    <p>For additional information, please contact support and provide this reference ID:</p>
    <p>"} req.http.x-request-id {"</p>
    <p><button onclick='history.back();'>Go Back</button></p>
  </body>
</html>
```

您可以使用Fastly配置菜单中的&#x200B;**自定义合成页面** > **编辑WAF页面**&#x200B;选项来自定义Adobe Commerce on cloud infrastructure项目的默认代码。 编辑代码时，请保留以下行，以提供WAF阻止事件的参考ID：

```html
<p>"} req.http.x-request-id {"</p>
```

>[!NOTE]
>
>仅当在云基础架构项目上为WAF启用了托管Cloud WAF服务时，“编辑Adobe Commerce”选项才可用。

**编辑WAF错误页**：

1. [登录到管理员](../../get-started/onboarding.md#access-your-admin-panel)。

1. 选择&#x200B;**商店** > **设置** > **配置** > **高级** > **系统**。

1. 在右窗格中，展开&#x200B;**全页缓存** > **Fastly配置** > **自定义合成页面**。

   ![编辑WAF错误页面选项](../../assets/cdn/fastly-custom-synthetic-pages-edit-waf.png)

1. 选择&#x200B;**编辑WAF页面**。

1. 填写字段以更新HTML。

   ![更新WAF错误页](../../assets/cdn/fastly-edit-waf-html.png)

   - **状态** — 选择`403 Forbidden`状态。
   - **MIME类型** — 类型`text/html`。
   - **内容** — 编辑默认HTML响应以添加自定义CSS并根据需要更新标题和消息。

1. 选择页面顶部的&#x200B;**上传**&#x200B;以将自定义HTML源上传到Fastly服务器。

1. 选择页面顶部的&#x200B;**保存配置**&#x200B;以保存更新的配置文件。

1. 刷新缓存。

   - 在页面顶部的通知中，选择&#x200B;**缓存管理**&#x200B;链接。

   - 在“缓存管理”页上，选择&#x200B;**刷新Magento缓存**。

## 显示错误报告编号

默认情况下，Fastly隐藏&#x200B;*503服务不可用*&#x200B;错误之后的所有Adobe Commerce错误。 要显示错误日志报告编号以便查找和查看日志中的错误详细信息，请使用以下步骤打开省略Fastly的网站：

1. 检索存储的IP地址：

   - 对于Pro暂存和生产环境：

     ```bash
     nslookup {your_project_id}.ent.magento.cloud
     ```

   - 对于专业集成环境和入门环境：

     ```bash
     nslookup gw.{your_region}.magentosite.cloud
     ```

1. 将您的应用程序域和IP地址添加到本地工作站上的hosts文件中：

   ```text
   {server_IP} {store_domain}
   ```

1. 清除浏览器缓存和Cookie（或切换到无痕模式）。

1. 再次打开您的商店网站以查看错误代码。

1. 使用错误代码在错误报告文件中查找详细信息：

   - [使用SSH连接到受影响的环境](../development/secure-connections.md#connect-to-a-remote-environment)

   - 找到`./var/report/{error_number}`文件。
