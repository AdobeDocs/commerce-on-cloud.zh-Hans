---
title: 管理系统特定设置的示例
description: 请参阅有关如何跨云基础架构环境中的所有Adobe Commerce管理和同步存储配置设置的示例。
hidefromtoc: true
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---


# 管理系统特定设置的示例

此示例说明如何使用配置管理来保持所有环境中的存储设置一致。

此示例使用[存储设置](store-settings.md)中定义的以下过程：

1. 在集成环境存储区管理员中输入您的配置。
1. 创建`config.php`文件并将其传输到您的本地工作站。
1. 将`config.php`推送到远程集成环境。
1. 验证您的设置在“管理员”中不可编辑。
1. 进行任何必要的修改：

   * 更改集成环境的配置设置。
   * 要添加配置，请再次运行命令以创建`config.php`。 新配置将附加到文件。
   * 要删除或编辑现有配置，请手动编辑文件。
   * 提交和推送。

例如，您可能需要设置以下设置：

* 禁用集成环境中的区域设置和静态文件优化设置
* 在暂存和生产环境中启用静态文件优化
* 在暂存和生产环境中为Fastly配置每个用户的特定凭据

_静态文件优化_&#x200B;是指合并和缩小JavaScript和层叠样式表，以及缩小HTML模板。 请参阅[静态内容部署策略](../deploy/static-content.md)。

## 先决条件

要完成这些配置管理任务，您需要执行以下操作：

* 具有[环境“管理员”](../project/user-access.md)权限的项目阅读器角色
* 集成、暂存和生产环境的管理员URL和凭据

## 配置Commerce管理员

在集成环境中，您可以登录到管理员以修改商店、网站、模块或扩展名的系统配置设置、静态文件优化以及与静态内容部署相关的系统值。 查看[配置数据](store-settings.md#scd-performance)。

**要更改区域设置和静态文件优化设置**：

1. 登录到集成环境管理员。 您可以通过[[!DNL Cloud Console]](../project/overview.md)访问此URL。
1. 导航到&#x200B;**商店** >设置> **配置** >常规> **常规**。
1. 在页面导航中，展开&#x200B;**区域设置选项**。
1. 从&#x200B;**区域设置**&#x200B;列表中，更改区域设置。 您可以稍后再更改它。

   ![更改区域设置](../../assets/locale-options.png)

1. 单击&#x200B;**保存配置**。
1. 如果出现提示，[刷新缓存](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/tools/cache-management)。
1. 从管理员注销。

## 导出值并将config.php传输到本地系统

此步骤使用在本地计算机上运行的命令在集成环境中创建和传输`config.php`配置文件。

此过程对应于[推荐的过程](store-settings.md)中的步骤2。 创建`config.php`后，将其传输到本地系统，以便将其添加到Git。

**创建和传输`config.php`**：

1. 在本地工作站上，转到您的项目目录。

1. 更改为集成环境。

   ```bash
   magento-cloud environment:checkout integration
   ```

1. 创建远程数据库的本地转储。

   ```bash
   magento-cloud db:dump
   ```

以下来自`config.php`的代码片段显示了将默认区域设置更改为`en_GB`以及更改静态文件优化设置的示例：

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## 将config.php推送并部署到环境

现在您已创建`config.php`并将其传输到本地系统，请将其提交到Git并将其推送到您的环境。 此过程对应于[推荐的过程](store-settings.md)中的步骤3和步骤4。

以下命令添加、提交并推送到`master`分支：

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

完成到暂存和生产环境的代码部署。 首先，您推送到`staging`和`master`分支。 有关部署命令的详细信息，请参阅[部署您的存储](../deploy/staging-production.md)。

等待在所有环境中完成部署。

## 验证配置更改

将`config.php`推送到您的环境后，您更改的任何值在Admin中均应为只读。 在此示例中，修改后的默认区域设置和静态文件优化设置不应在Admin中编辑。 这些配置设置是在`config.php`中设置的。

要验证配置更改，请执行以下操作：

1. 在其中一个环境中从管理员注销。
1. 重新登录到管理员。
1. 单击“**存储**”>“设置”>“**配置**”>“常规”>“**常规”**。
1. 在右窗格中，展开&#x200B;**区域设置选项**。

   注意，多个字段无法编辑，如以下示例所示。 这些配置设置由`config.php`维护。

   ![某些值在管理员中不再可编辑](../../assets/locale-options-disabled.png)

1. 从管理员注销。

## 更改和更新特定于系统的配置设置

如果需要修改其中的任何设置，请使用文本编辑器手动修改`config.php`文件。 完成编辑或移除后，您可以按照上述步骤提交并将其推送到远程环境。

要添加配置，请修改集成环境，然后再次运行命令以生成文件。 任何新配置都会附加到文件中的代码中。 按照上述步骤将其推送到Git。

对于此示例，请修改静态文件优化设置并为JavaScript添加新设置。

### 在集成中添加配置

在集成环境管理员中添加配置值。 此示例合并JavaScript文件。

1. 从集成管理员注销。
1. 重新登录到集成管理员。
1. 单击&#x200B;**商店** >设置> **配置** > **高级** > **开发人员**。
1. 在右窗格中，展开&#x200B;**JavaScript设置**。
1. 从&#x200B;**合并JavaScript文件**&#x200B;列表中，单击&#x200B;**是**。
1. 单击&#x200B;**保存配置**。
1. 如果出现提示，[刷新缓存](https://experienceleague.adobe.com/zh-hans/docs/commerce-admin/systems/tools/cache-management)。
1. 从管理员注销。

通过再次运行dump命令，新配置将附加到文件中。

```bash
magento-cloud db:dump
```

### 使用新设置编辑config.php

在本地使用文本编辑器编辑更新的`app/etc/config.php`文件。 编辑这些设置以实现JavaScript、HTML和CSS文件的缩小。

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

要修改设置以允许缩小，请为`'minify_html'`和每个`'minify_files'`选项编辑`'0'`到`'1'`：

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

将更改保存到文件。

### 将更改推送到Git

要推送更改，请输入以下内容：

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

等待部署完成。

重复部署过程以将代码推送到所有环境。
