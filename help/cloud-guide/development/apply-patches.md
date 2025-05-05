---
title: 应用修补程序
description: 了解如何在Adobe Commerce on cloud infrastructure项目中应用修补程序。
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# 应用修补程序

[适用于Commerce的云修补程序](https://github.com/magento/magento-cloud-patches)和[Quality Patches Tool](https://github.com/magento/quality-patches)，可为您安装的Adobe Commerce应用程序提供修补程序。

- Commerce云修补程序包提供了所需的修补程序以及关键修补程序
- 质量修补程序提供可选的、低影响的质量修补程序，作为[不包含向后不兼容更改的单个修补程序](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html?lang=zh-Hans#individual-patch)

请参阅&#x200B;_Commerce Operations Tools Guide_&#x200B;中的[可用修补程序](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hans)，以查看已发布修补程序的完整列表。

这两个包都改进了所有Adobe Commerce版本与Cloud环境的集成，并支持快速交付关键、可选和自定义修补程序。 您可以使用这些软件包来应用、还原和查看有关Commerce可用的所有单个修补程序的一般信息。

>[!TIP]
>
>您可以将Commerce的[Quality Patches Tool](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hans)和Cloud Patches用作Magento Open Source和Adobe Commerce项目的独立包。 我们建议将质量修补程序工具用于非云项目。

当您将更改部署到远程环境时，`ece-tools`包使用`magento/magento-cloud-patches`和`magento/quality-patches`来检查挂起的修补程序并按以下顺序自动应用它们：

1. 应用Commerce的Commerce修补程序包中包含的所有必需的Cloud修补程序。
1. 应用Quality Patches Tool中包含的可选Commerce修补程序。
1. 按修补程序名称的字母顺序应用`/m2-hotfixes`目录中的自定义修补程序。

>[!NOTE]
>
>当您更新`ece-tools`包或Commerce包的Cloud修补程序时，会在下次部署项目时应用最新的必需修补程序，或者您可以使用`ece-patches apply` CLI命令立即部署它们并重新部署Cloud环境。 您不能在部署过程中跳过[必需的修补程序](https://github.com/magento/magento-cloud-patches/tree/develop/patches)。

## 先决条件

{{upgrade-tip}}

Quality Patches Tool依赖于Commerce和`ece-tools`包的云修补程序。 要应用最新的修补程序，您必须安装[最新版本的ECE-Tools](../dev-tools/update-package.md)。 ECE-Tools的最低要求版本是2002.1.2。

## 查看可用的补丁程序和状态

要查看可用单个修补程序的列表，请执行以下操作：

```bash
php ./vendor/bin/ece-patches status
```

示例响应：

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

状态表包含以下类型的信息：

- **类型**：
   - `Optional` - Quality Patches Tool和Cloud Patches包中的所有修补程序对于Adobe Commerce和Magento Open Source安装都是可选的。 对于云基础架构上的Adobe Commerce，所有修补程序都是可选的。
   - `Required` - Cloud客户需要Commerce软件包的Cloud修补程序中的所有修补程序。
   - `Deprecated` — 单个修补程序已标记为已弃用，如果您已应用它，我们建议恢复它。 还原已弃用的修补程序后，该修补程序将不再显示在状态表中。
   - `Custom` — “m2-hotfixes”目录中的所有修补程序。

- **状态**：
   - `Applied` — 已应用修补程序。
   - `Not applied` — 尚未应用该修补程序。
   - `N/A` — 由于存在冲突，无法定义修补程序的状态。

- **详细信息**：
   - `Affected components` — 受影响的模块列表。
   - `Required patches` — 所需的修补程序（依赖项）列表。
   - `Recommended replacement` — 建议用于替代已弃用修补程序的修补程序。

## 在本地环境中应用修补程序

您可以在本地环境中手动应用修补程序，并在部署之前对其进行测试。

**要在本地开发环境中应用单个修补程序**：

1. 将“QUALITY_PATCH”变量添加到`.magento.env.yaml`文件中，并在下面列出所需的修补程序。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. 从项目根目录应用修补程序。

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   `ece-patches apply`命令按以下顺序应用修补程序：
   - 所需的修补程序
   - 可选的单个修补程序
   - 来自`/m2-hotfixes`目录的自定义修补程序

1. 清除缓存。

   ```bash
   php ./bin/magento cache:clean
   ```

1. 测试修补程序，对自定义修补程序进行任何必要的更改。

## 在远程环境中应用修补程序

>[!WARNING]
>
>我们强烈建议先测试集成或暂存环境中的所有修补程序，然后再部署到生产环境。

**要在远程环境中应用修补程序**：

1. 将`QUALITY_PATCHES`变量添加到`.magento.env.yaml`文件中并在下面列出所需的修补程序。

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >升级到Adobe Commerce的新版本后，如果修补程序未包含在新版本中，则必须重新应用修补程序。

1. 添加、提交和推送更新的`.magento.env.yaml`文件。

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## 应用自定义修补程序

部署时，ECE-Tools应用所有Adobe修补程序以及添加到项目根目录`/m2-hotfixes`目录的任何自定义修补程序。

>[!NOTE]
>
>所有修补程序文件名必须以`.patch`扩展名结尾。

**要在云环境中应用和测试自定义修补程序**：

1. 在项目根目录下，创建一个名为`m2-hotfixes`的目录（如果该目录不存在）

   ```bash
   mkdir m2-hotfixes
   ```

1. 将修补程序文件复制到`/m2-hotfixes`目录。

1. 添加、提交和推送代码更改。

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >请确保在预生产环境中测试所有修补程序。 对于云基础架构上的Adobe Commerce，您可以使用`magento-cloud environment:branch <branch-name>` CLI命令创建分支。

## 还原自定义修补程序

要还原或卸载以前应用的自定义修补程序，请执行以下操作：

1. 从`/m2-hotfixes`目录中删除修补程序文件。

1. 添加、提交和推送代码更改。

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >确保在预生产环境中测试。 对于云基础架构上的Adobe Commerce，您可以使用`magento-cloud environment:branch <branch-name>` CLI命令创建分支。

## 将修补程序应用到非云项目

使用[Quality Patches Tool](https://github.com/magento/quality-patches)进行Magento Open Source和Adobe Commerce项目。

## 还原本地环境中的修补程序

您可以使用`ece-patches` CLI还原本地开发环境中先前应用的所有修补程序。

还原所有应用的修补程序：

```bash
php ./vendor/bin/ece-patches revert
```

此命令按以下顺序还原所有修补程序：

- 还原从/m2-hotfixes目录应用的所有自定义修补程序。
- 还原所有应用的可选单个修补程序。
- 还原所有应用的所需修补程序。

## 记录

质量修补程序工具将所有操作记录到`<Project_root>/var/log/patch.log`文件中。
