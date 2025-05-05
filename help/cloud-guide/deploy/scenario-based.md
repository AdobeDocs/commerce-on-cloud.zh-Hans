---
title: 基于方案的部署
description: 了解如何使用自定义配置文件在云基础架构部署中自定义Adobe Commerce。
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# 基于方案的部署

对于`ece-tools` 2002.1.0及更高版本，您可以使用基于方案的部署功能自定义默认部署行为。
此功能在配置中使用&#x200B;**方案**&#x200B;和&#x200B;**步骤**：

- **方案配置** — 每个部署挂接都是&#x200B;*方案*，这是一个XML配置文件，用于描述完成部署任务的顺序和配置参数。 您可以在`.magento.app.yaml`文件的`hooks`部分中配置方案。

- **步骤配置** — 每个方案都使用一系列&#x200B;*步骤*，这些步骤以编程方式描述完成部署任务所需的操作。 您可以在基于XML的方案配置文件中配置步骤。

云基础架构上的Adobe Commerce在`ece-tools`包中提供了一组[默认方案](https://github.com/magento/ece-tools/tree/2002.1/scenario)和[默认步骤](https://github.com/magento/ece-tools/tree/2002.1/src/Step)。 您可以通过创建自定义XML配置文件来自定义部署行为，以覆盖或自定义默认配置。 您还可以使用场景和步骤从自定义模块运行代码。

## 使用生成和部署挂接添加方案

您将用于生成和部署Adobe Commerce的场景添加到`.magento.app.yaml`文件的`hooks`部分。 每个挂接指定每个阶段要运行的场景。 以下示例显示了默认方案配置。

> `magento.app.yaml`挂钩

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>随着`ece-tools` 2002.1.x的发布，有新的[挂接配置](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html)格式。 仍支持`ece-tools` 2002.0.x版本的旧格式。 但是，必须更新为新格式才能使用基于场景的部署功能。

## 查看方案步骤

在挂接配置中，每个方案都是一个XML文件，其中包含运行生成、部署或部署后任务的步骤。 例如，`scenario/transfer`文件包括三个步骤：`compress-static-content`、`clear-init-directory`和`backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## 扩展默认方案

以下示例通过将其他部署配置文件附加到挂接配置来扩展默认部署方案。

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

在部署期间，自定义方案与基于以下规则的默认方案合并：

- 方案将根据它们在挂接定义中的顺序进行优先级排序，其中列出具有最高优先级的最后一个方案。

  在本例中，这些方案的优先级如下：

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` （默认或基线方案）

- 最高优先级方案中的步骤将覆盖其他方案中具有相同名称的步骤。 新步骤将添加到配置中。 相同的规则适用于两个以上的场景，每个场景从右到左排列优先级，例如(C → B → A)。

### 删除默认步骤

使用`skip`参数从默认方案中删除步骤。

例如，要跳过默认部署方案中的`enable-maintenance-mode`和`set-production-mode`步骤，请创建包含以下配置的配置文件。

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

要使用自定义配置文件，请更新默认的`.magento.app.yaml`文件。

> 具有自定义部署方案的`.magento.app.yaml`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### 替换默认步骤

自定义方案可以替换默认步骤以提供自定义实施。 要实现此目的，请使用默认步骤名称作为自定义步骤的名称。

例如，在[默认部署方案]中，`enable-maintenance-mode`步骤运行默认的[EnableMaintenanceMode PHP脚本]。

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

要覆盖此步骤，请创建一个自定义方案配置文件，以在`enable-maintenance-mode`步骤运行时运行其他脚本。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### 更改步骤优先级

自定义方案可以更改默认步骤的优先级。 以下步骤将`enable-maintenance-mode`步骤的优先级从`300`更改为`10`，以便该步骤在部署方案中较早运行。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

在此示例中，`enable-maintenance-mode`步骤移至方案的开头，因为其优先级低于默认部署方案中的所有其他步骤。

### 示例：扩展部署方案

以下示例使用以下更改自定义[默认部署方案]：

- 将`remove-deploy-failed-flag`步骤替换为自定义步骤
- 跳过预部署步骤中的`clean-redis-cache`子步骤
- 跳过`unlock-cron-jobs`步骤
- 跳过`validate-config`步骤以禁用关键验证器
- 添加新的预部署步骤

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

要在项目中使用此脚本，请将以下配置添加到Adobe Commerce on cloud infrastructure项目的`.magento.app.yaml`文件中：

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>您可以查看`ece-tools` GitHub存储库中的[默认方案](https://github.com/magento/ece-tools/tree/2002.1/scenario)和[默认步骤配置](https://github.com/magento/ece-tools/tree/2002.1/src/Step)，以确定要为项目生成、部署和部署后任务自定义哪些方案和步骤。

## 添加自定义模块以扩展`ece-tools`

`ece-tools`包提供了遵循语义版本标准的默认API接口。 所有API接口都标有&#x200B;**@api**&#x200B;注释。 您可以通过创建自定义模块并根据需要修改默认代码来将默认API实施替换为您自己的实施。

要在云基础架构上的Adobe Commerce中使用自定义模块，您必须在`ece-tools`包的扩展列表中注册您的模块。 注册过程与在Adobe Commerce中注册模块时使用的过程类似。

**使用`ece-tools`包注册模块**：

1. 在模块的根目录中创建或扩展`registration.php`文件。

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. 更新模块配置文件的`autoload`部分以包含`registration.php`文件，以便在`composer.json`中自动加载模块文件。

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. 将`config/services.xml`文件添加到模块中。 此配置合并到`ece-tools`包中的`config/services.xml`上。

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

要了解有关依赖项注入的详细信息，请参阅[Symfony依赖项注入](https://symfony.com/doc/current/components/dependency_injection.html)。

<!-- link definitions -->

[默认部署方案]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP脚本]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
