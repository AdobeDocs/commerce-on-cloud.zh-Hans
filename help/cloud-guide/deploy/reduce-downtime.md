---
title: 零停机部署
description: 了解在云基础架构项目上部署Adobe Commerce时如何减少总体停机时间。
feature: Cloud, Deploy, SCD, Themes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 零停机部署

云基础架构上的Adobe Commerce在部署阶段以&#x200B;[_维护_&#x200B;模式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode)运行应用程序，该模式将使您的网站脱机，直到部署完成。 生产站点处于维护模式的时长取决于站点的大小、部署期间应用的更改数以及静态内容部署的配置。 可以配置您的项目，使其部署时具有&#x200B;**零**&#x200B;停机影响。

在部署过程中，所有连接将排队长达5分钟，以保留任何活动会话和待定操作，例如添加到购物车或结帐。 部署后，队列将释放，连接将继续而不会中断。 若要使用此&#x200B;_连接保持_&#x200B;的优势并将部署减少到&#x200B;_零_&#x200B;停机时间，必须将项目配置为使用最有效的部署策略。

使用以下步骤可减少存储将更新部署到生产环境所需的时间：

1. [升级到`ece-tools`包](../dev-tools/install-package.md)或[更新`ece-tools`版本](../dev-tools/update-package.md)
您的Adobe Commerce on cloud infrastructure项目必须具有最新的`ece-tools`包，以便您有可用的工具来配置最佳部署。 如果您有最新的`ece-tools`，请继续执行下一步。

   >[!NOTE]
   >
   >尽管最佳做法是使用最新的`ece-tools`包，但零停机部署方法可与`ece-tools` [版本2002.0.13](../release-notes/cloud-release-archive.md#v2002013)及更高版本配合使用。

1. [配置静态内容部署](static-content.md)
如果静态内容部署在部署阶段失败，则网站将卡在维护模式下。 当在构建阶段发生故障时，该进程可避免停机，因为它从不开始部署阶段。 [在构建阶段使用缩小的HTML生成静态内容](static-content.md#setting-the-scd-on-build)（也称为理想状态）是零停机部署的最佳配置，如果发生故障，_可防止_&#x200B;停机。

1. [配置部署后挂接](../application/hooks-property.md)
必须配置部署后挂接以清理和预热缓存。 默认情况下，当站点关闭时，会在部署阶段进行缓存清理。 将缓存清理到部署后阶段意味着您的缓存会一直保持活动状态，直到部署阶段结束，然后您就可以安全地清理缓存。

   使用[WARM_UP_PAGES环境变量](../environment/variables-post-deploy.md#warmuppages)自定义用于预加载缓存的页面列表。

1. [减少主题文件](../environment/variables-deploy.md#scdmatrix)
通过配置SCD\_MATRIX环境变量，可以减少不必要的主题文件的数量。

1. [加速静态内容部署](../environment/variables-deploy.md#scdthreads)
您可以通过更新SCD\_THREADS环境变量来增加静态内容部署的线程数，从而加快部署过程。

>[!NOTE]
>
>您可以通过[运行理想状态向导](smart-wizards.md#verifying-an-ideal-configuration)来验证项目配置以获得最佳部署。
