---
title: 优化云部署
description: 了解在云基础架构项目中为Adobe Commerce优化部署过程的方法，包括减少停机时间、静态内容部署、基于方案的部署和智能向导。
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# 优化部署

在部署过程中，网站性能可能会受到影响。 部署到生产站点时，站点处于维护模式的时长取决于许多因素，例如环境配置和站点包含的内容量。 优化云部署的第一个最佳实践是[升级以使用`ece-tools`](../dev-tools/install-package.md)从包功能中获益，例如创建数据库备份并验证环境配置的命令。

以下主题可以帮助您更好地了解如何优化部署过程：

- [云部署进程](process.md)
云部署过程分为三个阶段，您可以利用每个阶段的优点和缺点来达到自己的优势。

- [零停机部署](reduce-downtime.md)
了解在部署期间发生的情况以及如何减少在对生产环境进行更新期间存储体验的停机时间。

- [静态内容部署](static-content.md)
优化云部署的最佳方法是控制如何以及何时生成静态内容。

- [智能向导](smart-wizards.md)
`ece-tools`包提供了智能向导命令以快速评估您的项目配置。

- [使用New Relic跟踪部署](../monitor/track-deployments.md)
使用New Relic服务监控部署事件并分析部署对整体性能的影响。
