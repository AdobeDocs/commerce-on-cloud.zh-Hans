---
title: 特定于云的变量
description: 请参阅云基础架构上特定于Adobe Commerce的环境变量列表。
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
exl-id: 82923b6f-221d-4902-a1b8-5ba6c7b3339a
TQID: https://experienceleague.adobe.com/Zk52OMqjrB74v9djO1PVOYd3wOS8EbdfL1rnqIdA8B4
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: ab64bb5a3cc159844015072738404274fdea97cd
workflow-type: tm+mt
source-wordcount: 343
ht-degree: 0%

---

# 特定于云的变量

云基础架构上特定于Adobe Commerce的环境变量使用`MAGENTO_CLOUD_*`前缀：

| 变量 | 描述 |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | 应用程序目录的绝对路径。 |
| `MAGENTO_CLOUD_APPLICATION` | 描述应用程序的base64编码JSON对象。 它映射到`.magento.app.yaml`文件内容并具有子键。 |
| `MAGENTO_CLOUD_APPLICATION_NAME` | 在`.magento.app.yaml`文件中配置的应用程序的名称。 |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Web文档根目录的绝对路径（如果适用）。 |
| `MAGENTO_CLOUD_ENVIRONMENT` | 环境分支的名称。 |
| `MAGENTO_CLOUD_PROJECT` | 项目ID |
| `MAGENTO_CLOUD_RELATIONSHIPS` | 表示键（关系名称）和值（关系对数组）端点定义的base64编码的JSON对象。 每个关系端点定义是URL的一种分解形式。 它在`query`中具有`scheme`、`host`、`port`和&#x200B;_可选_、`username`、`password`、`path`以及一些其他信息。 |
| `MAGENTO_CLOUD_ROUTES` | 描述环境`.magento/routes.yaml`文件中定义的路由。 |
| `MAGENTO_CLOUD_TREE_ID` | 应用程序的树ID，对应于Git中树的SHA。 |
| `MAGENTO_CLOUD_VARIABLES` | 具有键值对的base64编码的JSON对象，如`"key":"value"`。 |
| `MAGENTO_CLOUD_LOCKS_DIR` | 为云基础架构上的锁定提供程序提供到挂载点的路径。 锁定提供程序阻止启动重复的cron作业和cron组。<br><br>仅支持`file`和`db`锁定提供程序。<br><br>**Pro生产和暂存环境**&#x200B;默认为`file`锁定提供程序。 此值无法更改。<br><br>**专业集成和入门环境**，不要使用`MAGENTO_CLOUD_LOCKS_DIR`变量。 默认情况下应用`db`锁定提供程序。 您可以通过更新`.magento.env.yaml`文件中的`[LOCK_PROVIDER](variables-deploy.md#lock_provider`环境部署变量来更改默认值。 |

>[!WARNING]
>
>要使用[[!DNL Cloud Console]](../project/overview.md)将环境变量添加到[覆盖配置设置](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html)，您必须在变量名称前面加上`env:`，如以下示例所示：
>
>![环境变量示例](../../assets/set-env-variable-ui.png)

由于值会随着时间的推移而改变，因此最好在运行时检查变量并使用它来配置应用程序。 例如，使用`MAGENTO_CLOUD_RELATIONSHIPS`变量检索与环境相关的关系，如下所示：

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## 查看环境变量

您可以使用[包`ece-tools`中的`env:config:show`命令](../dev-tools/package-overview.md)显示当前环境的变量列表。

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

`variables`选项的示例输出：

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
