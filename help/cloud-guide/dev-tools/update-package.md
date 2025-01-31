---
title: 更新ECE工具包
description: 了解如何升级ECE-Tools包，以利用在云基础架构上应用于Adobe Commerce的最新修复和功能。
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 更新ECE工具包

`ece-tools`包的更新还更新了Commerce包的其他[Cloud Tools Suite](../release-notes/cloud-tools-suite.md)，它们是`ece-tools`的依赖项。 因此，您必须在支持`ece-tools`包的云基础架构上使用某个版本的Adobe Commerce。

{{ece-tools-package}}

**先决条件**：

- 在更新`ece-tools`之前，请查看[Cloud Tools Suite for Commerce发行说明](../release-notes/cloud-tools-suite.md)。
- 如果您是从`ece-tools` 2002.0.22或更早版本更新到2002.1.0，请查看[向后不兼容的更改](../release-notes/backward-incompatible-changes.md)，并对云基础架构项目上的Adobe Commerce进行任何必需的更改。
- 查看[升级和修补程序](../development/commerce-version.md#upgrade-from-older-versions)，以确定与云基础架构项目上的Adobe Commerce兼容的ECE-Tools版本。

{{upgrade-tip}}

**要更新`ece-tools`包**：

1. 在本地工作站上，使用Composer执行更新。

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >如果不能更新超过`ece-tools`版本2002.0.8，请参阅[升级项目以使用ECE-Tools包](install-package.md)。

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 测试验证后，将此分支合并到集成分支。
