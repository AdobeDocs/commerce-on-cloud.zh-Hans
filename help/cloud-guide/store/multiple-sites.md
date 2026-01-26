---
title: 设置多个网站或商店
description: 了解如何在云基础架构上为Adobe Commerce配置多个网站或商店。
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# 设置多个网站或商店

您可以将Adobe Commerce配置为拥有多个网站或商店，例如英语商店、法语商店和德语商店。 查看[了解网站、商店和商店视图](best-practices.md#store-views)。

>[!WARNING]
>
>目录数据会随着您增加网站和商店的数量而扩展。 根据您的项目架构，额外的存储可能会导致非缓存目录页面的索引过程更长，响应速度更慢。 Adobe建议您密切监控站点性能。

设置多个商店的过程取决于您选择使用唯一域还是共享域。

具有唯一域的多个商店：

```
https://first.store.com/
https://second.store.com/
```

具有相同域的多个商店：

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>要将商店视图添加到站点基本URL，您不必创建多个目录。 请参阅[配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)中的&#x200B;_将存储代码添加到基本URL_。

## 添加域

自定义域可以添加到Pro Staging和任何生产环境；它们无法添加到集成环境。

添加域的过程取决于云帐户的类型：

- 用于Pro暂存和生产

  向Fastly添加新域，请参阅[管理域](../cdn/fastly-custom-cache-configuration.md#manage-domains)，或打开支持票证以请求帮助。 此外，您必须[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以请求将新域添加到群集。

- 仅用于入门级生产

  将新域添加到Fastly，请参阅[管理域](../cdn/fastly-custom-cache-configuration.md#manage-domains)或[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以请求帮助。 此外，您必须将新域添加到&#x200B;**中的**&#x200B;域[!DNL Cloud Console]选项卡： `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## 配置本地安装

若要将本地安装配置为使用多个商店，请参阅[配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html)中的&#x200B;_多个网站或商店_。

在成功创建和测试本地安装以使用多个存储区后，必须准备集成环境：

1. **配置路由或位置** — 指定Adobe Commerce处理传入URL的方式

   - [不同域的路由](#configure-routes-for-separate-domains)
   - [共享域的位置](#configure-locations-for-shared-domains)

1. **设置网站、商店和商店视图** — 使用Adobe Commerce管理UI进行配置
1. **修改变量** — 在`MAGE_RUN_TYPE`文件中指定`MAGE_RUN_CODE`和`magento-vars.php`变量的值
1. **部署和测试环境** — 部署和测试`integration`分支

>[!TIP]
>
>您可以使用本地环境设置多个网站或商店。 请参阅Cloud Docker说明以[设置多个网站或商店](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites)。

### Pro环境的配置更新

{{pro-self-service-warning}}

### 为不同的域配置路由

路由定义如何处理传入URL。 多个具有唯一域的存储区要求您定义`routes.yaml`文件中的每个域。 配置路由的方式取决于您希望站点的运行方式。

**要在集成环境中配置路由**：

1. 在本地工作站上，在文本编辑器中打开`.magento/routes.yaml`文件。

1. 定义域和子域。 `mymagento`上游值与`.magento.app.yaml`文件中的name属性值相同。

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. 将更改保存到`routes.yaml`文件。

1. 继续[设置网站、商店和商店视图](#set-up-websites-stores-and-store-views)。

### 为共享域配置位置

在路由配置定义如何处理URL的情况下，`web`文件中的`.magento.app.yaml`属性定义应用程序向Web公开的方式。 Web _位置_&#x200B;允许传入请求的更多粒度。 例如，如果您的域是`store.com`，则可以使用`/first` （默认站点）和`/second`来请求共享域的两个不同存储。

**要配置新的Web位置**：

1. 为根(`/`)创建别名。 在此示例中，第3行的别名为`&app`。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. 为网站(`/website`)创建传递，并使用上一步中的别名引用根。

   别名允许`website`从根位置访问值。 在此示例中，网站`passthru`在第21行上。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**要使用其他目录配置位置**：

1. 为根(`/`)和静态(`/static`)位置创建别名。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. 在`pub`目录下为网站创建子目录： `pub/<website>`

1. 将`pub/index.php`文件复制到`pub/<website>`目录中并更新`bootstrap`路径(`/../../app/bootstrap.php`)。

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. 为`index.php`文件创建传递。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. 提交并推送更改的文件。

   - `pub/<website>/index.php` （如果此文件位于`.gitignore`中，则推送可能需要强制选项。）
   - `.magento.app.yaml`

### 设置网站、商店和商店视图

在&#x200B;_管理UI_&#x200B;中，设置您的Adobe Commerce **网站**、**商店**&#x200B;和&#x200B;**商店视图**。 请参阅[配置指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)的“管理员”_中的_&#x200B;设置多个网站、商店和商店视图。

设置本地安装时，请务必使用管理员提供的网站、商店和商店视图的相同名称和代码。 更新`magento-vars.php`文件时需要这些值。

### 修改变量

请使用项目根目录中的`MAGE_RUN_CODE`文件传递`MAGE_RUN_TYPE`和`magento-vars.php`变量，而不是配置NGINX虚拟主机。

**要使用`magento-vars.php`文件传递变量**：

1. 在文本编辑器中打开`magento-vars.php`文件。

   [默认`magento-vars.php`文件](https://github.com/magento/magento-cloud/blob/master/magento-vars.php)应如下所示：

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. 移动评论的`if`块，使其位于&#x200B;_块的_&#x200B;之后`function`且不再评论。

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. 替换`if (isHttpHost("example.com"))`块中的以下值：
   - `example.com` — 使用您的&#x200B;_网站_&#x200B;的基本URL
   - `default` — 具有您的&#x200B;_网站_&#x200B;或&#x200B;_商店视图_&#x200B;的唯一代码
   - `store` — 使用以下值之一：
      - `website` — 在店面中加载&#x200B;_网站_
      - `store` — 在店面中加载&#x200B;_商店视图_

   对于使用唯一域的多个站点：

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   对于具有相同域的多个网站，必须检查&#x200B;_主机_&#x200B;和&#x200B;_URI_：

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. 将更改保存到`magento-vars.php`文件。

### 在集成服务器上部署和测试

在云基础架构集成环境中将更改推送到Adobe Commerce并测试您的站点。

1. 添加、提交代码更改并将其推送到远程分支。

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. 等待部署完成。

1. 部署后，在Web浏览器中打开您的商店URL。

   对于唯一域，请使用格式： `http://<magento-run-code>.<site-URL>`

   例如，`http://french.master-name-projectID.us.magentosite.cloud/`

   对于共享域，使用格式： `http://<site-URL>/<magento-run-code>`

   例如，`http://master-name-projectID.us.magentosite.cloud/french/`

1. 彻底测试您的网站，并将代码合并到`integration`分支以供进一步部署。

## 部署到暂存和生产环境

执行[部署到暂存和生产环境](../deploy/staging-production.md)的部署过程。 对于入门和专业版环境，您可以使用[!DNL Cloud Console]跨环境推送代码。

Adobe建议先在暂存环境中进行全面测试，然后再将其推送到生产环境。 在集成环境中更改代码，然后再次开始跨环境部署的流程。

