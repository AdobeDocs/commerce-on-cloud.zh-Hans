---
title: 部署后变量
description: 请参阅环境变量列表，这些变量用于控制Adobe Commerce中针对云基础架构部署后阶段的操作。
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# 部署后变量

以下&#x200B;_部署后_&#x200B;变量在部署后阶段控制操作，可以继承和覆盖[全局变量](variables-global.md)的值。 在`.magento.env.yaml`文件的`post-deploy`阶段中插入这些变量：

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

有关自定义生成和部署过程的详细信息：

- [部署配置](configure-env-yaml.md)
- [部署过程](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **默认值**— `[]` （空数组）
- **版本**—Adobe Commerce 2.1.4及更高版本

为指定页面配置&#x200B;_到第一个字节的时间_ (TTFB)测试以测试网站性能。 为需要测试的每个页面指定绝对路径引用，或指定带有协议和主机的URL。

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

在指定要测试和提交更改的页面后，_到第一个字节的时间_&#x200B;测试会在部署后阶段运行，并将每个路径的结果发布到云日志：

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

对于重定向路径，日志会报告重定向目标的路径，而不是环境变量中配置的路径。 如果指定的路径无效，日志将显示警告消息。

## `WARM_UP_CONCURRENCY`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

指定在高速缓存预热操作期间发送的并发请求数限制，以减少服务器负载。 此值限制并行连接的数量，对于`WARM_UP_PAGES`部署后变量为缓存预加载指定多个页面的环境配置非常有用。

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **默认值**— `index.php`
- **版本**—Adobe Commerce 2.1.4及更高版本

自定义用于预加载`post_deploy`阶段中的缓存的页面列表。 您必须配置部署后挂接。 查看`.magento.app.yaml`文件的[挂接部分](../application/hooks-property.md)。

- **单页** — 指定要添加到缓存的单个页。 您不必指定默认的基本URL。 以下示例缓存`BASE_URL/index.php`页面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **多个域** — 列出多个URL。 以下示例缓存来自两个域的页面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **多个页面** — 使用以下格式，根据特定的正则表达式模式缓存多个页面：

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`：可能的变体`category`、`cms-page`、`product`、`store-page`
   - `pattern|url|product_sku`：使用`regexp`模式或完全匹配项`url`筛选URL，或对所有页面使用星号(\*)。 为`product`实体类型使用产品SKU
   - `store_id|store_code`：使用商店的ID或代码或星号(\*)表示所有商店，您可以传递多个用`|`分隔的商店ID或代码

  以下示例基于这些条件缓存`category`和`cms-page`实体类型：
   - ID为`1`的存储的所有类别页面
   - 代码为`store1`和`store2`的存储的所有类别页面
   - 代码为`store_en`的商店的类别页面`cars`
   - 所有商店的cms页面`contact`
   - ID为`1`和`2`的存储的CMS页面`contact`
   - 任何包含`car_`且以`html`结尾的类别页面，用于ID为2的存储
   - 包含`tires_`且代码为`store_gb`的存储的任何类别页面

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  以下示例基于这些条件缓存`product`实体类型：
   - 所有商店的所有产品（通过编程方式限制为每个商店100个产品，以避免性能问题）
   - 商店`store1`的所有产品
   - 所有商店中具有`sku1`的产品
   - 代码为`store1`和`store2`的商店的`sku1`产品
   - 代码为`store1`和`store2`的商店的具有`sku1`、`sku2`和`sku3`的产品

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  以下示例基于这些条件缓存`store-page`实体类型：
   - 第`/contact-us`页（所有商店）
   - ID为`1`的商店的第`/contact-us`页
   - 代码为`code1`和`code2`的商店的页面`/contact-us`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
