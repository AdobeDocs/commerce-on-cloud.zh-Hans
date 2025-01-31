---
title: 入门项目工作流
description: 了解如何使用入门开发和部署工作流。
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '2103'
ht-degree: 0%

---

# 入门项目工作流

云基础架构上的Adobe Commerce包括用于生产环境的具有一个`master`分支的单个Git存储库，可以分支该存储库以创建用于测试和开发工作的暂存和多个集成环境。 您最多可以有四个活动环境，包括用于生产服务器的`master`环境。 请参阅[入门体系结构](starter-architecture.md)以了解概览。

对于您的环境，请按照[!UICONTROL Development > Staging > Production]工作流来开发和部署您的网站。

- **生产环境（实时站点）** — 提供具有从`master`分支上的代码生成和部署的所有服务的完整生产环境。
- **暂存环境** — 提供与生产环境匹配的完整暂存环境，该环境具有从`staging`分支生成和部署的所有服务，该分支是您从`master`克隆而创建的。
- **集成环境** — 提供最多两个您从`staging`分支创建的活动开发环境。 `integration`环境不支持Fastly和New Relic等第三方服务。

对于分支，您可以遵循任何开发方法。 例如，您可以遵循敏捷方法（如Scrum）为每个冲刺创建分支。

从每个冲刺中，可为每个用户故事创建分支。 所有的故事都变得可以检验。 您可以不断合并到冲刺分支，并连续验证该分支。 当冲刺结束时，您可以将冲刺分支合并到`master`以将所有冲刺更改部署到生产环境，而无需处理测试瓶颈。

## 开发工作流

Starter计划的开发和部署从您的初始项目开始。 您可以使用“空白站点”创建项目，该站点是一个云基础架构模板上的Adobe Commerce代码存储库，具有完全准备好的存储。 这将使用生产环境中的代码副本创建一个`master`分支。

开发工作流包括以下内容：

- 从`master`中[克隆和分支](#clone-and-branch)以创建`staging`和开发分支
- [开发代码](#develop-code)并在开发分支中本地安装扩展，包括[!DNL Composer]更新
- [配置](#configure-store)您的商店和扩展设置
- [生成配置](#generate-configuration-management-files)管理文件
- [推送代码](#push-code-and-test)和配置以生成并部署到`staging`和`production`环境

![开发和部署工作流](../../assets/starter/workflow.png)

此外，还有一些可选步骤可帮助开发和测试代码和存储数据：

- [将示例数据](#optional-install-sample-data)安装到您的存储
- [将生产存储区数据](#optional-pull-production-data)拉入环境

此过程假定您已设置[本地开发人员工作区](../development/overview.md)。

### 克隆和分支

对于新的入门计划项目，从云基础架构Git存储库上的Adobe Commerce克隆了`master`分支。 要开始分支和使用代码，请将`master`分支克隆到本地环境。

Git clone命令的格式为：

```bash
git fetch origin
```

```bash
git pull origin <environment-ID>
```

首次开始在入门项目的分支中工作时，请创建`staging`分支。 这将创建一个与部署到暂存环境的`master`分支匹配的代码分支，以在部署到生产环境之前测试配置和代码更改。

接下来，从`staging`创建分支以开发代码、添加扩展并配置第三方集成。 无论您何时开发自定义代码、添加扩展、与第三方服务集成，都可以在从`staging`分支创建的开发分支中工作。 您有四个可用的活动集成环境。 当您推送活动分支时，其中一个集成环境会自动部署您的代码以进行测试。

Git branch命令的格式为：

```bash
git checkout <branch-name>
```

Cloud CLI `branch`命令的格式为：

```bash
magento-cloud environment:branch <environment-name> <parent-environment-ID>
```

从母版![分支](../../assets/starter/branching.png)

### 开发代码

使用Adobe Commerce在云基础架构代码上的基础分支，您可以开始安装扩展、开发自定义代码、添加主题等。

在开发工作中使用分支策略。 使用一个分支同时完成所有工作可能会使测试变得困难。 例如，您可以遵循持续集成和sprint方法来工作：

- 添加一些扩展并在第一个分支中配置它们
- 推送此代码，测试并合并到暂存环境，然后再推送至生产环境
- 在`services.yaml`中完全配置您的服务并添加主题
- 推送此代码，测试并合并到暂存环境，然后再推送至生产环境
- 与第三方服务集成
- 推送此代码，测试并合并到暂存环境，然后再推送至生产环境

在您完全构建、配置并准备启动存储之前。 但请继续阅读，您的存储和代码配置有许多选项。

>[!NOTE]
>
>不要在本地工作站上完成任何配置。

![从本地推送代码](../../assets/starter/push-code.png)

### 配置存储

准备好配置存储区后，将所有代码推送到`integration`环境。 从管理员中为集成环境配置商店设置，而不是在本地环境中配置。 您可以通过单击[!DNL Cloud Console]中的&#x200B;**访问网站**&#x200B;来查找该URL

有关配置的最佳信息，请查看Adobe Commerce文档和已安装的扩展。 以下是帮助您入门的一些链接和想法：

- 针对云中的特定最佳实践，[存储配置的最佳实践](../store/best-practices.md)
- [商店管理员访问权限、名称、语言、货币、品牌、网站、商店视图等的](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/store-details)基本配置
- [主题](https://experienceleague.adobe.com/en/docs/commerce-admin/content-design/content-menu#design-features)，用于显示您的网站和存储（包括CSS和布局）的外观
- [系统配置](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/guide-overview)，用于角色、工具、通知和数据库的加密密钥
- 扩展设置使用其文档

除了商店设置之外，您还可以进一步配置多个站点和商店、配置的服务等。 请参阅[配置您的商店](../store/overview.md)。

### 生成配置管理文件

如果您熟悉Adobe Commerce，则可能会担心如何从正在开发的数据库获取到暂存环境和生产环境的配置设置。 以前，您必须将所有配置设置复制到纸张或文件中，然后手动将这些设置应用于其他环境。 或者，您可能已转储数据库并将该数据推送到其他环境。

云基础架构上的Adobe Commerce提供了一组两个[配置管理](../store/store-settings.md)命令，可将配置设置从您的环境导出到文件中。 这些命令仅适用于Cloud Infrastructure 2.2及更高版本上的&#x200B;**Adobe Commerce**。

- `php .vendor/bin/ece-tools config:dump` — 仅将您输入或修改的配置设置导出到配置文件中。 _推荐_。
- `php bin/magento app:config:dump` — 将每个配置设置（包括已修改和默认设置）导出到配置文件中。

生成的文件是`app/etc/config.php`。

在配置了Adobe Commerce的集成环境中生成文件。 逐步完成生成文件、将其添加到分支和部署文件的过程。

有关配置管理的&#x200B;**重要说明**：

- 通过`app:config:dump`命令生成的文件中包含的任何配置设置都将被锁定，无法在已部署的环境中进行编辑或只读。 这是Adobe建议使用`.vendor/bin/ece-tools config:dump`命令的原因之一。

  例如，在开发环境中为Fastly安装模块。 您只能在暂存和生产环境中配置此模块。 在将开发更改部署到暂存和生产环境时，使用`.vendor/bin/ece-tools config:dump`命令可保持这些默认字段可编辑。

- 根据部署的大小，生成的文件可能会很长。 `.vendor/bin/ece-tools config:dump`命令生成的文件比`app:config:dump`命令生成的文件小。

如果您使用的是Adobe Commerce版本2.2或更高版本，则配置管理命令提供了额外的功能来保护敏感数据，例如PayPal模块的沙盒凭据。 在导出过程中，任何包含敏感数据的值都会导出到`app/etc/`目录中的单独配置文件 — `env.php`。 此文件将保留在您的本地环境中，并且不会在您将代码推送到其他分支时复制。 您还可以在云基础架构版本的所有Adobe Commerce中使用CLI命令创建环境变量。

![环境变量生成](../../assets/starter/env-variables.png)

请参阅[配置管理](../store/store-settings.md)。

### 推送代码和测试

此时，您应该具有已开发的代码分支，其中配置文件（`config.local.php`或`config.php`）已准备好测试。

每次从本地环境推送代码时，都会运行一系列构建和部署脚本。 这些脚本可生成新代码并将其部署到远程环境。 例如，如果您将开发分支从本地环境推送到远程分支，则匹配的环境会更新服务、代码和静态内容。

您可以使用商店URL、管理员URL和SSH直接访问此环境。 这些环境包括Web服务器、数据库和配置的服务。 准备就绪后，您可以在暂存环境中开始部署和测试。

有关详细信息，请参阅[部署工作流](#deployment-workflow)。

### 可选：安装示例数据

如果您在开发存储时需要一些示例数据，则可以安装示例数据。 此数据模拟活动存储，包括客户、产品和其他数据。 在创建项目时，此示例数据最适合用于云基础架构模板安装中的“空白站点”Adobe Commerce。 作为最佳实践，请在上线前删除示例数据。 请参阅[安装可选示例数据](../test/sample-data.md)。

![安装可选示例数据](../../assets/starter/sample-data.png)

### 可选：拉取生产数据

将您的所有产品、目录、网站内容等直接添加到`production`环境。 通过将此数据添加到生产环境，您可以为客户提供更新的价格、优惠券、库存库存、销售公告、有关未来产品的信息等。 此数据不包括扩展配置，您可在本地开发分支中配置这些配置。

在开发功能、添加扩展和设计主题时，使用真实数据会很有帮助。 您可以随时[从生产环境创建数据库转储](../storage/database-dump.md)，并根据需要将其推送到暂存和集成环境。

要帮助将生产数据导出为测试数据，以便在暂存和集成环境中使用，请执行以下操作：

- [使用Adobe Commerce加密密钥导出客户的受保护备份并存储数据时，运行支持实用程序](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) CLI命令（推荐）

- 用于生成和导出数据的[数据收集](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/support#data-collector)工具

要迁移此数据，请参阅[迁移和部署静态文件和数据](../deploy/staging-production.md#migrate-static-files)。

![提取并整理生产数据](../../assets/starter/data-code-process.png)

>[!NOTE]
>
>将数据推送到其他环境之前，应考虑清理您的数据。 您有几个选项，包括[使用支持实用程序](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html)或开发脚本以清除客户数据。

>[!WARNING]
>
>请勿将数据库从集成或暂存环境推送到生产环境。 如果这样做，则集成或暂存环境中的数据将覆盖实时生产数据，包括销售、订单、新客户和更新客户等。

## 部署工作流

如架构信息中详述的那样，云基础架构上的Adobe Commerce是Git驱动的。 在云基础架构上部署Adobe Commerce是分支机构的Git推送流程的一部分。

将分支代码从本地环境推送到远程分支时，将开始一系列构建和部署脚本。

生成脚本：

- 在构建期间，目标环境中的站点将继续运行

- 在云基础架构修补程序和修补程序上检查并运行Adobe Commerce

- 使用生成和部署日志编译代码

- 如果在此阶段进行静态内容部署，请检查配置管理

- 创建或使用未更改代码的概要，以加快进程

- 配置所有后端服务和应用程序

部署脚本：

- 将您的站点置于处于维护模式的目标环境中

- 如果在生成期间未完成，则部署静态内容

- 在云基础架构上安装或更新Adobe Commerce

- 为流量配置路由

完全完成后，您的商店将恢复为在线状态，并包含所有更新的代码和配置。

请参阅[部署进程](../deploy/process.md)。

### 推送到暂存和测试

始终将迭代中的代码推送到`staging`环境以进行完全测试。 首次使用此环境时，必须配置一些服务，包括[Fastly](/help/cloud-guide/cdn/fastly.md)和[New Relic](../monitor/new-relic-service.md)。 此外，使用沙盒或测试凭据配置支付网关、运送、通知和其他重要服务。

暂存是一种预生产环境，可提供尽可能接近生产环境的所有服务和设置。 彻底测试每项服务，验证您的性能测试工具，以管理员和客户的身份执行UAT测试，直到您认为您的商店已准备好投入生产。

查看[部署您的商店](../deploy/staging-production.md)。

### 推送到生产

推送到`master`分支时，您正推送到`production`环境。 像在暂存环境中一样，在生产环境中完成配置和测试活动，但有一个重要区别。 在生产环境中，使用Live凭据进行配置和测试。 一旦您启动网站，客户即可完成购买，管理员即可管理实时商店。

查看[部署您的商店](../deploy/staging-production.md)。

### 站点启动

对于与您的网站一起上线，有一个明确的演练。 完成这些步骤后，您的商店可以立即提供自定义主题中的产品以供销售。

查看[站点启动项](../launch/overview.md)。

## 持续集成

按照您的分支和开发方法，您可以轻松地开发新功能、配置更改并添加扩展以持续开发和部署更新。

所有云基础架构环境都支持持续集成，以便不断更新。 此工作流可支持一天发布多次，或根据业务需求按预定计划发布。

- 创建具有未来功能和更改的开发分支

- 在`integration`环境中测试代码

- 在`staging`环境中部署和测试

- 部署到`production`环境
