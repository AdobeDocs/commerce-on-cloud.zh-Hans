---
title: 部署最佳实践
description: 探索在云基础架构上部署Adobe Commerce的最佳实践。
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# 部署最佳实践

将代码合并到远程环境时，生成和部署脚本会激活。 这些脚本使用环境[配置文件](../environment/overview.md)和应用程序代码来配置云基础架构和相应的数据和服务。 此外，这些脚本还用于在云环境中安装或更新Adobe Commerce应用程序、第三方服务和自定义扩展。

每个计划的构建和部署过程略有不同：

- **入门计划** — 对于集成环境，每个活动分支都构建并部署到完整环境以进行访问和测试。 合并到`staging`分支后对代码进行全面测试。 要启动您的站点，请将`staging`推送到`master`以部署到生产环境。 通过[!DNL Cloud Console]和CLI命令，您可以完全访问所有分支。

- **专业计划** — 对于集成环境，每个活动分支都构建并部署到完整环境，以进行访问和测试。 将您的代码合并到`integration`分支，然后再合并到暂存环境和生产环境。 您可以使用[!DNL Cloud Console]或使用SSH和`magento-cloud` CLI命令合并到暂存环境和生产环境。

## 跟踪进程

您可以使用终端或在部署过程中显示的[!DNL Cloud Console]状态消息（`in-progress`、`pending`、`success`或`failed`）实时跟踪生成和部署操作。 您可以在日志文件中查看详细信息。 查看[查看日志](../test/log-locations.md)。

如果您使用外部GitHub存储库，则操作日志不会显示在GitHub会话中。 但是，您仍然可以关注外部存储库和[!DNL Cloud Console]的界面中的活动。 查看[集成](../integrations/overview.md)。

>[!NOTE]
>
>在集成环境中，您无法从[!DNL Cloud Console]查看部署日志。 此功能仅适用于生产和暂存环境。 但是，您可以使用[生成和部署](../test/log-locations.md#build-and-deploy-logs)日志在任何环境中查看部署的每个阶段的日志。 有关疑难解答信息，请参阅[部署错误引用](../dev-tools/error-reference.md)。

您可以使用New Relic[&#128279;](../monitor/track-deployments.md)启用跟踪部署，以监视部署事件并分析部署之间的性能。

## 构建和部署的最佳实践

请查看部署过程中的以下最佳实践和注意事项：

- **请确保您运行的是`ece-tools`包的最新版本**

  请参阅[ECE-Tools的发行说明](../release-notes/ece-tools-package.md)。

- **遵循生成和部署流程**

  确保您在每个环境中都拥有正确的代码，以避免在环境之间合并代码时覆盖配置。 例如，要将配置更改应用于所有环境，请在部署到远程集成环境之前修改并测试本地环境中的更改。 然后，在部署到生产环境之前，在暂存环境中部署和测试更改。 从一个环境合并到另一个环境时，部署将覆盖环境中所有代码，特定于环境的配置和设置除外。

- **跨环境使用相同的变量**

  这些变量的值可能因环境而异；但是，您通常需要在每个环境中使用相同的变量。 查看存储设置的[配置管理](../store/store-settings.md)。

- **将敏感配置值和数据保留在特定于环境的变量中**

  这些值包括使用Cloud CLI指定的变量、[!DNL Cloud Console]或添加到`env.php`文件中的变量。 请参阅[变量级别](../environment/variable-levels.md)。

- **确保所有代码在环境分支中均可用**

  引用其他分支的代码（如专用分支）可能会在构建和部署过程中导致问题。 例如，如果您从专用分支引用主题，则无法访问该主题，也无法使用应用程序代码构建该主题。

- **在迭代的分支中添加扩展、集成和代码**

  在本地进行并测试更改，推送到`integration`，然后推送到`staging`和`production`。 在将更新合并到下一个环境之前，测试和解决每个环境中的问题。 由于依赖关系，某些扩展和集成必须按特定顺序启用和配置。 在组中添加和测试可以更轻松地执行生成和部署过程，并有助于确定出现问题的位置。

- **验证服务版本和关系以及连接能力**

  验证应用程序可用的服务，并确保您使用的是最新的兼容版本。 有关推荐的版本，请参阅&#x200B;_安装指南_&#x200B;中的[服务关系](../services/services-yaml.md#service-relationships)和[系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=zh-Hans)。

- **在部署到暂存和生产环境之前，在本地和集成环境中进行测试**

  识别并修复本地环境和集成环境中的问题，以防止在部署到暂存环境和生产环境时延长停机时间。

  >[!TIP]
  >
  >有[个智能向导](../deploy/smart-wizards.md)命令可用于验证您的云项目配置是否遵循生成和部署配置(包括静态内容部署(SCD)策略)的最佳实践。

- **在本地环境和集成环境中完成测试后，在暂存环境中部署和测试**

  请参阅[暂存和生产测试](../test/staging-and-production.md)。

- **检查生产环境配置**

  在部署到生产环境之前，请完成以下任务：

   - 确保您可以使用[SSH](../development/secure-connections.md)连接到生产环境中的所有三个节点。

   - 验证索引器是否设置为计划&#x200B;_上的_&#x200B;更新。 请参阅&#x200B;_扩展开发人员指南_&#x200B;中的[索引模式](https://developer.adobe.com/commerce/php/development/components/indexing/)。

   - 通过更新生产代码中的任何特定于环境的变量、验证服务可用性和兼容性并进行任何其他所需的配置更改来准备环境。

- **监视部署进程**

  查看部署状态消息并根据需要缓解问题。 查看云[日志](../test/log-locations.md#)以了解详细的日志消息。

## 集成构建和部署的五个阶段

在本地开发环境和集成环境中将执行以下阶段。 对于Pro计划，在这些初始阶段中，代码不会部署到暂存或生产环境。

### 阶段1：代码和配置验证

最初设置项目时，[云基础架构模板](https://github.com/magento/magento-cloud)为代码文件提供了基础。 此代码存储库将作为`master`分支克隆到您的项目中。

- **对于入门版**—`master`分支是您的生产环境。
- Pro **的**—`master`作为集成环境的原始分支开始。

从`master`为您的自定义代码、扩展和模块以及第三方集成创建分支。 有一个用于在云中测试代码的远程集成环境。

将代码从本地工作区推送到远程存储库时，在开始构建和部署脚本之前会完成一系列检查和代码验证。 内置Git服务器会验证您正在推送的内容并进行更改。 例如，如果添加OpenSearch服务，内置的Git服务器将验证是否相应地修改了集群的拓扑。

如果配置文件中出现语法错误，Git服务器会拒绝推送。 请参阅[保护块](../development/protective-block.md)。

此阶段还运行`composer install`以检索依赖关系。

### 第2阶段：构建

>[!NOTE]
>
>在构建阶段，站点未处于维护模式，如果发生错误或问题，则不会停机。 它仅构建自上次构建以来发生更改的代码。

此阶段构建代码库并在`.magento.app.yaml`的`build`部分中运行挂接。 默认的生成挂接是`php ./vendor/bin/ece-tools`命令，并执行以下操作：

- 在`vendor/magento/ece-patches`中应用修补程序，在`m2-hotfixes`中应用项目特定的可选修补程序
- 使用`bin/magento setup:di:compile`重新生成代码和[依赖项注入](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/glossary)配置（即`generated/`目录，包括`generated/code`和`generated/metapackage`）。
- 检查代码库中是否存在[`app/etc/config.php`](../store/store-settings.md)文件。 如果Adobe Commerce在构建阶段未检测到此文件并包含模块和扩展名的列表，则会自动生成此文件。 如果存在，则构建阶段将照常继续，使用GZIP压缩静态文件并进行部署，从而减少部署阶段的停机时间。 请参阅[生成选项](../environment/variables-build.md)，了解有关自定义或禁用文件压缩的信息。

>[!WARNING]
>
>此时，尚未创建群集，因此请勿尝试连接到数据库或假定存在活动的守护程序进程。

应用程序生成后，它将装载到&#x200B;**只读文件系统**&#x200B;上。 您可以配置将要读/写的特定装载点。 您无法通过FTP连接到服务器并添加模块。 相反，您必须将代码添加到本地存储库并运行`git push`，以构建和部署环境。 有关项目结构，请参阅[本地项目目录结构](../project/file-structure.md)。

### 阶段3：准备概要

生成阶段的结果是称为&#x200B;_概要_&#x200B;的只读文件系统。 此阶段将创建一个归档文件，并将该概要文件置于永久存储中。 下次推送代码时，如果服务未发生更改，则它会使用存档中的概要。

- 通过重用未更改的代码，加快持续集成的构建速度
- 如果代码发生更改，会更新概要文件以供下次构建重用
- 如果需要，允许即时恢复部署
- 如果`app/etc/config.php`文件存在于代码库中，则包含静态文件

概要文件包括所有文件和文件夹&#x200B;**，不包括在`magento.app.yaml`中配置的以下**&#x200B;个装载：

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### 阶段4：部署Slug和群集

您的应用程序和所有[后端](https://experienceleague.adobe.com/zh-hans/docs/commerce-operations/implementation-playbook/glossary)服务预配，如下所示：

- 将每个服务挂载到容器中，如Web服务器、OpenSearch、[!DNL RabbitMQ]
- 装载读写文件系统（装载在高可用分布式存储网格上）
- 配置网络，使服务可以“看到”彼此（并且只能看到彼此）

>[!NOTE]
>
>在所有构建和部署完成后在Git分支中进行更改并再次推送。 所有环境文件系统均为&#x200B;_只读_。 只读系统可保证确定性部署，并显着提升网站安全性，因为没有任何进程可以写入文件系统。 它还可以确保您的代码在集成、暂存和生产环境中均相同。

### 阶段5：部署挂接

>[!NOTE]
>
>此阶段将[!DNL Commerce]应用程序置于维护模式，直到部署完成。

最后一步将运行部署脚本，使用该脚本可对开发环境中的数据进行匿名处理、清除缓存以及查询外部持续集成工具。 当此脚本运行时，您可以访问环境中的所有服务，例如Redis。

如果`app/etc/config.php`文件在代码库中不存在，则使用`gzip`压缩静态文件并在此阶段部署，这会增加部署阶段和站点维护的长度。

>[!NOTE]
>
>请参阅[部署变量](../environment/variables-deploy.md)，了解有关自定义或禁用文件压缩的信息。

有两个部署挂钩。 `pre-deploy.php`挂接完成对生成挂接中生成的资源和代码进行必要的清理和检索。 `php ./vendor/bin/ece-tools deploy`挂接运行一系列命令和脚本：

- 如果Adobe Commerce是&#x200B;**未安装**，则它随`bin/magento setup:install`一起安装，更新部署配置`app/etc/env.php`以及指定环境（如Redis和网站URL）的数据库。 **重要信息：**&#x200B;当您在安装程序期间完成[首次部署](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html?lang=zh-Hans)时，在所有环境中都安装和部署了Adobe Commerce。

- 如果已安装Adobe Commerce **&#x200B;**，请执行任何必要的升级。 部署脚本运行`bin/magento setup:upgrade`以更新数据库架构和数据（在扩展或核心代码更新后必需），并更新环境的部署配置、`app/etc/env.php`和数据库。 最后，部署脚本清除Adobe Commerce缓存。

- 脚本可以选择使用命令`magento setup:static-content:deploy`生成静态Web内容。

- 为静态内容部署策略使用默认设置为`quick`的作用域（生成脚本中的`-s`标志）。 您可以使用环境变量[`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy)自定义策略。 有关这些选项和功能的详细信息，请参阅[部署静态视图文件](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=zh-Hans)的[静态文件部署策略](../deploy/static-content.md)和`-s`标志。

>[!NOTE]
>
>部署脚本使用`.magento`目录中的配置文件定义的值，然后脚本删除该目录及其内容。 您的本地开发环境不受影响。

### 部署后：配置路由

在部署运行时，该进程将在入口点暂停传入流量60秒，并重新配置路由，以便您的Web流量到达新创建的群集。

成功部署将删除维护模式以允许正常访问，并为`app/etc/env.php`和`app/etc/config.php`配置文件创建备份(BAK)文件。

使用`SCD_ON_DEMAND`变量启用静态内容生成并配置[`post_deploy`挂接](../application/hooks-property.md)，以便它清除缓存并在&#x200B;_之后预加载(warms)缓存_，容器开始接受连接并在&#x200B;_正常传入流量期间接受_。

要查看生成和部署日志，请参阅[查看日志](../test/log-locations.md#view-and-manage-logs)。
