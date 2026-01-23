---
title: 添加站点地图和搜索引擎机器人
description: 了解如何在云基础架构上将站点地图和搜索引擎机器人添加到Adobe Commerce。
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# 添加站点地图和搜索引擎机器人

尝试生成`sitemap.xml`文件并将其写入根目录会导致以下错误：

```
Please make sure that "/" is writable by the web-server.
```

在云基础架构上使用Adobe Commerce，您只能写入特定目录，如`var`、`pub/media`、`pub/static`或`app/etc`。 当您使用“管理”面板生成`sitemap.xml`文件时，必须指定`/media/`路径。

您不必生成`robots.txt`文件，因为它根据需要生成`robots.txt`内容并将其存储在数据库中。 您可以使用`<domain.your.project>/robots.txt`或`<domain.your.project>/robots`链接在浏览器中查看内容。

这需要ECE-Tools版本2002.0.12及更高版本和更新的`.magento.app.yaml`文件。 在[magento-cloud存储库](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49)中查看这些规则的示例。

**要生成版本2.2及更高版本的`sitemap.xml`文件，请执行以下操作：**

1. 访问管理员。
1. 在&#x200B;_营销_&#x200B;菜单上，单击&#x200B;**SEO和搜索**&#x200B;部分中的&#x200B;_网站地图_。
1. 在&#x200B;_站点地图_&#x200B;视图中，单击&#x200B;**添加站点地图**。
1. 在&#x200B;_新建站点地图_&#x200B;视图中，输入以下值：

   - **文件名**：`sitemap.xml`
   - **路径**：`/media/`

1. 单击&#x200B;**保存并生成**。 新的站点地图将在&#x200B;_站点地图_&#x200B;网格中变得可用。
1. 单击Google _的_&#x200B;链接列中的路径。

**要将内容添加到`robots.txt`文件**：

1. 访问管理员。
1. 在&#x200B;_Content_&#x200B;菜单上，单击&#x200B;**设计**&#x200B;部分中的&#x200B;_配置_。
1. 在&#x200B;_设计配置_&#x200B;视图中，在&#x200B;**操作**&#x200B;列中单击网站的&#x200B;_编辑_。
1. 在&#x200B;_主网站_&#x200B;视图中，单击&#x200B;**搜索引擎机器人**。
1. 更新robots.txt **字段的**&#x200B;编辑自定义指令。
1. 单击&#x200B;**保存配置**。
1. 验证浏览器中的`<domain.your.project>/robots.txt`文件或`<domain.your.project>/robots` URL。

>[!NOTE]
>
>如果`<domain.your.project>/robots.txt`文件生成`404 error`，请[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)以移除从`/robots.txt`到`/media/robots.txt`的重定向。

## 使用Fastly VCL代码片段重写

如果您有不同的域并且需要单独的站点地图，则可以创建一个VCL以路由到正确的站点地图。 如上所述，在“管理”面板中生成`sitemap.xml`文件，然后创建自定义Fastly VCL代码片段来管理重定向。 查看[自定义Fastly VCL片段](../cdn/fastly-vcl-custom-snippets.md)。

>[!NOTE]
>
> 您可以从管理员UI或使用Fastly API上传自定义VCL片段。 请参阅[自定义VCL代码片段示例和教程](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code)。

### 使用Fastly VCL代码片段进行重定向

创建自定义VCL代码片段，以使用`sitemap.xml`和`/media/sitemap.xml`键值对将`type`的路径重写为`content`。

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

以下示例演示了如何将`robots.txt`和`sitemap.xml`的路径重写为`/media/robots.txt`和`/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**要对特定域重定向**&#x200B;使用Fastly VCL代码片段：

创建域为`pub/media/domain_robots.txt`的`domain.com`文件，并使用下一个VCL代码片段：

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL代码段路由`http://domain.com/robots.txt`并显示`pub/media/domain_robots.txt`文件。

要在单个代码片段中配置`robots.txt`和`sitemap.xml`的重定向，请创建`pub/media/domain_robots.txt`和`pub/media/domain_sitemap.xml`文件（域为`domain.com`）并使用下一个VCL代码片段：

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

在`sitemap`管理配置中，您必须使用`pub/media/`而不是`/`指定文件的位置。

### 按搜索引擎配置索引

要在生产环境中激活`robots.txt`自定义项，请在Cloud Console上的项目设置中启用搜索引擎对`<environment-name>`选项的索引：

- 旧版Cloud Console - URL遵循模式`https://<region-id>.magento.cloud/projects/<project_id>`

  将设置[!UICONTROL Indexing by search engines] （旧版控制台） [!UICONTROL Hide from search engines] (Adobe控制台)切换为&#x200B;**On**。

  ![使用[!DNL Cloud Console]管理环境](../../assets/robots-indexing-by-search-engine.png)

- Adobe Cloud Console - URL遵循模式``https://console.adobecommerce.com/<username>/<project_id>``

  取消选中设置[!UICONTROL Hide from search engines]。

- 您还可以使用magento-cloud CLI更新此设置：

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- 只能在生产环境中启用按搜索引擎编制索引，而不能在任何较低级别的环境中启用。
>
>- 如果您使用PWA Studio并且无法访问您配置的`robots.txt`文件，请将`robots.txt`添加到[前名允许列表](https://github.com/magento/magento2-upward-connector#front-name-allowlist)，位置为&#x200B;**商店** >配置> **常规** > **Web** > PWA配置向上。

