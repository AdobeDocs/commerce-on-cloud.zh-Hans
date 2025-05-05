---
title: 配置路由
description: 了解如何为云基础架构环境上的Adobe Commerce的传入HTTPS请求定义路由。
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 配置路由

`.magento/routes.yaml`目录中的`routes.yaml`文件定义了Adobe Commerce在云基础架构集成、暂存和生产环境中的路由。 路由决定应用程序如何处理传入的HTTP和HTTPS请求。

默认`routes.yaml`文件指定用于处理HTTP请求的路由模板为HTTPS，这些模板位于具有单个默认域的项目以及为多个域配置的项目中：

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

使用`magento-cloud` CLI查看已配置路由的列表：

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Pro环境的配置更新

{{pro-self-service-warning}}

## 路由模板

`routes.yaml`文件是模板化路由及其配置的列表。 您可以在路由模板中使用以下占位符：

- `{default}`占位符表示配置为项目默认值的限定域名。

  例如，具有默认域`example.com`的项目和以下路由模板：

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  这些模板将解析为生产环境中的以下URL：

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- `{all}`占位符表示为项目配置的所有域名。

  例如，具有`example.com`和`example1.com`个域的项目具有以下路由模板：

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  对于项目中的所有域，这些模板解析为以下路由：

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  `{all}`占位符对于为多个域配置的项目非常有用。 在非生产分支中，`{all}`被替换为每个域的项目ID和环境ID。

  如果项目未配置任何域（这在开发过程中很常见），`{all}`占位符的行为与`{default}`占位符相同。

Adobe Commerce还为每个活动的集成环境生成路由。 对于集成环境，`{default}`占位符将替换为以下域名：

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

例如，在`us`群集中托管的`mswy7hzcuhcjw`项目的`refactorcss`分支具有以下域：

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>如果您的云项目支持多个商店，请按照[多个网站或商店](../store/multiple-sites.md)的路由配置说明操作。

### 尾随斜杠

路由定义包含指示文件夹或目录的尾随斜杠；但是，可以使用或不使用尾随斜杠提供相同的内容。 以下URL解析相同，但可以解释为&#x200B;_两个不同的_ URL：

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>最好在目录中使用结尾斜杠，但无论您选择哪种方法，都务必&#x200B;**保持一致**&#x200B;以避免生成两个位置。

## 路由协议

所有环境都自动支持HTTP和HTTPS。

- 如果配置仅指定HTTP路由，则会自动创建HTTPS路由，从而允许从HTTP和HTTPS为站点提供服务，而无需重定向。

  例如，具有默认域`example.com`的项目和以下路由模板：

  ```text
  http://{default}/
  ```

  此模板解析为以下URL：

  ```text
  http://example.com/
  
  https://example.com/
  ```

- 如果配置仅指定HTTPS路由，则所有HTTP请求都将重定向到HTTPS。

  例如，默认域为`example.com`的项目具有以下路由模板：

  ```text
  https://{default}/
  ```

  此模板解析为以下URL：

  ```text
  https://example.com/
  ```

  它还会处理以下重定向：

  `http://example.com/` ==> `https://example.com/`

通过TLS提供所有页面。 对于此配置，必须使用以下方法之一将所有未加密的请求重定向到等效的TLS：

- 在`routes.yaml`文件中将协议更改为HTTPS。

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- 对于暂存和生产环境，请从管理UI中启用[强制Fastly上的TLS](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html)选项。 使用此选项时，Fastly处理到HTTPS的重新定向，因此您不必更新`routes.yaml`配置。

## 路由选项

使用以下属性分别配置每个路由：

| 属性 | 描述 |
| ---------------- | ----------- |
| `type: upstream` | 为应用程序提供服务。 此外，它有一个`upstream`属性，该属性指定应用程序名称（如`.magento.app.yaml`中所定义）并后跟`:http`终结点。 |
| `type: redirect` | 重定向到另一条路由。 它后面是`to`属性，该属性是对由其模板标识的另一路由的HTTP重定向。 |
| `cache:` | 控制路由[&#128279;](caching.md)的缓存。 |
| `redirects:` | 控制[重定向规则](redirects.md)。 |
| `ssi:` | 控件启用[服务器端Include](server-side-includes.md)。 |

## 简单路由

在以下示例中，路由配置将Apex域和`www`子域路由到`mymagento`应用程序。 此路由不会重定向HTTPS请求。

**示例1：**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

在本例中，请求路由遵循以下规则：

- 服务器使用以下URL模式直接响应请求：

  ```text
  http://example.com/path
  ```

- 服务器为具有以下URL模式的请求发出&#x200B;_301重定向_：

  ```text
  http://www.example.com/mypath
  ```

  这些请求将重定向到apex域，例如：

  ```text
  http://example.com/mypath
  ```

在以下示例中，路由配置不会将URL从www域重定向到apex域。 相反，会同时从www和apex域提供请求。

**示例2：**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## 通配符路由

云基础架构上的Adobe Commerce支持通配符路由，因此您可以将多个子域映射到同一个应用程序。 这适用于重定向和上游路由。 您用星号(\*)为路由添加前缀。 例如，以下路由到同一个应用程序：

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

这可用作实时环境中的捕获所有域。

### 路由未映射的域

您可以使用点(`.`)来分隔子域，从而路由到未映射到域的系统。

**示例：**

具有`add-theme`分支的项目路由到以下URL：

```text
http://add-theme-projectID.us.magento.com/
```

如果定义以下路由模板：

```text
http://www.{default}/
```

路由解析到以下URL：

```text
http://www.add-theme-projectID.us.magento.com/
```

您可以在点和路由解析之前插入任何子域。

**示例：**

定义以下工艺路线模板：

```text
http://*.{default}/
```

此模板解析以下两个URL：

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

您可以查看未映射域的路由模式，方法是建立到环境的SSH连接，并使用`magento-cloud` CLI列出路由：

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## 重定向和缓存

如[重定向](redirects.md)中详细讨论的，您可以管理复杂的重定向规则，如&#x200B;_部分重定向_，并为基于路由的[缓存](caching.md)指定规则：

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
