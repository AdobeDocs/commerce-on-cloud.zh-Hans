---
title: PHP设置
description: 了解在云基础架构中用于Commerce应用程序配置的最佳PHP设置。
feature: Cloud, Configuration, Extensions
exl-id: 83094c16-7407-41fa-ba1c-46b206aa160d
source-git-commit: 1725741cfab62a2791fe95cfae9ed9dffa352339
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# PHP设置

您可以选择在`.magento.app.yaml`文件中运行的PHP[&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=zh-Hans)的版本：

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>如果升级到PHP 8.1及更高版本，请从`.magento.app.yaml`文件中的[`runtime: extensions:`属性](properties.md#runtime)中删除JSON并重新部署。 JSON扩展自PHP 8.0起即安装在云环境中。

## 配置PHP

您可以使用附加到Adobe Commerce维护的配置的`php.ini`文件自定义环境的PHP设置。

在存储库中，将`php.ini`文件添加到应用程序的根目录（存储库根目录）。

>[!TIP]
>
>不正确配置PHP设置可能会导致问题，因此只有高级管理员才应该设置这些选项。

### 增加PHP内存限制

要增加PHP内存限制，请将以下设置添加到`php.ini`文件中：

```ini
memory_limit = 1G
```

对于调试，请将该值增加到2G。

### 优化real路径缓存配置

设置以下`realpath_cache`设置以提高应用程序性能。

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

这些设置允许PHP进程缓存文件的路径，而不是在每次加载页时查找这些路径。 请参阅PHP文档中的[性能调整](https://www.php.net/manual/en/ini.core.php)。

>[!NOTE]
>
>有关推荐的PHP配置设置列表，请参阅&#x200B;_安装指南_&#x200B;中的[必需的PHP设置](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html?lang=zh-Hans)。

### 检查自定义PHP设置

将`php.ini`更改推送到云环境后，您可以检查自定义PHP配置是否已添加到环境中。 例如，使用SSH登录到远程环境，显示PHP配置信息，并筛选`register_argc_argv`指令：

```bash
php -i | grep register_argc_ar
```

示例输出：

```text
register_argc_argv => On => On
```

>[!WARNING]
>
>如果您使用Cloud Docker for Commerce进行本地开发，请参阅[Docker服务容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container)，以了解有关在Docker环境中使用自定义`php.ini`文件的信息。

## 启用扩展

您可以在`runtime:extension`部分中启用或禁用PHP扩展。 此外，指定的扩展在Docker PHP容器中可用。

>[!IMPORTANT]
>
>在启用扩展之前，请务必了解PHP版本必须与托管项目的操作系统兼容。 您的项目环境可能需要基础架构团队升级操作系统，然后才能继续。

`.magento.app.yaml`文件中的示例：

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

使用SSH登录到环境并列出PHP扩展。

```bash
php -m
```

有关特定PHP扩展的详细信息，请参阅[PHP扩展列表](https://www.php.net/manual/en/extensions.alphabetical.php)。

下表显示了在Cloud平台上部署Adobe Commerce时支持的PHP扩展。

{{$include /help/_includes/templated/php-extensions-cloud.md}}

PHP模块要求与Adobe Commerce版本相关联。 请参阅[PHP要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html?lang=zh-Hans)。

### 扩展支持

对于Pro项目，需要其他支持才能安装以下扩展：

- `ioncube`
- `sourceguardian`

例如，要将PHP设置为在所有环境中仅执行受SourceGuardian保护的脚本，必须在`php.ini`文件中设置以下选项：

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

请参阅SourceGuardian文档的[第3.5节](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf)。 _这是指向PDF_&#x200B;的链接。

[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hans#submit-ticket)，以获得在所有生产环境和Pro暂存环境中安装这些PHP扩展的帮助。 包含更新的`.magento/services.yaml`文件、`.magento.app.yaml`文件（包含更新的PHP版本和任何其他PHP扩展名）。 对于实时生产环境的更改，您必须至少提供48小时的通知。 Cloud Infrastructure团队更新项目最多可能需要48小时。

>[!WARNING]
>
>不支持使用调试编译的PHP，并且探测器可能与[!DNL XDebug]或[!DNL XHProf]冲突。 启用Probe时禁用这些扩展。 探测与某些PHP扩展冲突，如[!DNL Pinba]或IonCube。
