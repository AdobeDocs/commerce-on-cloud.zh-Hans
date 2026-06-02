---
title: 自定义主题
description: 了解如何在云基础架构上安装Adobe Commerce的自定义主题。
feature: Cloud, Themes
exl-id: 3ae4b0d5-9179-42c4-bb07-8ec09bd057d0
TQID: https://experienceleague.adobe.com/rk-VP6z1tQSY-HMU-dD9hv6O9Wpesbp1o5KQMYapCJE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 296
ht-degree: 0%

---

# 自定义主题

您可以安装一个或多个主题以用于项目中的一个或多个商店和网站。 主题包括多个静态文件（包括图像、字体、CSS、JavaScript、PHP等），以完全设计您的商店。 您可以通过提取主题代码到文件系统或使用编辑器来添加主题。

## 手动安装主题

要手动安装主题，主题代码必须位于压缩存档中或类似于以下内容的目录结构中：

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**手动安装主题**：

1. 复制店面主题的`<Project root dir>/app/design/frontend`下的主题代码或管理主题的`<Project root dir>/app/design/adminhtml`。 验证顶层目录是否为`<VendorName>`；否则，该主题安装不正确。

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 确认将主题复制到正确位置。

   * 店面主题： `ls <project-root>/app/design/frontend`
   * 管理主题： `ls <project-root>/app/design/adminhtml`

   下面是一个示例：

   示例主题Adobe Commerce

1. 添加和提交文件。

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. 将文件推送到您的分支。

   ```bash
   git push origin <branch name>
   ```

1. 等待部署完成。
1. 登录到管理员。
1. 单击&#x200B;**内容** >设计> **主题**。

   主题显示在右侧窗格中。

## 使用编辑器安装主题

使用Composer安装主题与使用Composer安装任何其他扩展相同。 有关详细信息，请参阅[安装、管理和升级模块](extensions.md)。

要使用编辑器安装主题，请执行以下操作：

1. 从Commerce Marketplace购买主题。
1. 获取主题的编辑器名称。
1. 转到Adobe Commerce根目录并输入命令：

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   例如，

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. 等待依赖项更新。
1. 输入以下命令：

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. 登录到管理员。
1. 单击&#x200B;**内容** >设计> **主题**。

   主题显示在右侧窗格中。

## 多个主题

使用多个主题时（如每种区域设置使用不同的主题），请查看`SCD_MATRIX`环境变量以自定义主题部署。 查看[环境配置](../environment/configure-env-yaml.md)中的[生成](../environment/variables-build.md#scd_matrix)或[部署](../environment/variables-deploy.md#scd_matrix)阶段。
