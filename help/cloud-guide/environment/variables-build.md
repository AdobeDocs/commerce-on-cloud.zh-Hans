---
title: 生成变量
description: 请参阅在云基础架构构建阶段控制Adobe Commerce中操作的环境变量列表。
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# 生成变量

以下&#x200B;_生成_&#x200B;变量在生成阶段控制操作，可以继承和覆盖来自[全局变量](variables-global.md)的值。 在`.magento.env.yaml`文件的`build`阶段中插入这些变量：

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

有关自定义生成和部署过程的详细信息：

- [部署配置](configure-env-yaml.md)
- [部署过程](../deploy/process.md)

v2.2中删除了以下变量：

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **默认**—`1`
- **版本**—Adobe Commerce 2.1.4及更高版本

设置用于保存错误报告文件的目录嵌套级别，以避免报告目录填入成千上万个文件，这会使管理和查看数据变得困难。 此设置默认为`1`。 通常，除非在管理`<magento_root>/var/report/`目录中的错误报告文件时遇到问题，否则无需更改默认值。

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

指定要在部署期间应用的Adobe Commerce质量修补程序列表。

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

以下示例指定部署期间要应用的三个修补程序。

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

请参阅[应用修补程序](../development/apply-patches.md)。

## `SCD_COMPRESSION_LEVEL`

- **默认**—`6`
- **版本**—Adobe Commerce 2.1.4及更高版本

指定压缩静态内容时要使用的[gzip](https://www.gnu.org/software/gzip)压缩级别（`0`到`9`）；`0`禁用压缩。

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **默认**—`600`
- **版本**—Adobe Commerce 2.1.4及更高版本

当压缩静态资源所花费的时间超过压缩超时限制时，将中断部署过程。 设置静态内容压缩命令的最长执行时间（秒）。

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **默认**—`false`
- **版本**—Adobe Commerce 2.4.2及更高版本

设置为`true`以防止在生成阶段为父主题生成静态内容。

在构建阶段设置`SCD_NO_PARENT: false`，以便为父主题生成静态内容不会影响站点部署或导致不必要的站点停机。 请参阅[静态内容部署](../deploy/static-content.md)。

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

您可以为每个主题配置多个区域设置。 此自定义有助于通过减少不必要的主题文件来加快构建过程。 例如，您可以使用英语构建&#x200B;_magento/backend_&#x200B;主题，并使用其他语言构建自定义主题。

以下示例使用三种区域设置构建`Magento/backend`主题：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

以下示例使用三种区域设置构建三个主题：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

或者，您可以选择&#x200B;_不_&#x200B;部署主题：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.2.0及更高版本

允许您增加静态内容部署的最大预期执行时间。

默认情况下，云基础架构上的Adobe Commerce将最大预期执行时间设置为900秒，但在某些情况下，您可能需要更多时间才能完成云项目的静态内容部署。

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **默认**—`quick`
- **版本**—Adobe Commerce 2.2.0及更高版本

为静态内容自定义[部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=zh-Hans)。 请参阅[部署静态视图文件](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=zh-Hans)。

如果您有多个区域设置，请仅使用这些选项&#x200B;__：

- `standard` — 为所有包部署所有静态视图文件。
- `quick` — （_默认值_）可最大限度地缩短部署时间。
- `compact` — 节省服务器上的磁盘空间。 在Adobe Commerce版本2.2.4及更早版本中，此设置将使用`1`的值覆盖`scd_threads`的值。

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **默认** — 自动
- **版本**—Adobe Commerce 2.1.4及更高版本

设置静态内容部署的线程数。 默认值是根据检测到的CPU线程数设置的，不超过4的值。 增加线程数会加快静态内容部署；减少线程数会减慢部署速度。 您可以设置线程值，例如：

```yaml
stage:
  build:
    SCD_THREADS: 2
```

要进一步缩短部署时间，请使用[配置管理](../store/store-settings.md)和`scd-dump`命令将静态部署移动到生成阶段。

## `SCD_USE_BALER`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.3.0及更高版本

[Baler](https://github.com/magento/baler)扫描您生成的JavaScript代码并创建优化的JavaScript捆绑包。 将优化的捆绑包部署到您的站点可以减少加载您的站点时的网络请求数并缩短页面加载时间。

设置为`true`可在执行静态内容部署后运行过滤器。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>由于Baler是Alpha版本，因此不建议在生产环境中使用它。

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **默认值**— _未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

设置为`true`以在Cloud Docker安装期间跳过`composer dump-autoload`命令。 此变量仅与具有可写文件系统的Cloud Docker容器相关。 在这种情况下，跳过该命令可防止其他命令尝试从已删除的`generated`目录访问代码时出错。

当Adobe Commerce运行`composer dump-autoload`时，它将创建带有指向`generated`文件夹中生成的类的链接的自动加载文件，这在具有只读文件系统的生产环境中不是问题。 但是，对于具有可写文件系统的Cloud Docker安装（仅为使用`./vendor/bin/ece-docker build:compose --with-test`进行测试和开发而创建），您可以运行`bin/magento -n setup:upgrade`命令而不使用`--keep-generated`选项，这将删除`generated`目录。 如果删除了目录，则`composer dump-autoload`命令将失败，因为自动加载包含指向已删除目录中的文件的链接。

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **默认值**— _未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

设置为`true`可在生成阶段跳过静态内容部署。

如果您已在使用[配置管理](../store/store-settings.md)的生成阶段部署静态内容，则可以跳过静态内容部署以进行快速生成测试。

在生成阶段，设置`SKIP_SCD: false`，以便在生成阶段进行静态内容生成，该过程不会影响站点部署或导致不必要的站点停机。 请参阅[静态内容部署](../deploy/static-content.md)。

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **默认值**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

为部署阶段执行的`bin/magento`个CLI命令启用或禁用[Symfony](https://symfony.com/doc/current/console/verbosity.html)调试详细级别。

>[!NOTE]
>
>要使用VERBOSE_COMMANDS控制命令输出中成功和失败的`bin/magento` CLI命令的详细信息，必须设置[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`。

选择日志中提供的详细信息级别：

- `-v`=正常输出
- `-vv`=更多详细输出
- `-vvv` =详细输出，非常适用于调试

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
