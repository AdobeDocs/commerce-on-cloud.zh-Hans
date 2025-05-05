---
title: 重定向
description: 了解如何在云基础架构项目中管理Adobe Commerce的重定向规则。
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 重定向

管理重定向规则是Web应用程序的常见要求，尤其是在您不希望丢失随时间更改或移除的传入链接的情况下。

下面演示了如何使用`routes.yaml`配置文件管理云基础架构项目上Adobe Commerce上的重定向规则。 如果本主题中讨论的重定向方法不适用，则可以使用缓存标头执行相同的操作。

{{route-placeholder}}

## Pro环境更新

{{pro-self-service-warning}}

>[!WARNING]
>
>对于云基础架构项目上的Adobe Commerce，在`routes.yaml`文件中配置大量非正则表达式重定向和重写可能会导致性能问题。 如果`routes.yaml`文件大于或等于32 KB，请卸载非正则表达式重定向并重新写入Fastly。 查看&#x200B;_Adobe Commerce帮助中心_&#x200B;中的[卸载非正则表达式重定向到Fastly，而不是Nginx（路由）](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html?lang=zh-Hans)。

## 全路径重定向

使用全路由重定向，您可以使用`routes.yaml`文件定义简单路由。 例如，您可以按如下方式从Apex域重定向到`www`子域：

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## 部分路由重定向

在`.magento/routes.yaml`文件中，您可以根据模式匹配将部分重定向规则添加到现有路由：

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

部分重定向适用于任何类型的路由，包括应用程序直接提供的路由。

`redirects`下有两个键可用：

- **过期** — 可选，指定在浏览器中缓存重定向的时间量。 有效值的示例包括`3600s`、`1d`、`2w`、`3m`。

- **路径** — 一个或多个为部分路由重定向规则指定配置的键值对。

  对于每个重定向规则，键都是一个表达式，用于筛选请求路径以进行重定向。 该值是一个对象，它指定重定向的目标目标和处理重定向的选项。

  值对象具有以下属性：

  | 属性 | 描述 |
  | ---------- | ----------- |
  | `to` | 必需，部分绝对路径、包含协议和主机的URL或指定重定向规则的目标目标的模式。 |
  | `regexp` | 可选，默认为`false`。 指定是否应将路径键解释为PCRE正则表达式。 |
  | `prefix` | 指定重定向是同时应用于路径及其所有子路径，还是只应用于路径本身。 默认为`true`。 如果`regexp`为`true`，则不支持此值。 |
  | `append_suffix` | 确定后缀是否随重定向一起传递。 默认为`true`。 如果`regexp`键为`true`或*如果`prefix`键为`false`，则不支持此值。 |
  | `code` | 指定HTTP状态代码。 有效的状态代码为[`301` （永久移动）](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2)、[`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3)、[`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8)和[`308`](https://www.rfc-editor.org/rfc/rfc7238)。 默认为`302`。 |
  | `expires` | 可选，指定在浏览器中缓存重定向的时间量。 默认为直接在`redirects`键下定义的`expires`值，但在此级别上，您可以微调各个部分重定向的缓存过期时间。 |

## 部分路由重定向示例

以下示例显示如何使用各种`paths`配置选项在`routes.yaml`文件中指定部分路由重定向。

### 正则表达式模式匹配

使用以下格式可基于正则表达式配置重定向请求。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

此配置根据正则表达式筛选请求路径，并将匹配请求重定向到`https://example.com`。 例如，对`https://example.com/regexp/a/b/c/match`的请求重定向到`https://example.com/a/b/c`。

### 前缀模式匹配

使用以下格式为以指定前缀模式开头的路径配置重定向请求。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

此配置的工作方式如下：

- 将匹配模式`/from`的请求重定向到路径`http://{default}/to`。

- 将匹配模式`/from/another/path`的请求重定向到`https://{default}/to/another/path`。

- 如果将`prefix`属性更改为`false`，则与`/from`模式匹配的请求将触发重定向，但与`/from/another/path`模式匹配的请求不会触发重定向。

### 后缀模式匹配

使用以下格式配置重定向请求，这些请求会将路径后缀从请求附加到目标目标：

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

此配置的工作方式如下：

- 将匹配模式`/from/path/suffix`的请求重定向到路径`https://{default}/to`。

- 如果将`append_suffix`属性更改为`true`，则与`/from/path/suffix`匹配的请求将重定向到路径`https://{default}/to/path/suffix`。

### 特定于路径的缓存配置

使用以下格式可自定义从特定路径缓存重定向的时间：

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

此配置的工作方式如下：

- 从第一个路径(`/from`)进行的重定向缓存了一天。

- 第二个路径(`/here`)中的重定向将缓存两周。
