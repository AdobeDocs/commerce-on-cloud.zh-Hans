---
title: 部署过程
description: 了解部署如何适用于Adobe Commerce的云基础架构项目。
feature: Cloud, Build, Deploy, SCD
exl-id: 76806381-0ecc-4d76-974a-f203d3bf44da
TQID: https://experienceleague.adobe.com/mSJOsLfNVGbkSNSrUzJgszxsqc07c-4KFhrJxm5I72U
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 413
ht-degree: 0%

---

# 部署过程

当您执行合并、推送或同步环境时，或者当您触发[手动重新部署](../dev-tools/cloud-cli-overview.md#redeploy-the-environment)时，部署过程将开始。 部署过程需要时间，但优化部署的方法取决于您是开发和测试还是使用实时站点。 最值得注意的是，您可以控制[静态内容部署](static-content.md)。

部署过程分为三个不同的阶段：构建、部署和部署后。 每个阶段使用有限的资源执行特定操作：

## ![生成阶段](../../assets/status-build.png)生成阶段

_生成_&#x200B;阶段为配置文件中定义的服务组装容器，安装基于`composer.lock`文件的依赖项，并运行`.magento.app.yaml`文件中定义的生成挂接。 如果无法连接到任何服务或无法访问数据库，则构建阶段将取决于限制在环境中的资源。

## ![部署阶段](../../assets/status-deploy.png)部署阶段

_部署_&#x200B;阶段对传入的请求进行临时保留，并将站点转换到[维护模式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html)。 部署阶段使用新容器，在装入文件系统后打开网络连接，激活`.magento.app.yaml`文件的`relationships`部分中定义的服务，并运行`.magento.app.yaml`文件中定义的部署挂接。 除`.magento.app.yaml`文件中定义的目录外，所有内容均为&#x200B;_只读_。 默认情况下，[`mounts`属性](../application/properties.md#mounts)包含以下目录：

- `app/etc` — 包含`env.php`和`config.php`配置文件
- `pub/media` — 包含所有媒体数据，如产品或类别
- `pub/static` — 包含生成的静态文件
- `var` — 包含运行时创建的临时文件

所有其他目录均具有只读权限。 当新站点从维护模式过渡并释放对传入请求的临时保留时，它在部署阶段结束时变为活动状态。

在部署阶段，`app/etc/config.php`和`app/etc/env.php`部署配置文件的副本将以BAK扩展名保存。 请参阅[存储设置](../store/store-settings.md#restore-configuration-files)以了解如何还原这些文件。

## ![部署后阶段](../../assets/status-post-deploy.png)部署后阶段

_部署后_&#x200B;阶段运行`.magento.app.yaml`文件中定义的部署后挂接。 在此阶段执行任何操作都会影响网站性能；但是，您可以使用[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages)环境变量来填充缓存。

## ![验证状态](../../assets/status-verify.png)验证配置

您可以通过运行[智能向导](smart-wizards.md)来测试项目状态的最佳配置。

>[!NOTE]
>
>使用`ece-tools` 2002.1.0及更高版本，您可以使用基于方案的部署功能在云基础架构项目上自定义Adobe Commerce的生成、部署和部署后流程。 请参阅[基于方案的部署](scenario-based.md)。

