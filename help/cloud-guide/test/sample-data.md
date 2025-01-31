---
title: 示例数据
description: 了解如何在云基础架构上使用Adobe Commerce安装示例数据。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 示例数据

如果您在开发存储时需要一些示例数据，则可以安装示例数据。 此数据模拟一个包含客户、产品和其他数据的活动Adobe Commerce存储。 此示例数据最适合用于安装新的Adobe Commerce on cloud infrastructure模板。

作为最佳实践，请在开发和集成环境中安装示例数据。 如果您在暂存或生产中使用示例数据，则必须[删除](#reset-or-uninstall-sample-data)信息和产品才能正式启用。

## 安装示例数据

要安装示例数据，请执行以下操作：

1. 在本地工作站上，转到您的项目目录。

1. 在根下，输入以下命令以添加示例数据：

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. 等待组件更新。

1. 提交并推送更改：

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 等待项目部署。

1. 通过转到集成环境中的店面页面，验证安装是否成功。 您可以通过[!DNL Cloud Console]找到店面的URL链接。

1. 拍摄环境的快照：

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

您可以使用实时数据开始测试开发！

## 重置或卸载示例数据

您可以按照与安装示例数据相同的过程重置或删除示例数据：

- 删除样本数据： `./bin/magento sampledata:remove`
- 重置样本数据： `./bin/magento sampledata:reset`
