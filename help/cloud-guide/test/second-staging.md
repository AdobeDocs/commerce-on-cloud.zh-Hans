---
title: 第二个暂存环境
description: 了解第二个暂存环境用于并行测试、问题隔离、版本控制等的好处和用法。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# 第二个暂存环境

在Adobe Commerce Cloud基础架构中，暂存环境确保在可以镜像生产环境的设置中进行全面的测试和验证。 利用此环境，您可以安全地识别和解决Bug，同时确保在所有新功能或更改影响您的实时网站之前无缝集成。

有些项目需要更复杂的开发工作流。 为了满足此需求，Adobe提供了额外的暂存环境，作为云基础架构的附加选项。 拥有两个暂存环境对于复杂的项目和跨团队的同步协作很有利。

拥有辅助暂存环境具有以下优势：

- **并行测试**：使用两个暂存环境，团队可以同时测试新功能和关键更新，而不会干扰彼此的开发。 例如，您可以将环境专用于功能测试，而保留其他环境用于性能和压力测试。

- **问题隔离**：当主暂存环境中出现错误或问题时，您可以在辅助环境中复制和诊断它们，而不会中断测试进度。 这可确保故障排除不会干扰正在进行的测试。

- **版本控制**：更有效地管理应用程序的不同版本。 主暂存环境可以运行最新的稳定内部版本，而辅助测试则会进行实验性功能。 这有助于您更好地管理发布周期，并确保实验性更改不会破坏稳定的内部版本。

- **增强的转出计划**：您可以模拟不同的部署方案。 主暂存环境可以模拟当前生产设置，而辅助环境可以配置为测试即将发布的版本。

- **用户验收测试(UAT)**：在使用主环境进行持续开发时，您可以将辅助环境专用于UAT。 这允许用户或客户端在受控设置中测试新功能并提供反馈，而不影响测试。

- **回归测试**：您可以将一个环境用于正向测试，将另一个环境用于回归测试，从而确保新的更改不会破坏现有功能。

- **培训和演示**：使用一种环境培训新团队成员或向利益相关者演示新功能。 这可确保培训活动不会中断测试。

- **专用链接设置**：主环境和集成环境不支持专用链接设置。 如果您的开发工作流需要通过专用链接连接其他环境来进行开发、测试或任何其他目的，请考虑添加额外的暂存环境。

拥有两个暂存环境可以极大地提高您的工作流效率、风险管理和应用程序的总体质量。 这些环境提供了一个暂存环境无法提供的安全性和灵活性。

>[!NOTE]
>
>此安装程序是一个加载项。 需要辅助环境的客户必须联系其销售代表以请求辅助环境。 销售代表可以提供有关定价和配置过程的信息。

如果您的项目已经有一个额外的暂存环境，或者您考虑升级当前项目，请考虑以下预配时间表和责任：

- 向Adobe销售代表确认请求后，资源调配过程最长可能需要两周时间。

- 新的暂存环境仅与您当前的暂存环境和生产环境位于同一区域。

- 您必须更新集成或主环境，并指定您的辅助暂存环境将基于哪些环境。 无法从暂存或生产环境中复制辅助暂存环境。

- 收到新的暂存环境后，可以将其包含在开发流中。 与其他环境一样，代码和数据库更新由您负责。

## 请求环境

要简化请求，请执行以下步骤：

1. 升级您的分支。

   使用要用于新环境的代码和数据库升级集成或主分支。

1. 请联系您的销售代表。

   请联系您的Adobe销售代表，向他们提供要用作新暂存环境基础的特定分支（集成或主环境）。

1. 确认详细信息。

   在销售代表确认详细信息后，请等待销售代表确认已收到并正在处理配置请求。 配置过程完成后，Adobe团队将向您发送确认信息。

1. 部署到新环境。

   按照[标准部署步骤](../deploy/staging-production.md)将代码和数据库部署到新的暂存环境。
