---
title: 升级项目以使用ECE工具
description: 了解如何升级云基础架构项目上的Adobe Commerce以使用ECE-Tools包并利用最新的修复和功能。
feature: Cloud, Install
exl-id: 164c47e4-c871-41a3-b268-581d426e7a7f
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# 升级项目以使用ECE-Tools包

Adobe已弃用`magento/magento-cloud-configuration`和`magento/ece-patches`包而支持`ece-tools`包，这简化了许多云过程。 如果您在云基础架构项目上使用早期的Adobe Commerce，但&#x200B;_不_&#x200B;包含`ece-tools`包，则必须执行一次性的手动&#x200B;_升级_&#x200B;过程。

>[!WARNING]
>
>如果您的项目包含`ece-tools`包，则可以跳过以下升级。 要验证，请使用本地项目根目录中的`php vendor/bin/ece-tools -V`命令检索[!DNL Commerce]版本。

此项目升级过程要求您更新根目录`composer.json`文件中的`magento/magento-cloud-metapackage`版本约束。 此限制允许对Adobe Commerce进行云基础架构中继（包括删除已弃用的包）的更新，而无需升级您当前的Adobe Commerce版本。

{{upgrade-tip}}

## 移除已弃用的包

在执行升级以使用`ece-tools`包之前，请检查`composer.lock`文件中以下已弃用的包：

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## 更新中继包

每个Adobe Commerce版本都需要一个基于以下条件的限制：

```
>=current_version <next_version
```

- 对于`current_version`，指定要安装的Adobe Commerce版本。
- 对于`next_version`，在`current_version`中指定的值之后指定下一个修补程序版本。

如果要安装Adobe Commerce `2.3.5-p2`，请将`current_version`设置为`2.3.5`，将`next_version`设置为`2.3.6`。 约束`">=2.3.5 <2.3.6"`安装2.3.5的最新可用包。

您始终可以在[`magento-cloud`模板](https://github.com/magento/magento-cloud/blob/master/composer.json)中找到最新的中继包约束。

以下示例将Adobe Commerce对云基础架构的元包限制为大于或等于当前版本2.4.8且小于下一个版本2.4.9的任何版本：

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
},
```

## 升级项目

若要升级项目以使用`ece-tools`包，您必须更新中继包和`.magento.app.yaml`挂接属性，然后执行编辑器更新。

**若要将项目升级为使用ece-tools**：

1. 更新`composer.json`文件中的`magento/magento-cloud-metapackage`版本约束。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.8 <2.4.9" --no-update
   ```

1. 更新中继。

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. 修改`magento.app.yaml`文件中的挂接命令。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 检查并删除[已弃用的包](#remove-deprecated-packages)。 已弃用的包可能会阻止成功升级。

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. 可能需要更新`ece-tools`包。

   ```bash
   composer update magento/ece-tools
   ```

1. 添加并提交代码更改。 在此示例中，更新了以下文件：

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. 将代码更改推送到远程服务器，并将此分支与`integration`分支合并。

   ```bash
   git push origin <branch-name>
   ```
