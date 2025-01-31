---
title: 缓存
description: 了解如何在云基础架构环境中为Adobe Commerce启用缓存。
feature: Cloud, Cache, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 缓存

您可以在云基础架构项目环境中启用缓存。 如果禁用缓存，则Adobe Commerce直接提供文件。

{{route-placeholder}}

## 设置缓存

通过在`.magento/routes.yaml`文件中配置缓存规则来启用应用程序的缓存，如下所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## 基于路由的缓存

通过分别为多个路由设置缓存规则来启用细粒度缓存，如以下示例所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

上述示例缓存以下路由：

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

并且以下路由&#x200B;**未被缓存**：

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>路由中的正则表达式&#x200B;**不支持**。

## 缓存持续时间

缓存持续时间由`Cache-Control`响应标头值决定。 如果响应中没有`Cache-Control`标头，则使用`default_ttl`键。

## 缓存键

为了确定如何缓存响应，Adobe Commerce构建了一个依赖于多个因素的缓存键并存储与此键关联的响应。 当请求带有相同的缓存键时，将重用响应。 其目的与HTTP [`Vary`标头](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44)类似。

参数`headers`和`cookies`键允许您更改此缓存键。

这些键的默认值如下：

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## 缓存属性

### `enabled`

当设置为`true`时，启用此路由的缓存。 当设置为`false`时，禁用此路由的缓存。

### `headers`

定义缓存键必须依赖的值。

例如，如果`headers`键如下所示：

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

然后Adobe Commerce会为`Accept` HTTP标头的每个值缓存不同的响应。

### `cookies`

`cookies`键定义缓存键必须依赖的值。

例如：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

缓存键取决于请求中`value` Cookie的值。

如果`cookies`键具有`["*"]`值，则存在特殊情况。 此值表示任何带有Cookie的请求都会绕过缓存。 这是默认值。

>[!NOTE]
>
>不能在Cookie名称中使用通配符。 使用精确的Cookie名称或匹配所有带星号(`*`)的Cookie。 例如，`SESS*`或`~SESS`当前是&#x200B;**非**&#x200B;有效值。

Cookie具有以下限制：

- 您可以在系统中设置最多&#x200B;**50个Cookie**。 否则，应用程序会引发`Unable to send the cookie. Maximum number of cookies would be exceeded`异常。
- Cookie最大大小为&#x200B;**4096字节**。 否则，应用程序会引发`Unable to send the cookie. Size of '%name' is %size bytes`异常。

### `default_ttl`

如果响应没有`Cache-Control`标头，则使用`default_ttl`键定义缓存持续时间（以秒为单位）。 默认值为`0`，这意味着不缓存任何内容。
